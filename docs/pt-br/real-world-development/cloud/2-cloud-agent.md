---
title: "Exercício 2 - Agente GitHub Copilot na nuvem"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se houver alguma — não enfrentem dificuldades com dívida técnica. Pode ser a existência de problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram no backlog porque simplesmente não havia tempo para implementá-las. O agente GitHub Copilot na nuvem foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele cria um PR de rascunho para que um desenvolvedor humano o revise. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais abrangentes.

Neste exercício, você explorará como:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo bem definido.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar o site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo exiba jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Por enquanto, a equipe não precisa de uma implementação refinada; basta algo que possa ser usado para testes de aceitação da experiência. Isso é um bloqueio, mas outras issues têm prioridade maior no momento.

Esses dois exemplos ilustram tarefas que podem ser facilmente adiadas e que são ótimas candidatas para atribuir ao agente GitHub Copilot na nuvem. Ele pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se dedica a outras tarefas e, depois, voltar para revisar o trabalho e confirmar que está de acordo com o esperado.

## Apresentação do agente GitHub Copilot na nuvem

O [agente GitHub Copilot na nuvem](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a um desenvolvedor humano. Assim como no trabalho com outra pessoa desenvolvedora, há várias maneiras de iniciar uma tarefa, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot criará um pull request de rascunho para acompanhar o progresso, configurará um ambiente e começará a trabalhar. Você pode acompanhar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, ele marcará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — você não pode deixar de pensar. Até operações aparentemente simples ficam complexas quando analisadas em profundidade; por isso, [defina com cuidado o escopo das tarefas para o agente na nuvem][cloud-agent-best-practices]. Trate-o como um programador par de IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente na nuvem, leia os arquivos de instruções incluídos neste repositório sem alterá-los para que você possa observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub do seu repositório ou em um codespace, se já houver um aberto:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica essas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe se o pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos de comentário. Essas características vêm dos arquivos de instruções que você acabou de ler. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente na nuvem

Criar código, independentemente de quem esteja envolvido, normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [fluxo de trabalho especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas e dependências necessárias para o desenvolvimento. Esse fluxo já foi configurado para facilitar o laboratório e proporcionar essa experiência de aprendizagem. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se você não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro arquivo de fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente pela guia **Actions** do repositório. Isso é útil para testar o fluxo com êxito em vez de esperar que o Copilot o execute.

## Adicione a documentação

Embora todos entendam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue perfeita para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, você verá que o Copilot criou uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende concluir.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot está resolvendo o problema. Ele explora o código e entende o estado do projeto, pausa para pensar e decidir sobre o plano adequado e também cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente GitHub Copilot na nuvem é executar tarefas de forma assíncrona, liberando tempo para você se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já vimos, uma das grandes vantagens do agente GitHub Copilot na nuvem é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a inclusão de uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo ainda pode ser usado para outras tarefas. Vamos atribuí-la ao agente na nuvem.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá uma sequência de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando diligentemente na segunda solicitação. O agente na nuvem funciona de maneira semelhante a um engenheiro de software; portanto, você não precisa monitorá-lo continuamente. Basta revisar o trabalho quando ele estiver concluído. Vamos voltar nossa atenção para a criação e o uso de agentes personalizados.

## Resumo e próximas etapas

Este exercício apresentou o [agente GitHub Copilot na nuvem][copilot-agents]. Com o agente na nuvem, você pode atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Você pode usar o Copilot para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou como:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo bem definido.
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha, vamos nos concentrar em criar e usar agentes personalizados. O [agente GitHub Copilot na nuvem também pode usar servidores MCP][cloud-agent-mcp] e pode acessar instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente GitHub Copilot na nuvem][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do agente GitHub Copilot na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
