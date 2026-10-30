---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorrem. Você pode revisar cada diff no chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as alterações propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um botão de alternância do modo claro além do alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design fez outro pedido: também quer um modo claro para quem considera o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as alterações propostas. Cada alteração aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role pelo chat para acompanhar cada chamada de ferramenta, leitura, edição e comando de terminal solicitado.
3. Quando o agente propuser uma alteração, o diff aparecerá no chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir **Files changed** na parte superior do chat para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente fará uma pausa e exibirá **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Uma mensagem final resumirá as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um botão de alternância do modo claro. Como continuará na mesma sessão, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilos e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo atualizações para o modo claro.
3. Revise cada nova alteração à medida que aparecer, usando **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos botões de alternância —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes (“simplifique o CSS — use variáveis CSS em vez de duplicar seletores”), corrigir o rumo (“não adicione um terceiro botão; integre o modo claro ao painel de configurações existente”) ou ampliar o escopo (“adicione também atalhos de teclado para os dois modos”).

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em eventuais testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente estão presentes: atributos ARIA, navegação pelo teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada alteração, aceitou ou rejeitou edições e enviou prompts de acompanhamento para ampliar o trabalho sem perder contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições de arquivos individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

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
