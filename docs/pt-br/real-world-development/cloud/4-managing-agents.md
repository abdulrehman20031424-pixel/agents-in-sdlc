---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente GitHub Copilot na nuvem que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas disponíveis para gerenciar as tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse Mission Control, você vê todas as tarefas do agente que têm pull requests (PRs) abertos. Você pode explorar as operações realizadas e até orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente na nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe decidiu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar alternância entre modos, pareceu lógico incluir esse recurso. Você orientará o agente para garantir que ele adicione um modo claro além do alto contraste.

## Revise as tarefas do agente GitHub Copilot na nuvem

Vamos conferir o status atual de todas as tarefas atribuídas ao agente GitHub Copilot na nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações já executadas pelo Copilot. Elas podem incluir:
   - clonar o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel do pull request (PR), que aparece à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o agente na nuvem

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você perceberá isso quando começarem a aparecer mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente na nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o incorpora ao fluxo de trabalho.

## Deixe o Copilot trabalhar

Assim como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa específica que estava executando quando você enviou a mensagem.

Como antes, isso levará alguns minutos; aproveite esse tempo para refletir sobre tudo que aprendeu e explorou até agora.

## Resumo e próximas etapas

Este exercício apresentou a página de agentes do Copilot, o ponto central para monitorar e orientar as tarefas do agente GitHub Copilot na nuvem. Nesse Mission Control, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho realizado e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações.

Você explorou como:

- usar o Mission Control e a página de agentes para monitorar tarefas do agente na nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
