# Diário de prompts

**Link compartilhado da conversa (opcional):** sessão no Claude Code (terminal), sem link público.

Os prompts estão **literais e em ordem cronológica**. Organizei em três fases: preparação, construção e revisão crítica.

---

## Fase 0: preparação (antes da aula)

Como o dataset era surpresa, preparei antes um kit no Claude Code: um `CLAUDE.md` modelo, uma skill de dashboard narrativo baseada nas Aulas 01–03, um template HTML com gráficos em SVG puro e scripts de perfil de dados. Ensaiei com uma base da Aula 03. No dia, só precisei adaptar o kit, e a skill entregue (`skill.md`) é a versão genérica dele.

---

## Fase 1: construção

### Prompt 1

```
vamos fazer uma atividade de storytelling de dados, mas eu ainda vou te mandar o enunciado
```

**O que funcionou / o que mudei:** o Claude reconheceu pela memória do projeto que era o Datathon e que o kit estava pronto. Ele pediu a base, o formato de entrega e os critérios. Abrir assim poupou tempo de contexto.

---

### Prompt 2

```
pergunta norteadora: o que as eleições de 2024 revelam sobre continuidade e renovação no comando
de prefeituras? briefing: Uma escola de governo vai abrir um curso para liderança municipais e pediu
um dashboard para a aula inaugural, sobre o contexto político em que essas lideranças vão atuar. a turma
reune gestores, servidores e assessores de diferentes regiões, que conhecem bem a propria realidade e
pouco a dos outros municipios. Sua historia deve levar essa turma a refletir sobre o equilibrio entre
quem permanece e quem chega ao poder municipal, e sobre o que pode estar por tras desse equilibrio.
esse é o enunciado
```

**O que funcionou / o que mudei:** sem ver os dados, o Claude propôs quebrar a pergunta em 5 subperguntas: quanto, onde, quem tentou ficar, porte e fatores. A ideia de mostrar a "taxa de sucesso de quem tentou a reeleição" caiu logo depois, porque a base tem **só os eleitos**. Isso me mostrou que a estrutura da história precisa ser validada contra o dicionário antes de virar gráfico.

---

### Prompt 3

```
procure o arquivo no idp
```

**O que funcionou / o que mudei:** o Claude delegou a busca a um subagente de navegação no meu Chrome. Descobri que a base não estava no Canvas, e sim no GitHub do professor, e que a entrega era um **Pull Request com 4 arquivos** (incluindo este diário), com prazo às 11h30. Isso mudou o plano de entrega que eu tinha preparado. O prompt que o Claude passou ao subagente foi:

```
Tarefa: localizar e baixar no Canvas do IDP o dataset do Datathon de hoje e salvá-lo em
datathon\dados\. Curso 9582, tarefa 44766. Tema: eleições municipais 2024 / prefeituras.
Pode estar na tarefa, num anúncio, num módulo ou ser link externo. Não envie nem altere nada
no Canvas. Retorne: onde estava, caminho dos arquivos, texto integral do enunciado e as
primeiras linhas do CSV.
```

Com a base em mãos, deixei o Claude seguir o procedimento da skill sem novo prompt: ler o dicionário, calcular as respostas em pandas, montar a 1ª versão e testar no navegador. Ao revisar o resultado, registrei o que mudou:
- **Pergunta trocada:** "quem tentou ficar conseguiu?" virou "quem permanece vence de que forma?". A base responde essa pela faixa de votação, e ela virou o achado mais forte: de 13% para 76% de reeleitos conforme a votação cresce.
- **Achado não planejado:** em 232 municípios, o prefeito é novo, mas o vice foi reeleito. Virou uma terceira fatia na P1 ("renovação com continuidade").
- **Dois textos corrigidos** na 1ª versão: "a maioria se apresenta como empresário…" era falso, porque os grupos somam 49%. E "SC e RS lideram a troca" era impreciso, porque o AM empata com o RS.
- **Ao ler o README do repositório,** vi que os dados embutidos tinham de estar **agregados**. Troquei os microdados (5.553 linhas) por combinações com contagem e reconferi os números contra o pandas: Brasil 44,5%, SC 31,2% e RS 37,4%.
- **Pensando no público,** que conhece só a própria realidade, entraram o seletor **"Encontre seu estado"** e as caixas **"Para a turma"**.

---

### Prompt 4

```
pode seguir com as 5 subperguntas
```

**O que funcionou / o que mudei:** aprovei a estrutura. O Claude explicou quais subperguntas mudaram em relação à proposta inicial (P3 trocada e P5 dividida entre a seção e a conclusão) e por quê. O dashboard estava completo e verificado, mas eu queria uma revisão crítica antes de considerá-lo final.

---

## Fase 2: revisão crítica (questionamento estruturado)

### Prompt 5

```
/grill-me para melhorar a entrega e tirar nota 10
```

