---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Quase todas as organizações enfrentam dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request de rascunho para uma pessoa revisar. Assim, você pode delegar tarefas repetitivas e se concentrar em objetivos mais amplos.

Neste exercício, você explorará:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de delimitar as issues com clareza.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica para resolver. As pessoas contratadas inicialmente para criar o site não documentaram o código. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar outros jogos da mesma categoria para que as pessoas possam continuar navegando. A equipe ainda não precisa de uma implementação refinada; quer algo para testar a experiência de usuário. Esse trabalho é um impedimento, mas há issues de prioridade maior no momento.

Essas tarefas podem ser adiadas facilmente e são boas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar de forma assíncrona enquanto você cuida de outras tarefas e, depois, você revisa o resultado.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não é mágico: ele não elimina a necessidade de pensar. Até tarefas aparentemente simples podem ter complexidades escondidas, então [defina com cuidado o escopo do trabalho para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examinar os arquivos de instruções do repositório, sem alterá-los, e observar seus efeitos mais tarde.

Abra os arquivos a seguir na interface web do GitHub ou no Codespace:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário no pull request. Essas orientações vêm dos arquivos de instruções e serão retomadas durante a revisão dos PRs.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes de o agente começar. O workflow deste laboratório já foi configurado para instalar Node.js, dependências do projeto, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Conceda somente as permissões mínimas necessárias às etapas. O Copilot recebe um token próprio para suas operações.
    permissions:
      # Se as etapas precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não houver clone, o Copilot fará isso automaticamente depois da conclusão destas etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora usa Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar o workflow sem esperar o Copilot iniciar o trabalho.

## Adicionar documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção da base de código e a integração de novas pessoas à equipe. O Copilot pode criar documentação, e esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize o navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente, executando o workflow descrito acima, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do PR para baixo e localize a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em andamento e como o Copilot resolve o problema: explora o código, entende o estado do projeto, planeja a abordagem e cria código.

Isso pode levar alguns minutos. Uma das principais vantagens do agente de nuvem é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltar a desenvolver o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: você pode cuidar de uma tarefa enquanto o agente cuida de outra. Mesmo que a seção de jogos relacionados não leve muito tempo, você poderia usar esse tempo em outras atividades. Vamos atribuir essa tarefa ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem atua de forma semelhante a uma pessoa engenheira de software, você não precisa acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para que ele as execute de forma assíncrona. Ele pode resolver dívida técnica, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- delimitar issues com clareza.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e recebe instruções personalizadas, que foram abordadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
