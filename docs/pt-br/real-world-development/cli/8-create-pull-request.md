---
title: "Lição 8 - Criar e fazer merge do PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação dos filtros, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) de avaliação por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Em geral, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, reunindo o trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar a alteração completa do recurso e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi integrado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request. Quando você o habilita, o Copilot resolve o que estiver bloqueando o PR — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários de revisão e fazendo rebase quando necessário — e depois habilita o auto-merge do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisá-lo em conjunto, criar o PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que você recebeu do Copilot uma mensagem semelhante a “PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branches, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e faça o merge manualmente após a revisão, quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, o teste e a validação do código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou a alteração completa do recurso e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi integrado.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar contexto, modelos, compartilhamento e a delegação opcional ao agente de nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
