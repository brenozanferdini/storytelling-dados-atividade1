## Qual história meu dashboard conta?

Quase empate nas prefeituras: em 2024, 44% dos prefeitos ficaram e 56% chegaram. O equilíbrio muda por região (Sul renova mais, Norte mantém mais) e pela força nas urnas: com 70% a 99% dos votos, 3 em cada 4 vencedores eram reeleitos. O que decide o desempate?

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
3. **O tamanho do município muda o equilíbrio?** Não explica. A taxa fica entre 39% e 47% em todas as faixas de eleitorado, perto da média de 44,5%, sem tendência clara (a faixa de 100 mil+ tem só 152 municípios). A seção funciona como ponte: "se não é o porte, o que separa quem fica de quem sai?". À parte, nos 51 municípios com segundo turno, a taxa é de 25%.
4. **Quem permanece vence de que forma?** Com folga. Com votação abaixo de 50%, cerca de 20% dos vencedores eram reeleitos. Entre 70% e 99% dos votos, são 74% a 76%. A votação mediana dos reeleitos é 65%, contra 54% dos novos.
5. **De onde vêm os que chegam?** A maioria vem de fora da política declarada: só 8% dos 3.084 novos prefeitos declararam trajetória política, sendo 172 vereadores ou deputados (5,6%) e 73 ex-prefeitos voltando ao cargo (2,4%), uma continuidade que aparece como renovação. Os grupos mais comuns são empresário ou comerciante (23,5%), agropecuária (12,8%) e servidor público (11,7%). Os 25% com ocupações dispersas ("Outras") ficam fora do ranking. A ocupação é autodeclarada e a base não tem histórico de cargos, então a trajetória política real pode ser maior.

**Resolução:** continuidade e renovação quase empatam. O desempate varia por território e acompanha a força nas urnas, uma associação e não uma causa. Três hipóteses que a base não testa ficam para o debate: limite de mandatos, renovação de rosto ou de grupo (232 vices reeleitos e 73 ex-prefeitos que voltam) e avaliação da gestão. O painel termina devolvendo a pergunta ao município de cada aluno.

## Decisões de design

- **Seguir a skill `dashboard-narrativo-setor-publico` (`skill.md`).**
- **`<h1>` curto e com número exato:** "Quase empate: 44 prefeitos ficaram e 56 chegaram, a cada 100 cidades". A região e a votação ficam no subtítulo como gancho.
- **Títulos que dão a resposta.** Cada `<h2>` é a conclusão da pergunta ("Quem fica, fica com folga…"), e a pergunta aparece pequena acima dele.
- **Ordem da narrativa:** o quanto mudou (contexto) → onde (o que a turma não conhece) → porte e votação (tensão: o que explica) → quem chega → hipóteses para debate (resolução).
- **Gráficos escolhidos pela intenção:**
  - P1: barra 100% com 3 partes, porque é parte de um todo e por isso não é pizza.
  - P2: barras horizontais ordenadas por UF, com linha de referência na média do Brasil.
  - P3: colunas por faixa de eleitorado, **todas em cinza**, com linha tracejada na média do Brasil. Num resultado nulo, nenhum destaque de cor para não exagerar diferenças pequenas.
  - P4: colunas por faixa de votação, com destaque nas faixas de 70% a 99% (a chapa única, 68%, fica em coluna à parte).
  - P5: barras horizontais por grupo de ocupação, sem a categoria residual "Outras" (informada no texto), com as duas formas de trajetória política em azul.
- **Taxas, não absolutos.** Regiões e faixas têm tamanhos muito diferentes; o n aparece no rótulo do eixo quando importa.
- **Cor com intenção e segura para daltonismo.** Tudo é cinza, com um único destaque azul Okabe-Ito (#0072B2) por gráfico, e as caixas de reflexão têm borda laranja (#E69F00). Toda informação de cor tem redundância em rótulo direto ou texto. Não há cores de partido.
- **Seletor "Encontre seu estado".** É a resposta ao briefing (a turma conhece só a própria realidade): todos os gráficos se recalculam para a UF escolhida, e o estado aparece em azul no ranking.
- **Caixas "Para a turma"** em cada seção, com uma pergunta de reflexão, porque o objetivo é provocar debate numa aula e não só informar.
- **O que ficou de fora:**
  - Mapa coroplético, que exigiria uma geometria externa e esconderia os estados pequenos.
  - Análise por partido, que é o tema 3 de outro colega e diluiria a história.
  - Gênero e idade, cujas diferenças entre reeleitos e novos são pequenas (45% × 44% de reeleição entre mulheres e homens) e não mudam a mensagem.
- **Dados embutidos já agregados.** São 2.953 combinações de UF × região × reeleição × faixa de votação × porte × grupo de ocupação, com contagem n, geradas por `ferramentas/preparar_continuidade.py`. O HTML não lê o CSV.

## Decisões da revisão crítica

Depois da 1ª versão, fiz uma revisão estruturada com o Claude, uma decisão por vez (detalhes em `prompts.md`, prompts 5 a 11):

| # | Ponto revisado | Decisão | Motivo |
|---|---|---|---|
| 1 | Estratégia de prazo | Abrir o PR cedo e melhorar com pushes | Vale o último commit; testar a verificação automática cedo |
| 2 | Título principal | "Quase empate: 44 ficaram e 56 chegaram, a cada 100 cidades" | Uma ideia, número exato, legível no projetor; "metade" seria impreciso |
| 3 | Seção do porte (resultado nulo) | Tudo cinza, linha da média e frase-ponte para a P4 | O destaque em 39% exagerava uma diferença pequena |
| 4 | Seção da ocupação | Separar ex-prefeitos que voltam e tirar "Outras" do ranking | Ex-prefeito que volta é continuidade disfarçada; "Outras" é ruído |
| 5 | Diário de prompts | Fases e prompts literais, com reflexão | Registro fiel; não inventar prompts |
| 6 | Card da galeria | Texto com até 280 caracteres e o achado mais forte | A galeria corta em 280 caracteres |
| 7 | Conclusão | "Acompanha a força nas urnas" (associação), ex-prefeitos na hipótese 2 e fechamento para o município | A frase antiga afirmava causa; o briefing pede reflexão |
| 8 | Frase-chave da P4 | "70% a 99% dos votos: 3 em cada 4" (75,0%) | Com a chapa única incluída, seriam 73,4%; o recorte exato evita arredondar o número-vitrine |
| 9 | Skill | Exemplo do meu tema removido e nova seção "Rigor e honestidade" com as regras aprendidas na revisão | O README exige skill genérica; as regras novas são as que de fato mudaram o painel |
| 10 | Celular | Abaixo de 600px, cada gráfico mantém 560px de largura e rola de lado dentro da caixa, com a dica "↔ arraste" | A 400px os rótulos caíam para cerca de 6px; o público principal vê projetado, então preferi uma solução de baixo risco a redesenhar a biblioteca |

## Instruções para o Claude

- Gerar um único arquivo `dashboard.html`, autocontido, com os dados agregados embutidos. Usar SVG e JavaScript puros, sem CDN nem `fetch()`.
- Seguir a skill descrita em `skill.md`.
- Calcular cada resposta em pandas **antes** de desenhar e conferir pelo menos 2 números do HTML contra o pandas.
- Nunca afirmar algo que a base não sustenta: nada de "taxa de sucesso de quem tentou se reeleger" nem de "renovação por limite de mandato". O que for hipótese deve aparecer como pergunta para a turma.
- Português do Brasil, com vírgula decimal e ponto de milhar.
