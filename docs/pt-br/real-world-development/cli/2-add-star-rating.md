---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revise-a em um navegador encaminhado e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e a mesclará no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente para fazer uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo na Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Porém, os cartões de jogos na página inicial exibem apenas título, categoria, editora e descrição. Como aquecimento, você pedirá ao agente para mostrar a avaliação existente em cada cartão — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade do agente com ferramentas enquanto explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para começar a implementar o recurso.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Solicite a alteração com este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI.

## Revise as diferenças

Todas as alterações geradas por IA devem ser revisadas antes da mesclagem, mesmo as pequenas. Vamos explorar as alterações no próprio Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão de jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça para corrigir quaisquer falhas.
5. Depois da revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico; portanto, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Confira as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot para iniciar o site e examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões de jogos mostram as avaliações de zero a cinco estrelas.
4. Volte ao Copilot e peça para parar o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Você criou o recurso. Agora é hora de abrir um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão para fazer commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximas etapas

Parabéns! Você publicou sua primeira alteração usando o GitHub Copilot CLI. Em particular, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente para fazer uma pequena alteração nos cartões de jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para conferir a avaliação por estrelas em um navegador encaminhado.
- abriu e mesclou seu primeiro pull request.

Na próxima etapa, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