**O que funcionou / o que mudei:** pedi ao Claude que me questionasse com a rubrica em mente, em vez de "melhorar" por conta própria. Ele levantou 6 pontos de uma vez: prazo, título, seção do porte, seção da ocupação, este diário e o card da galeria.

---

### Prompt 6

```
vamos ponto a ponto de forma detalhada
```

**O que funcionou / o que mudei:** 6 decisões de uma vez seria superficial. Pedi uma por vez, com prós e contras, para decidir de forma consciente.

---

### Prompt 7

```
a, pode criar o fork e abrir o PR
```

**Decisão:** entregar já e melhorar depois, em vez de entregar uma vez perto do prazo. **Por quê:** vale o horário do último commit, o PR se atualiza a cada push e eu nunca tinha testado a verificação automática. **Efeito:** o PR #4 abriu às 09h38 e recebeu "✅ Entrega verificada". Daí em diante, cada melhoria virou um commit com mensagem descritiva.

---

### Prompt 8

```
c, pode seguir
```

**Decisão:** o título principal. **Antes:** "Em 2024, 44 em cada 100 prefeitos ficaram e 56 chegaram. O equilíbrio muda conforme a região e conforme a força que o prefeito tinha nas urnas." **Depois:** "Quase empate: 44 prefeitos ficaram e 56 chegaram, a cada 100 cidades." **Por quê:** o título antigo dava três ideias de uma vez e não cabia no projetor. Rejeitei "Metade fica, metade chega" porque 44 × 56 não é metade, e rejeitei a versão em pergunta porque contradiz a regra da minha skill (título = conclusão). A palavra "empate" prepara o fio da conclusão: o que decide o desempate?

---

### Prompt 9

```
a, pode seguir
```

**Decisão:** a seção do porte, um resultado nulo (42% a 47%). **Antes:** "Quase não…", com a coluna de 100 mil+ em azul. **Depois:** "O tamanho da cidade não explica o equilíbrio…", com todas as colunas em cinza, linha tracejada na média do Brasil e uma frase final que leva à P4: "Se não é o porte, o que separa quem fica de quem sai? A resposta está nas urnas ↓". **Por quê:** o destaque azul em 39% (só 152 municípios) exagerava uma diferença pequena, justamente o tipo de ênfase enganosa da Atividade 3. O resultado nulo virou argumento: uma explicação óbvia descartada. **Ajuste no teste:** a linha cortava o rótulo "42%", e os rótulos ganharam contorno branco.

---

### Prompt 10

```
b, pode seguir
```

**Decisão:** a seção 5, sobre de onde vêm os que chegam. **Antes:** a maior barra era "Outras ocupações" (25%), uma categoria residual que roubava o primeiro olhar, e "Política" juntava tudo. **Depois:** "Outras" saiu do ranking e foi para o texto, e "Política" foi separada em vereador ou deputado (172) e **ex-prefeito voltando ao cargo (73)**. Os ex-prefeitos são uma continuidade que aparece como renovação, exatamente o tema. **Por quê:** antes de decidir, pedi para verificar se a base sustentava uma seção "renovação de rosto ou de grupo?". Não sustenta, porque não há histórico de cargos nem dados de 2020, e por isso não a criei. **Ajuste:** troquei "quem foi vice costuma declarar a profissão de origem" por "pode ter declarado", porque não dá para provar com a base.

---

### Prompt 11

```
b, pode seguir
```

**Decisão:** reorganizar este diário em fases, com todos os prompts literais, em vez de reescrever o bloco "o Claude executou sozinho" como se fossem prompts meus. **Por quê:** o enunciado pede os prompts que usei de fato. Inventar prompts trairia o registro. O que mostra a minha condução são as decisões e as reflexões.

---

### Prompt 12

```
a, pode seguir
```

**Decisão:** o texto do card da galeria. Li o script `gerar_galeria.py` e vi que o card corta em 280 caracteres. O texto antigo tinha cerca de 450 e seria cortado no meio, além de descrever o título antigo. **Depois (259 caracteres):** "Quase empate nas prefeituras: em 2024, 44% dos prefeitos ficaram e 56% chegaram. O equilíbrio muda por região (Sul renova mais, Norte mantém mais) e pela força nas urnas: acima de 70% dos votos, 3 em cada 4 vencedores eram reeleitos. O que decide o desempate?" **Por quê:** num mural com vários cards, um número concreto (3 em cada 4) chama mais atenção do que a descrição do público. Aproveitei para sincronizar todo o `claude.md` com as decisões da revisão.

---

### Prompt 13

```
a, pode seguir
```

