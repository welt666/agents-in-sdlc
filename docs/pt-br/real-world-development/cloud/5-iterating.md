---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência das pessoas usuárias e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse o recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, pedir que o Copilot aprimore o trabalho.

### Cenário

Como já destacamos, os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e a acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot trabalha de forma assíncrona e sem supervisão contínua, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem executa o trabalho no GitHub Actions, em um ambiente separado e temporário.
- Uma pessoa precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a documentação do código. Você fará essa revisão na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez apareça um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**: o acesso do Copilot a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a marca **[WIP]**. Aguarde até que ele termine. Isso pode levar alguns minutos.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui novos comentários TSDoc e outra documentação. As alterações exatas variam.

   Ao revisar, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as alterações, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot nos comentários do PR — como faria com outra pessoa da equipe — ou em comentários inline no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um prompt único obrigatório. Você pode pedir, por exemplo, que o Copilot:

- adicione cabeçalhos de comentário no início de cada arquivo de código para explicar sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das sugestões acima ou peça outra melhoria de documentação.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, vamos voltar ao PR que o Copilot criou para mostrar jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode trocar para essa branch no Codespace para testar manualmente os jogos relacionados. Abra o terminal do Codespace e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar com o agente de nuvem e pedir atualizações, por exemplo, para alterar quantos jogos relacionados são exibidos ou adicionar cabeçalhos de comentário e comentários TSDoc. Lembre-se: essa issue foi atribuída **antes** de você atualizar as instruções personalizadas. Para pedir alterações, adicione um comentário na guia **Conversation**; o Copilot iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a marca **[WIP]**. Aguarde até que ele termine. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de CSS ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a troca de modos.

6. Depois de revisar as alterações, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode trocar para essa branch no Codespace e testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os botões de alternância dos modos de alto contraste e claro no navegador. Confirme que funcionam e continuam selecionados depois de recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizados do repositório** para observar seus efeitos no trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e refinou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente de nuvem.

No seu próprio repositório, experimente:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
