## Qual história meu dashboard conta?

Em 2024, 44 em cada 100 prefeitos brasileiros foram reeleitos e 56 chegaram ao cargo pela primeira vez naquele município. O equilíbrio é quase um empate, mas não é uniforme: o Sul renova mais que o Norte, e quem permanece costuma vencer com votações acima de 70%. O dashboard convida a turma a perguntar o que está por trás desse desempate: força do grupo no poder, limite de mandatos ou avaliação da gestão.

## Contexto do projeto

- **Tema designado:** 4. Continuidade e renovação.
- **Pergunta norteadora:** *O que as eleições de 2024 revelam sobre continuidade e renovação no comando das prefeituras?*
- **Briefing:** uma escola de governo vai abrir um curso para lideranças municipais e pediu um dashboard para a aula inaugural sobre o contexto político em que essas lideranças vão atuar. A história deve levar a turma a refletir sobre o equilíbrio entre quem permanece e quem chega ao poder municipal, e sobre o que pode estar por trás desse equilíbrio.
- **Base:** `dados/eleitos.csv`, com 11.106 linhas (prefeito e vice de 5.553 municípios) e 72 colunas. Usa `;` como separador e vírgula decimal, em UTF-8 com BOM.
- **Cuidados do dicionário que foram aplicados:**
  - Filtro `cargo = Prefeito` para contar prefeitos e municípios. O vice só entra para saber se também concorria à reeleição.
  - "Reeleito" = `candidato_a_reeleicao_tse = Sim` (declaração ST_REELEICAO), sem auditoria do mandato anterior.
  - Votos e percentuais se repetem na chapa, por isso foram lidos só na linha do prefeito.
  - Célula vazia não é zero: 9 chapas sem percentual de votos ficaram fora apenas do gráfico de votação.
  - As 23 chapas com classificação `CHAPA_CASSADA_TSE` ou `ELEICAO_HISTORICA_DOCUMENTADA` foram mantidas. A base retrata a eleição, não quem governa hoje, e a taxa com ou sem elas é a mesma: 44,5%.
  - **Limite central:** a base só tem quem venceu. Não dá para calcular a taxa de sucesso de quem tentou a reeleição nem saber quem estava impedido de concorrer por já cumprir o segundo mandato. O painel diz isso explicitamente.

## Público-alvo

- **Quem:** a turma da aula inaugural, com gestores, servidores e assessores municipais de várias regiões.
- **O que já sabe:** conhece bem a realidade política do próprio município e pouco a dos outros. Não é especialista em dados eleitorais.
- **Como vai ler:** o painel é projetado em sala e depois explorado por cada aluno, com cerca de 5 minutos de leitura guiada.
- **O que deve fazer depois:** situar o próprio município e estado no quadro nacional e debater hipóteses sobre por que uns permanecem e outros chegam.

## Perguntas que os dados respondem

Na ordem da história (contexto → tensão → resolução):

1. **Quanto o comando das prefeituras mudou em 2024?** 44,5% dos prefeitos foram reeleitos (2.469 de 5.553). 51,4% das prefeituras trocaram prefeito e vice, e 4,2% trocaram o prefeito, mas mantiveram o vice.
2. **O equilíbrio é o mesmo em todo o país?** Não. Os reeleitos são 37% no Sul, 43% no Sudeste, 48% no Nordeste, 50% no Centro-Oeste e 51% no Norte. Por estado, a taxa vai de 31% em SC a 67% em RR.
3. **O tamanho do município muda o equilíbrio?** Pouco. A taxa fica entre 42% e 47% até 100 mil votos válidos e cai para 39% acima disso. Nos 51 municípios com segundo turno, ela é de 25%.
4. **Quem permanece vence de que forma?** Com folga. Com votação abaixo de 50%, cerca de 20% dos vencedores eram reeleitos. Entre 70% e 99% dos votos, são 74% a 76%. A votação mediana dos reeleitos é 65%, contra 54% dos novos.
5. **De onde vêm os que chegam?** Só 8% dos 3.084 novos prefeitos declararam ocupação política (ex-prefeito, vereador, deputado). Os grupos mais comuns são empresário ou comerciante (24%), agropecuária (13%) e servidor público (12%). A ocupação é autodeclarada e subestima a trajetória política.

**Resolução:** continuidade e renovação estão quase empatadas, e o desempate depende da força do grupo no poder, que varia por território. Três hipóteses que a base não testa ficam para o debate: limite de mandatos, renovação de rosto e não de grupo, e avaliação da gestão.

## Decisões de design

- **Seguir a skill `dashboard-narrativo-setor-publico` (`skill.md`).**
- **Títulos que dão a resposta.** Cada `<h2>` é a conclusão da pergunta ("Quem fica, fica com folga…"), e a pergunta aparece pequena acima dele.
- **Ordem da narrativa:** o quanto mudou (contexto) → onde (o que a turma não conhece) → porte e votação (tensão: o que explica) → quem chega → hipóteses para debate (resolução).
- **Gráficos escolhidos pela intenção:**
  - P1: barra 100% com 3 partes, porque é parte de um todo e por isso não é pizza.
  - P2: barras horizontais ordenadas por UF, com linha de referência na média do Brasil.
  - P3 e P4: colunas por faixa ordenada.
  - P5: barras horizontais de grupos de ocupação.
- **Taxas, não absolutos.** Regiões e faixas têm tamanhos muito diferentes; o n aparece no rótulo do eixo quando importa.
- **Cor com intenção e segura para daltonismo.** Tudo é cinza, com um único destaque azul Okabe-Ito (#0072B2) por gráfico, e as caixas de reflexão têm borda laranja (#E69F00). Toda informação de cor tem redundância em rótulo direto ou texto. Não há cores de partido.
- **Seletor "Encontre seu estado".** É a resposta ao briefing (a turma conhece só a própria realidade): todos os gráficos se recalculam para a UF escolhida, e o estado aparece em azul no ranking.
- **Caixas "Para a turma"** em cada seção, com uma pergunta de reflexão, porque o objetivo é provocar debate numa aula e não só informar.
- **O que ficou de fora:**
  - Mapa coroplético, que exigiria uma geometria externa e esconderia os estados pequenos.
  - Análise por partido, que é o tema 3 de outro colega e diluiria a história.
  - Gênero e idade, cujas diferenças entre reeleitos e novos são pequenas (45% × 44% de reeleição entre mulheres e homens) e não mudam a mensagem.
- **Dados embutidos já agregados.** São 2.953 combinações de UF × região × reeleição × faixa de votação × porte × grupo de ocupação, com contagem n, geradas por `ferramentas/preparar_continuidade.py`. O HTML não lê o CSV.

## Instruções para o Claude

- Gerar um único arquivo `dashboard.html`, autocontido, com os dados agregados embutidos. Usar SVG e JavaScript puros, sem CDN nem `fetch()`.
- Seguir a skill descrita em `skill.md`.
- Calcular cada resposta em pandas **antes** de desenhar e conferir pelo menos 2 números do HTML contra o pandas.
- Nunca afirmar algo que a base não sustenta: nada de "taxa de sucesso de quem tentou se reeleger" nem de "renovação por limite de mandato". O que for hipótese deve aparecer como pergunta para a turma.
- Português do Brasil, com vírgula decimal e ponto de milhar.
