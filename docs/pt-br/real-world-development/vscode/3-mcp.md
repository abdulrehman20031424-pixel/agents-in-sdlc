---
title: "Exercício 3 - Testar seu recurso com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o **servidor Playwright MCP** e deixará que o Copilot controle um navegador real para testar o recurso — depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas,
- adicionar o servidor Playwright MCP ao workspace,
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem em um navegador,
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é a maneira como o Copilot faz isso: um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Isso permite acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP populares:

- **[Servidor GitHub MCP](https://github.com/github/github-mcp-server)**: oferece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. Ele permite que o agente de IA execute ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador por meio do Playwright. Ele permite que o agente de IA execute ações como acessar páginas Web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de recursos e contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no seu codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do template do projeto, e é por isso que o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor GitHub MCP remoto][remote-github-mcp-server], então não é preciso instalar nada localmente: o VS Code se conecta a ele por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor Playwright MCP

Agora você adicionará um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o servidor `playwright` é **local**: o VS Code o inicia no seu computador executando `npx @playwright/mcp@latest`. O sinalizador `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário dentro de um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta; portanto, normalmente o navegador necessário já está instalado. Se o Copilot informar mais tarde que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e escolha confiar nele para permitir que seja iniciado.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá que você confie nele na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste o recurso de filtragem

O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de você navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É a maneira mais rápida de confirmar se um recurso funciona como esperado sem sair da conversa.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Isso significa que o agente raciocina sobre elementos estruturados e rotulados (botões, links, itens de lista) da mesma maneira que as tecnologias assistivas — assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar o recurso de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor Playwright MCP, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça para corrigir o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que o recurso funciona, você está pronto para abrir um pull request (PR) para que a equipe o revise. O primeiro passo é publicar a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Mas, como você já está trabalhando com o GitHub Copilot, vamos deixar que ele crie o PR. Ele pode localizar a issue relevante e criar um PR associado a ela.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no seletor de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot execute as tarefas necessárias para coletar informações e realizar operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja solicitado que você se autentique no GitHub — siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link criado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas,
- adicionou o servidor Playwright MCP ao workspace,
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo,
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma maneira de ampliar os recursos do Copilot. Na próxima etapa, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é o MCP e por que todo mundo está falando dele?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
