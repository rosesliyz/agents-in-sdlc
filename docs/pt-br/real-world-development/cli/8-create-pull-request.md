---
title: "Lição 8 - Crie e faça o merge da PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir uma pull request. Você fez o merge da pull request (PR) de avaliação com estrelas manualmente; desta vez, deixará o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em algumas PRs. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluirá todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar todas as alterações do recurso e as evidências de verificação.
- criar a PR da filtragem.
- ativar o Agent Merge somente depois da revisão e confirmar que a PR foi mesclada.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR sem tirar do desenvolvedor o controle da autorização do merge.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para integrar uma pull request. Quando você o ativa, o Copilot resolve os bloqueios da PR — corrige verificações de integração contínua (CI) que falharam, responde a comentários da revisão e faz o rebase quando necessário — e depois habilita o merge automático do GitHub se o repositório permitir.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de ativá-lo.

## Use o Agent Merge para gerenciar a PR

Com o código criado, vamos revisá-lo, criar a PR e deixar o Agent Merge cuidar do restante do processo.

1. Volte ao Codespace.
2. Digite `/agent` para abrir a caixa de diálogo do agente.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo der certo, ele fará o merge.
7. Confirme que o Copilot exibiu uma mensagem semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração de código, os testes e a validação, e agora o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou todas as alterações do recurso e as evidências de verificação.
- criou a PR da filtragem.
- ativou o Agent Merge somente depois da revisão e confirmou que a PR foi mesclada.

Em seguida, você vai [explorar outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
