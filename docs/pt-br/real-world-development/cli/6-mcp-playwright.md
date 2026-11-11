---
title: "Lição 6 - Validar a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. Precisamos trabalhar com dados, serviços externos e até disponibilizar automações adicionais para o Copilot. É aí que entram os servidores MCP: eles permitem que o Copilot vá além dos recursos incluídos no CLI, oferecendo ainda mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor Playwright MCP, se ele ainda não estiver disponível.
- pedir ao agente para controlar um navegador e explorar seu recurso de filtragem.

## Cenário

Os testes unitários e de ponta a ponta são importantes, mas validar atualizações da interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária para automatizar ainda mais as alterações e aumentar a confiança de que elas funcionarão como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Isso permite acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP populares:

- **[Servidor GitHub MCP][github-mcp]**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: oferece recursos de automação de navegador por meio do Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de recursos e contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao seu Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP digitando:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para fechar a caixa de diálogo MCP.

## Peça ao Copilot para explorar o recurso com Playwright

Anteriormente, você confirmou manualmente que a funcionalidade estava funcionando como esperado. Agora vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo.

1. Use o seguinte prompt para pedir ao Copilot que valide a funcionalidade usando o servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, o Copilot consegue descobrir sozinho que deve usar o servidor MCP, então não é necessário especificá-lo. Mas, como você sabe qual servidor deve ser usado, indicá-lo pode ajudar a obter resultados mais consistentes e economizar alguns tokens.

2. Acompanhe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme que tudo está funcionando como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximas etapas

Parabéns, você usou o servidor Playwright MCP para explorar seu recurso em um navegador real a partir do Copilot CLI. Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor Playwright MCP, se ele ainda não estivesse disponível.
- pediu ao agente para controlar um navegador e explorar seu recurso de filtragem.

Na próxima etapa, você [criará um agente de QA personalizado][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é o MCP e por que todo mundo está falando dele?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