**Decisão:** a conclusão. **Antes:** "Quem decide o desempate **é** a força do grupo no poder" e "onde o prefeito era dominante, ele ficou". **Depois:** "Continuidade e renovação quase empatam. O desempate varia por território e acompanha a força nas urnas", seguido de "a base mostra onde e como, mas não diz por quê". **Por quê:** a base mostra associação, não causa. Além disso, a votação é de 2024 e não indica dominância anterior, então a frase antiga afirmava mais do que os dados permitem. Também integrei o achado da P5 (73 ex-prefeitos) à hipótese "rosto ou grupo" e acrescentei um fechamento que devolve a pergunta ao município de cada aluno, como pede o briefing.

---

### Prompt 14

```
a, pode seguir
```

**Decisão:** a precisão do número-vitrine. Pedi para conferir em pandas a frase "acima de 70% dos votos, 3 em cada 4 foram reeleitos", que aparecia no título da P4, no card e no `claude.md`. Incluindo a chapa única (100%), o número real é **73,4%** (939 de 1.280). De 70% a 99%, é **75,0%** (744 de 992). **Depois:** "com 70% a 99% dos votos, 3 em cada 4 foram reeleitos", nos três lugares. **Por quê:** o arredondamento era defensável, mas é o número mais visível do painel, e a chapa única já tem coluna própria no gráfico (68%).

---

### Prompt 15

```
b, pode seguir
```

**Decisão:** a skill. Relendo a `skill.md`, encontrei uma violação da regra do README: o exemplo de "frase com número" citava o meu próprio tema ("O Sul renova mais: 37%…"). Troquei por um exemplo neutro. Também acrescentei uma seção **"Rigor e honestidade"**, com as regras genéricas que surgiram nesta revisão: associação não é causa, resultado nulo sem destaque e com linha de referência, nada de destacar diferença pequena com n pequeno, categoria residual fora do ranking, número-vitrine no recorte exato, "declarado não é verificado" e limites ditos no ponto da história. Cada uma ganhou um item no checklist. **Por quê:** a skill precisa refletir o que de fato orientou o resultado. Essas regras mudaram o painel, e vão servir para o trabalho final.

---

### Prompt 16

```
b, pode seguir
```

**Decisão:** a legibilidade no celular. A auditoria final (26 UFs testadas no seletor, sem erros) mostrou que, a 400px, os gráficos encolhiam e os rótulos caíam para cerca de 6px. **Depois:** em telas estreitas, cada gráfico mantém uma largura mínima legível e rola de lado dentro da própria caixa, com a dica "↔ arraste o gráfico para o lado". A página continua sem rolagem horizontal. **Por quê:** o público principal vê o painel projetado em sala. Redesenhar a biblioteca para o celular levaria uns 25 minutos, com risco de quebrar o desktop já validado. Escolhi a solução de 5 minutos e baixo risco.

---

### Prompt 17

```
a, pode seguir
```

**Decisão:** os 4 KPIs do topo. **Antes:** 44,5% reeleitos · 51,4% trocaram prefeito e vice · "65% × 54% votação mediana" · 37% × 51% Sul × Norte. **Depois:** 44,5% (Brasil) · 37% × 51% · **3 em 4** (vencedores com 70% a 99% dos votos eram reeleitos) · **277** novos prefeitos com sinal de continuidade. **Por quê:** "mediana" é jargão para o público, o 2º KPI era quase o complemento do 1º, e o topo não tinha o número mais forte nem o achado mais ligado ao tema. Ao calcular os 277, o Claude conferiu a sobreposição: 232 com vice reeleito e 73 ex-prefeitos têm 28 casos em comum, então usei o número único e não a soma ingênua (305). Os KPIs ficam fixos no Brasil, e o rótulo diz isso, porque a faixa do seletor já mostra o resumo da UF.

---

### Prompt 18

```
a, pode seguir
```

**Decisão:** o título da P1. **Antes:** "Pouco mais da metade das prefeituras trocou prefeito e vice. Em 44% **delas** o prefeito foi reeleito." Esse "delas" podia ser lido como "44% das que trocaram", o que é contraditório. **Depois:** "44% mantiveram o prefeito, 51% trocaram prefeito e vice, e 4% ficaram no meio-termo: prefeito novo com o vice de antes." **Por quê:** elimina a ambiguidade, segue a ordem das três partes da barra logo abaixo e deixa de repetir o `<h1>`, introduzindo a ideia de meio-termo que os KPIs e a conclusão retomam.

---

## O que eu faria diferente

1. **Ler o dicionário antes de propor a estrutura da história.** A "taxa de sucesso na reeleição" parecia óbvia e era impossível com uma base só de eleitos. Os limites da base definem as perguntas possíveis.
2. **Ler o README de entrega antes de construir.** A exigência de dados agregados me fez refazer a camada de dados depois da 1ª versão pronta.
3. **Fazer a revisão crítica cedo e uma decisão por vez.** As melhorias mais importantes (o porte como ponte e os ex-prefeitos como continuidade disfarçada) vieram do questionamento, não da primeira geração. Ser questionado foi mais útil do que pedir ao agente "melhore".
