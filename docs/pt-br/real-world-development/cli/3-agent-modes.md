---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para concordar com uma abordagem, Autopilot para criar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mas alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a criar as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará usando uma issue para criar um recurso, confirmará que o código está correto e que o recurso funciona como esperado e, por fim, mesclará o resultado ao projeto.

> [!NOTE]
> Você continuará usando a mesma conversa e branch durante o fluxo de trabalho do recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivos em que trabalharia, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar o recurso no modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como combinar categorias precisam ser acordados antes de escrever código. Você usará o modo Plan para decidir esses detalhes e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

A introdução de agentes de programação de IA ao fluxo de desenvolvimento não altera os fundamentos; na verdade, torna-os ainda mais importantes. A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Começar com uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria segue uma variação desse processo.

Ao manter essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de verificação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que agora o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite trazer uma issue ou um pull request (PR) do GitHub para o contexto.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem de implementação de um recurso ou da execução de tarefas antes de delegá-las ao Copilot. Para tarefas complexas, sempre vale a pena planejar um pouco. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador de modo abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a criar o plano. Primeiro, explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot para revisar qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere no problema até considerar que a tarefa está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com rótulo semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano acordado, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; portanto, revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, ele precisa ser revisado antes da mesclagem, como qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo parece correto.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diferenças.
5. Revise os resultados das verificações do projeto e peça ao Copilot para corrigir as falhas.

## Explore a nova funcionalidade

O código parece correto — mas funciona? Vamos pedir ao Copilot para iniciar o aplicativo, como fizemos antes, e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente filtrar por categoria, por editora e pelas combinações acordadas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot para parar o servidor de desenvolvimento iniciado por ele.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Na próxima etapa, vamos entender melhor como o código é gerado e como garantir que ele siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
