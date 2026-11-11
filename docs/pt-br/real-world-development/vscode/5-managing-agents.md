---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra todas as chamadas de ferramentas e edições de arquivos à medida que ocorrem. Você pode revisar cada comparação no editor, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar uma opção de modo claro junto com o alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz uma solicitação adicional: também quer uma opção de modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Monitore o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa executada pelo agente: os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição de arquivo aparece no chat com uma comparação e os controles **Keep** e **Undo**.

1. Volte à visualização do Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, a comparação aparecerá no chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para reverter a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todo o conjunto de arquivos que o agente modificou nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão — por exemplo, para executar um comando no terminal — o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você deverá ver uma mensagem final com o resumo das alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar uma opção de modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. Na entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorará as alterações já feitas e proporá adições para o modo claro.
3. Revise cada nova edição à medida que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada comparação.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das opções de alternância de uma maneira que você não queira — interrompa-o com o botão **Stop** ao lado da entrada do chat e envie uma mensagem complementar para esclarecer a solicitação.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira opção, integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas opções").

## Revise o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns arquivos alterados e percorra as diferenças. Confirme se as práticas de acessibilidade do agente personalizado estão presentes: atributos ARIA, navegação por teclado, HTML semântico e persistência por meio do armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações na visualização Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento, revisando cada edição de arquivo quando ela foi proposta, aceitando ou rejeitando alterações e enviando prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você explorou como:

- monitorar o trabalho de um agente em tempo real.
- aceitar e rejeitar edições individuais de arquivos.
- orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
