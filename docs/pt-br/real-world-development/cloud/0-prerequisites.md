---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do agente GitHub Copilot na nuvem, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [codespace][codespaces] para editar arquivos de instruções e revisar o trabalho produzido pelo agente na nuvem.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório onde você desenvolverá o código, crie uma instância a partir do [template][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório e será usada ao longo dos exercícios.

1. Em uma nova janela do navegador, acesse o [template Tailspin Toys][tailspin-template].
2. Crie sua cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando do workshop em um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do repositório do template](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**nome-da-organização-ou-usuário/nome-do-repositório**), pois você o usará mais adiante no laboratório.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios do laboratório.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento baseado na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, o botão **+** para criar um novo Codespace.

    ![Crie um novo codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace levará alguns minutos, embora ainda seja bem mais rápida do que instalar todos os serviços manualmente. Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que veremos a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [dev container][dev-containers] local. Ambos garantem que o ambiente tenha todos os pré-requisitos necessários para uma experiência tranquila. Se preferir trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado — o VS Code criará o mesmo dev container usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório! Você também iniciou a criação do codespace, que usará enquanto trabalha com o agente Copilot na nuvem.

## Próxima etapa

Vamos adicionar instruções personalizadas que o agente na nuvem seguirá. Continue para o [Exercício 1: Instruções personalizadas][next-lesson].

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Primeiros passos com o Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
