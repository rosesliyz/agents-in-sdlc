---
title: "Lição 2 - Adicione avaliações com estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, confira o resultado em um navegador encaminhado e faça o merge como sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação com estrelas, mas os cards da página inicial ainda não a exibem. Você vai pedir ao agente que mostre essa avaliação, conferir a alteração e fazer o merge como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- conferir a alteração com `/diff`.
- executar o app para confirmar a alteração em um navegador encaminhado.
- abrir e fazer o merge da sua primeira pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação com estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você vai pedir ao agente que exiba a avaliação existente em cada card: uma alteração pequena e independente, ideal para sua primeira sessão.

## Como uma conversa funciona

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do app Copilot, uma conversa normal do CLI usa o repositório e a branch do Git que estão em uso no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar o mesmo diálogo mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente; a atividade das ferramentas do agente enquanto ele explora e edita arquivos; e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e peça a alteração

Inicie uma nova conversa para começar a implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver aberto, inicie-o com este comando:

   ```bash
   copilot --yolo
   ```

4. Use o comando `/new` e pressione <kbd>Enter</kbd> para iniciar uma nova sessão.
5. Use este prompt para pedir a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você adicionou um recurso com o Copilot CLI!

## Confira as alterações

Todas as alterações geradas por inteligência artificial (IA) devem ser revisadas antes do merge, mesmo as pequenas. Vamos conferir as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o card exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Confira os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como o Copilot, assim como outras ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. Confira o comportamento em vez de esperar uma implementação específica.

## Confira as alterações em execução

Não basta ler o código e presumir que ele funciona. Peça ao Copilot que inicie o site para você examinar a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o app:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards de jogos exibem as avaliações em uma escala de cinco.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e faça o merge da sua primeira pull request

Você criou o recurso. Agora, crie uma pull request (PR) para fazer o merge do novo código no projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar a PR, o Copilot CLI envia o commit existente e mostra o localizador uniforme de recursos (URL) da PR.
3. Abra a PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Confira os arquivos alterados e as verificações.
5. Quando tudo estiver pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você lançou sua primeira alteração usando o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- pediu ao agente que fizesse uma pequena alteração nos cards de jogos.
- conferiu a alteração com `/diff`.
- executou o app para confirmar a avaliação com estrelas em um navegador encaminhado.
- abriu e fez o merge da sua primeira pull request.

Em seguida, você vai [partir da issue de filtragem e usar os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
