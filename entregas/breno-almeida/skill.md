---
name: dashboard-narrativo-setor-publico
description: Regras visuais e narrativas para construir dashboards em HTML único e autocontido que contam uma história com dados públicos para um público definido. Use sempre que for criar um painel, gráfico ou relatório visual a partir de uma base de dados, ou revisar um que já exista.
---

# Dashboard narrativo para o setor público

## Quando usar

- Pedidos de "dashboard", "painel", "gráfico", "visualização" ou "relatório visual" a partir de uma base tabular.
- Revisão de um dashboard existente para clareza, honestidade e acessibilidade.
- Sempre que houver uma **pergunta norteadora** e um **público** definidos. Se faltar um dos dois, pergunte antes de desenhar.

## Procedimento (nesta ordem)

1. **Ler o dicionário de dados antes do CSV.** Anote a unidade de cada linha, colunas que se repetem entre linhas, o significado de célula vazia, o separador decimal e os critérios de inclusão. Escreva os cuidados no `claude.md`.
2. **Responder antes de desenhar.** Quebre a pergunta norteadora em 4 a 6 subperguntas. Calcule cada resposta em pandas e escreva-a como **uma frase com número** ("O Sul renova mais: 37% de reeleitos contra 51% no Norte").
3. **Descobrir o que a base NÃO responde** e decidir como dizer isso no painel: nota de método, aviso junto ao gráfico ou pergunta aberta ao público. Nunca preencha lacuna com suposição.
4. **Escolher um gráfico por subpergunta** (tabela abaixo) e definir **o único elemento em destaque**, que é o que prova a frase.
5. **Agregar em pandas e embutir só a tabela agregada** no HTML, com contagens (`n`). Assim o navegador recalcula taxas por filtro sem carregar microdados.
6. **Construir** o HTML (estrutura abaixo), abrir no navegador e corrigir até não haver erro no console.
7. **Verificar** com o checklist final e registrar as decisões no `claude.md`.

## Estrutura narrativa

- **Cabeçalho (contexto):** um rótulo pequeno com tema e período; um `<h1>` que é a **mensagem principal em uma frase**, nunca o nome do tema; e um subtítulo que fala com o público ("Vocês vão…").
- **Até 4 KPIs**, com um só em destaque, que deve ser a mensagem principal.
- **Uma seção por subpergunta (desenvolvimento e tensão).** A pergunta vai em letra pequena acima, o `<h2>` traz a **resposta** e a linha seguinte informa unidade, recorte e n. Depois vêm o gráfico e um bloco "como ler" com 2 a 3 frases que dizem o que o gráfico prova.
- **Ordene as seções como uma história:** quanto (tamanho do fenômeno) → onde (distribuição) → por quê (fatores) → quem (perfil) → e agora (resolução).
- **Seção final (resolução):** a conclusão como título, mais recomendações, alertas ou perguntas para o público agir ou debater.
- **Rodapé:** fonte, data da extração, autor e **notas de método** (filtros, exclusões, limites da base).
- **Adapte ao público.** Para formação ou aula, inclua uma pergunta de reflexão por seção. Para gestores com pouco tempo, deixe a conclusão no topo e corte texto. Para o cidadão, use linguagem simples e um exemplo concreto. Se o público conhece só o próprio território, ofereça um filtro "encontre o seu" que destaque o item escolhido em todos os gráficos.

## Escolha de gráficos

| Intenção | Use | Evite |
|---|---|---|
| Ranking ou comparação entre muitas categorias | barras horizontais ordenadas, com rótulo direto e linha de referência (média, meta) | barras em ordem alfabética, legenda separada |
| Poucas faixas ordenadas (porte, idade, renda) | colunas na ordem natural das faixas | reordenar faixas pelo valor |
| Evolução no tempo | linhas (ou colunas se houver até 6 períodos) | ligar com linha categorias que não são tempo |
| Parte de um todo | barra 100% com até 4 partes | pizza ou rosca com mais de 3 fatias, 3D |
| Relação entre duas variáveis | dispersão com rótulo só nos pontos de interesse | eixo duplo |
| Valores exatos | tabela curta | gráfico com 30 rótulos sobrepostos |

- **Taxas e percentuais, não absolutos**, quando os grupos têm tamanhos diferentes. Mostre o `n` quando ele for pequeno.
- **Barras e colunas começam em zero.**
- Não emende fontes ou metodologias diferentes numa mesma série.

## Paleta de cores

Paleta Okabe-Ito, segura para daltonismo:

- `#b8c0c8`: **cinza de contexto**, para tudo que não é a mensagem.
- `#0072B2`: **azul de destaque**, só para o elemento que prova o título (um por gráfico).
- `#D55E00`: **vermelhão**, só para alerta que exige ação.
- `#E69F00`: **laranja de apoio**, para uma segunda série inevitável ou caixas de reflexão.
- Texto em `#1f2933`, texto secundário em `#5f6b76`, grades em `#d9dee3`, painéis em `#f5f7f9`.
- **Nunca** use vermelho × verde, arco-íris, cores de partidos ou instituições, nem gradientes.
- **Redundância obrigatória:** toda informação codificada em cor também aparece em rótulo, posição ou texto. O painel precisa ser legível em escala de cinza.

## Tipografia e layout

- Fonte do sistema (`"Segoe UI", Calibri, Arial, sans-serif`), sem fontes web. `<h1>` com cerca de 28px, `<h2>` com cerca de 21px, texto de 14 a 15px e notas de 12 a 13px.
- Título em frase completa, sem ponto de exclamação nem caixa alta.
- Grade de 2 colunas (gráfico 2/3 e leitura 1/3) que vira 1 coluna abaixo de 820px. Nenhuma rolagem horizontal a 400px de largura.
- Gráficos em SVG com `viewBox` (escalam sozinhos), **rótulos diretos** no lugar de legenda, grade discreta, sem moldura, sombra ou ícone decorativo.
- Números em pt-BR (vírgula decimal, ponto de milhar), com `toLocaleString("pt-BR")`. Arredonde sem mudar a conclusão.
- Arquivo **único**: CSS e JS embutidos, sem `fetch()` e sem ler arquivos locais.

## Checklist final

- [ ] O `<h1>` e cada `<h2>` dizem uma **conclusão**, não um tema.
- [ ] Cada subpergunta tem 1 gráfico principal, 1 destaque de cor e um bloco "como ler".
- [ ] 2 ou mais números do HTML conferidos contra o cálculo em pandas.
- [ ] Os limites da base estão declarados no painel (notas de método ou aviso junto ao gráfico).
- [ ] Nenhum `{{`, `TODO` ou texto de exemplo esquecido.
- [ ] Abre com duplo clique, sem erros no console; filtros e interações funcionam.
- [ ] A 400px de largura não há rolagem horizontal e os rótulos não se sobrepõem.
- [ ] A mensagem continua legível em escala de cinza.
- [ ] As decisões de design e o que ficou de fora estão registrados no `claude.md`.
