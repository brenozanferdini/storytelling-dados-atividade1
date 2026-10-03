# Diário de prompts

**Link compartilhado da conversa (opcional):** sessão no Claude Code (terminal), sem link público.

Antes da aula, preparei um kit no Claude Code: um `CLAUDE.md` modelo, uma `SKILL.md` de dashboard narrativo, um template HTML com uma mini-biblioteca de gráficos em SVG puro e scripts de perfil de dados. O kit foi ensaiado com um dataset da Aula 03. No dia, os prompts foram estes, em ordem.

---

## Prompt 1

```
vamos fazer uma atividade de storytelling de dados, mas eu ainda vou te mandar o enunciado
```

**O que funcionou / o que mudei:** o Claude recuperou da memória do projeto que era o Datathon de hoje e que já havia um kit pronto. Ele pediu a base e o formato de entrega. Isso poupou o tempo de reexplicar a disciplina.

---

## Prompt 2

```
pergunta norteadora: o que as eleições de 2024 revelam sobre continuidade e renovação no comando
de prefeituras? briefing: Uma escola de governo vai abrir um curso para liderança municipais e pediu
um dashboard para a aula inaugural, sobre o contexto político em que essas lideranças vão atuar. A turma
reúne gestores, servidores e assessores de diferentes regiões, que conhecem bem a própria realidade e
pouco a dos outros municípios. Sua história deve levar essa turma a refletir sobre o equilíbrio entre
quem permanece e quem chega ao poder municipal, e sobre o que pode estar por trás desse equilíbrio.
esse é o enunciado
```

**O que funcionou / o que mudei:** sem ver os dados, o Claude propôs quebrar a pergunta em 5 subperguntas (quanto, onde, quem tentou, porte, fatores). A ideia de "taxa de sucesso de quem tentou a reeleição" caiu depois, porque a base só tem eleitos. Aprendizado: a estrutura da história deve ser validada contra o dicionário antes de virar gráfico.

---

## Prompt 3

```
procure o arquivo no idp
```

**O que funcionou / o que mudei:** o Claude delegou a busca a um subagente de navegação, que abriu a tarefa no Canvas pelo meu Chrome. A base não estava no Canvas, e sim no GitHub do professor. O subagente baixou `eleitos.csv` e `dicionario.md` e copiou o enunciado completo. Isso mudou a entrega: virou Pull Request com 4 arquivos (incluindo este `prompts.md`), prazo às 11h30, e não um repositório próprio. O prompt que o Claude passou ao subagente foi:

```
Tarefa: localizar e baixar no Canvas do IDP o dataset do Datathon de hoje e salvá-lo em
datathon\dados\. Curso 9582, tarefa 44766. Tema: eleições municipais 2024 / prefeituras.
Pode estar na tarefa, num anúncio, num módulo ou ser link externo. Não envie nem altere nada
no Canvas. Retorne: onde estava, caminho dos arquivos, texto integral do enunciado e as
primeiras linhas do CSV.
```

---

## Passos que o Claude executou a partir daí (sem novo prompt meu)

Registro para mostrar como o contexto (`claude.md`) e a skill (`skill.md`) guiaram o resultado.

1. **Leitura do dicionário e cálculos em pandas.** A taxa de reeleição saiu por região, UF, porte, faixa de votação, turno e gênero, e a ocupação foi cruzada para os novos prefeitos. Descoberta-chave: a base só tem vencedores, então "reeleito" significa "prefeito que ficou", não "candidato bem-sucedido". **Mudança:** a pergunta "quem tentou ficar conseguiu?" foi trocada por "quem permanece vence de que forma?", que a base responde com a faixa de votação.
2. **Vice como sinal de continuidade.** Ao cruzar a declaração de reeleição do vice, apareceram 232 municípios com prefeito novo e vice reeleito. Eles viraram uma terceira fatia na P1 ("renovação com continuidade") e um argumento da conclusão.
3. **Primeira versão do HTML** a partir do template, com os dados embutidos linha a linha. Testada no navegador (sem erros, sem rolagem horizontal a 400px). **Correções de texto:** "a maioria se apresenta como empresário…" era falso (os grupos somam 49%) e foi reescrito. "SC e RS lideram a troca" ficou "SC tem a menor continuidade do país".
4. **Leitura do README do repositório.** Ele exige dados **já agregados**. **Mudança:** troquei os microdados (5.553 linhas) por 2.953 combinações com contagem `n`, e o JavaScript passou a calcular taxas ponderadas. O arquivo caiu de 750 KB para 370 KB. Os números foram reconferidos contra o pandas: 44,5% no Brasil, 31,2% em SC (92 de 295) e 37,4% no RS.
5. **Seletor "Encontre seu estado"** e caixas "Para a turma", que respondem diretamente ao briefing (público que conhece só a própria realidade, aula que precisa de debate).
