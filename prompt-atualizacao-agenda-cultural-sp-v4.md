# Prompt-mestre: Atualização do app Agenda Cultural SP

> Cole este prompt inteiro sempre que pedir uma atualização. Reescreve o bloco de
> dados do **`index.html`** — o arquivo standalone publicado via GitHub Pages
> a partir deste repositório (não o artifact `.jsx` de versões anteriores, que
> ficou defasado assim que passamos a publicar fora do Claude.ai). Sem quebrar
> favoritos salvos (os `id` de eventos que continuam válidos não podem mudar).
>
> Cada rodada cobre **mês corrente + prévia do mês seguinte** — leia a seção 1.1
> antes de mexer em `MONTH_LABEL`, e a 5.3 antes de escrever qualquer `period`.
>
> Ao final, o arquivo atualizado deve ser entregue pronto pra commitar e dar
> push neste repositório — o GitHub Pages publica a versão nova a partir daí.

---

## 1. Objetivo

Atualizar a agenda cultural do app **Agenda Cultural SP**, cobrindo a cidade de
São Paulo como um todo (sem recorte de bairro ou raio). Buscar informações reais
e verificadas — nunca usar conhecimento de treinamento sem confirmar, já que a
programação cultural muda mensalmente.

Cada rodada cobre **dois meses**: o **mês corrente**, com agenda completa, e o
**mês seguinte**, como prévia do que já estiver confirmado (seção 1.1).

### 1.1 Modelo de dois meses

**`MONTH_LABEL`/`YEAR_LABEL` são sempre o mês real de hoje — nunca adiantar
para o mês seguinte, nem no fim do mês.** Não é preferência estética: o app só
esconde evento passado quando o rótulo bate com o mês real (`hasEventEnded()`
compara os dois). Adiantar o rótulo faz a agenda inteira do mês corrente
reaparecer e nada mais expirar.

Onde cada coisa entra:

- **`WEEKS` — só o mês corrente.** O campo `date` (`"Sáb 29"`, `"29–30"`) é
  sempre lido como dia do mês do rótulo. Evento de outro mês em `WEEKS` gera
  data errada no indicador "Hoje" e no botão "Adicionar ao calendário".
- **`ONGOING` — itens contínuos do mês corrente + tudo que já estiver
  confirmado do mês seguinte.** Os do mês seguinte usam `period` começando com
  o nome do mês e um separador `·`, seguido de datas com mês explícito:
  `"Setembro · 18 a 26/09"`. O prefixo deixa claro no card que não é deste mês,
  e o `/MM` faz o app calcular a data certa mesmo com o rótulo em outro mês.

**Na virada do mês**, o que era prévia vira agenda: mover o item de `ONGOING`
para `WEEKS`, **mantendo exatamente o mesmo `id`** (é o que preserva os
favoritos de quem já salvou o evento na prévia), trocando `period` por `date`.
Itens que atravessam a virada e continuam contínuos ficam em `ONGOING`, só com
o `period` reescrito para o novo mês corrente.

A prévia do mês seguinte **não tem meta de cobertura**. Grandes instituições
(Theatro Municipal, Osesp, MASP, Pinacoteca, Japan House) publicam temporada com
antecedência e costumam render alguns itens; casas de show com agenda contínua
(JazzB, Blue Note, Casa de Francisca, Cine Joia) raramente publicam o mês
seguinte a tempo — e nesse caso simplesmente não entram, sem inventar nada. A
regra 4.4 vale igual para os dois meses.

## 2. Locais de curadoria fixa (buscar cada um individualmente)

Buscas genéricas ("agenda cultural SP agosto") favorecem sempre os mesmos 2-3
resultados mais indexados. Cada local abaixo precisa de sua própria busca
específica (ex: `"Sesc Pompeia" programação agosto 2026`).

**Sesc — cada unidade separadamente:**
Sesc Pompeia · Sesc 14 Bis · Sesc Belenzinho · Sesc Avenida Paulista ·
Sesc 24 de Maio · guia "Em Cartaz" mensal (sescsp.org.br/editorial/emcartaz)
como checagem cruzada.

**Música ao vivo:**

· JazzB (site oficial tem agenda completa e confiável — https://www.jazzb.com.br/shows — usar sympla como alternativa — https://www.sympla.com.br/produtor/jazznosfundos) 

· Blue Note São Paulo (site oficial tem agenda completa e confiável — https://bluenotesp.com/shows/ — usar eventim como alternativa — https://www.eventim.com.br/artist/blue-note-sp/) 

· Casa de Francisca (site oficial tem agenda completa e confiável —
casadefrancisca.art.br/novo/programacao) 

· Cine Joia (agenda de shows —
cinejoia.com.br/agenda — usar shotgun como alternativa — https://shotgun.live/pt-br/venues/cine-joia — o site oficial costuma bloquear extração automática; se isso acontecer, tentar shotgun e, na falta de resultado, buscar diretamente por `"Cine Joia" [mês] [ano]` antes de desistir para o mês inteiro).

**Grandes instituições:**

· Theatro Municipal e Praça das Artes  (agenda de espetáculo https://theatromunicipal.org.br/programacao/)

· Sala São Paulo / Temporada Osesp (https://salasaopaulo.art.br/salasp/pt/programacao-ingressos)

· MASP (https://masp.org.br/exposicoes e https://masp.org.br/espetaculos-eventos)

· Pinacoteca (Luz, Estação, Contemporânea) (https://pinacoteca.org.br/programacao/tipo/exposicoes/ — checar sempre se uma exposição "em cartaz" anterior já não saiu de circulação antes de repeti-la)

· CCBB (https://ccbb.com.br/sao-paulo/programacao/?cartaz=mes)

· CCSP (Vergueiro) (https://centrocultural.sp.gov.br/ e https://centrocultural.sp.gov.br/agenda/)

· Cinemateca Brasileira (https://cinemateca.org.br/ e https://cinemateca.org.br/programacao/)

**Bixiga e entorno histórico:**

· Teatro Oficina (buscar também como "Teat(r)o Oficina Uzyna Uzona") (https://teatroficina.com/ e  https://site.bileto.sympla.com.br/teatrooficina/)

· Vila Itororó (checar vilaitororo.prefeitura.sp.gov.br/programacao e https://spmaiscultura.prefeitura.sp.gov.br/)

· Mundo Pensante (https://www.mundopensante.com.br/ e https://shotgun.live/pt-br/venues/mundo-pensante)

· Galeria Metrópole (https://www.instagram.com/galeriametropole.sp/ e https://metropolegaleria.com.br/)

**Cultura japonesa:**

· Japan House São Paulo (https://japanhousesp.com.br/programacao/)

· Bunkyo (https://bunkyo.org.br/br/ e https://bunkyo.org.br/br/noticias-e-eventos/eventos/.

**Feiras:**

· Jabuticaba

· Gengibrão - Rasga e Quebra

· Barbotina

· Feiras do livro

· impressos/zines/publicações independentes.

## 3. Locais sugeridos para ampliar a curadoria

Instituto Tomie Ohtake, Museu da Língua Portuguesa, Farol Santander, Auditório do Ibirapuera, Museu Afro Brasil, Galeria Ouro Velho/Ugra. Buscar antes de incluir — só entram se houver evento real no mês.

Local que aparecer na busca editorial (4.9) e render evento confirmado pode ser
promovido para esta lista na rodada seguinte — é assim que a curadoria cresce
sem virar palpite.

## 4. Metodologia de busca

1. Uma busca por local — nunca agrupar vários locais numa query só. O que é
   proibido é juntar locais; fazer **duas buscas para o mesmo local** (uma para
   o mês corrente, outra para o mês seguinte) é o esperado, e às vezes uma
   busca só já devolve os dois meses.
2. Tentar `web_fetch` direto na página
   oficial de programação. Se o ambiente bloquear `web_fetch` (proxy de
   egresso devolvendo host bloqueado), cair para busca web — e **registrar a
   limitação no relatório (seção 8)**, porque a cobertura cai muito: casas cujo
   site não está indexado em snippet ficam sem dados verificáveis.
3. Confirmar sempre o ano — muito conteúdo indexado é de anos anteriores. Quando a fonte
   mencionar o dia da semana (ex: "dia 23, sexta-feira"), cruzar com o calendário real do
   mês/ano em questão antes de usar a data — é o jeito mais confiável de pegar conteúdo
   reindexado de anos anteriores que passaria despercebido só pelo texto. Vale
   com força redobrada para o mês seguinte, onde "programação de setembro"
   de um ano anterior é fácil de confundir com a deste ano. Quando o dia da
   semana não bater com o calendário real, **descartar o item** — não tentar
   "corrigir" a data.
4. **Se a agenda do mês não estiver publicada, não inventar**: simplesmente
   não incluir esse local nesta rodada, relatando o que foi verificado no
   relatório final (seção 8). Esta regra tem prioridade sobre qualquer meta
   de cobertura daquela seção.
5. **Casas com agenda contínua (JazzB, Blue Note, Cine Joia, entre outros) devem ter
   TODOS os eventos do mês registrados individualmente — sem resumir, sem escolher só
   "os mais notáveis" para caber no app.** Se o site oficial lista 25 shows no mês, o
   `WEEKS` tem 25 entradas para aquela casa (distribuídas na semana de cada data), não
   uma linha genérica tipo "confira a agenda completa no site". Isso vale mesmo que o
   resultado fique visualmente denso — completude vem antes de economia de espaço.
6. Instagram: só posts públicos individuais que o usuário/pesquisa passar como link —
   perfis inteiros costumam bloquear sem login.
7. Para Sesc, tentar o padrão `unidade.sescsp.org.br/programacao/?data=MM/YYYY`
   quando disponível, mas validar se retornou dado real antes de usar.
8. Puxar a atração/obra principal para o início do campo `desc` (ver seção 5.1).
9. **Busca editorial geral — obrigatória nos dois modos de execução (seção 9).**
   Depois das buscas por local, fazer 1–3 buscas amplas em veículos que publicam
   roteiro semanal: `o que fazer em São Paulo neste fim de semana`,
   `agenda cultural São Paulo [mês] [ano]`, `estreias de exposições São Paulo [mês]`.
   Serve para pegar o que a curadoria fixa não cobre — estreia num espaço fora da
   lista, festival novo, ocupação temporária, feira que acontece uma vez só. Foi
   assim que o Kinoforum, que não estava em nenhuma lista, apareceu.

   **Essas fontes são de descoberta, nunca de confirmação.** Roundup de jornal e
   agregador é justamente onde mora o conteúdo reindexado de anos anteriores: um
   texto de "sexta-feira, dia 29" de outro ano passa liso se ninguém conferir.
   Todo evento que aparecer só aí precisa passar pelo cruzamento de dia da semana
   (4.3) e, de preferência, ser confirmado na fonte do próprio local antes de
   entrar. Se entrar só com fonte secundária, **dizer isso no relatório** (seção 8).

   Veículos que costumam render: Guia Folha, Veja SP, Time Out São Paulo, Catraca
   Livre, A Cidade de São Paulo, além de newsletters especializadas de cinema e
   de shows. Nenhum deles é curadoria fixa — se um sair do ar ou virar paywall,
   seguir sem ele, sem substituir por fonte pior.

## 5. Estrutura de dados (não alterar o formato)

```js
{ id: "e-slug-unico", date: "DD" ou "DD–DD", title: "Nome do evento",
  venue: "Nome do local", cat: "musica|teatro|expo|cinema|feira",
  desc: "1-2 frases com o dado mais interessante (artista convidado,
  preço especial, motivo de ser destaque)", highlight: true|false,
  url: "link direto para a página do evento, se existir" }
```

Regras:
- Nunca reutilizar ou alterar um `id` de evento que permaneça no mês seguinte
  ou já tenha sido favoritado.
- `id` novo = `e-` + slug curto e descritivo.
- Categoria (`cat`) é sempre uma das cinco suportadas — um mesmo local pode
  ter eventos de categorias diferentes no mesmo mês (ex: CCBB com exposição
  E teatro), isso não é erro, é normal.

### 5.1 Campo `url` (link do evento no app)

O app usa `url` para dois recursos: o link "Ver página do evento" no card e o
texto compartilhado no botão do WhatsApp.

- Sempre que a busca (seção 4) encontrar uma página específica do evento
  (não só da agenda geral do local), registrar o link ali — página do
  ingresso (Sympla, Eventim, Bilheteria Digital etc.), página oficial do
  evento, ou o post que confirmou a informação.
- Se só existir a agenda geral do local (sem página própria do evento),
  **não inventar uma URL nem usar a home do site**: omitir o campo `url`
  inteiramente (não usar `url: ""` nem um link genérico) — o app já trata a
  ausência do campo e simplesmente não mostra o link. Preencher com a home
  do site "só para ter alguma coisa" é pior que omitir, porque implica um
  link específico que não existe.
- Nunca reaproveitar a URL de um evento anterior para um evento novo com
  nome parecido (ex: mesma casa, artista diferente) — cada `url` precisa ter
  sido de fato encontrada para aquele evento específico nesta rodada.
- Isso vale tanto para `WEEKS` quanto para `ONGOING`.

### 5.1 Atração principal no início do `desc`
Nas primeiras palavras do `desc`, priorizar o nome do artista/obra/diretor
central, não o contexto genérico. Ex: "Hamilton de Holanda (bandolim) estreia
repertório inédito..." em vez de "Show de música instrumental...".

### 5.2 Critério para `highlight: true`
6-8 eventos por mês, escolhidos por relevância real: raridade, exclusividade
(estreia/temporada curta), relevância nacional/internacional, ou proximidade
de casa. **Não existe cota obrigatória por critério** — se o mês tiver só 4
eventos genuinamente destacáveis, o app tem 4 destaques, não 6 forçados. Isso
vale mesmo quando a pesquisa da seção 4.5 traz dezenas de eventos de agenda
contínua: a maioria entra com `highlight: false` — a cota de 6-8 é sobre
relevância, não sobre volume de eventos coletados.

A cota de 6-8 é do **mês corrente**. Do **mês seguinte**, no máximo **2**
destaques, e só para acontecimento realmente grande (ópera, grande coletiva,
estreia de temporada) — o resto da prévia entra com `highlight: false`. Como a
aba Destaques esconde o que já encerrou, o que importa é quantos ficam
*visíveis ao mesmo tempo*: mirar em não passar de ~10.

### 5.3 Formatos seguros de `period`

O app interpreta `period` por texto livre e **esconde** o item quando conclui
que a data já passou. Um `period` mal formado pode sumir com o evento no dia
seguinte à publicação. Usar só estes formatos, todos verificados:

| Formato | Exemplo | Como o app entende |
|---|---|---|
| `Em cartaz o mês todo` | — | mês corrente inteiro |
| `DD a DD/MM` | `Setembro · 18 a 26/09` | intervalo, com o mês explícito |
| `DD/MM` | `Setembro · 5/09` | um dia |
| `DD, DD e DD/MM` | `Setembro · 3, 4 e 5/09` | dias avulsos |
| `A partir de DD/MM` | `A partir de 06/08` | **só o dia DD** — some no dia seguinte; usar apenas para coisa curta |
| texto sem nenhum `DD/MM` | `estreia em setembro, no Pina Luz` | não interpretado: o app nunca esconde e omite o botão de calendário — é o fallback seguro para temporada longa ou data ainda não divulgada |

Duas armadilhas reais, já observadas:

- **`até DD/MM` de outro ano some.** `"até 25/01/2027"` é lido como 25/01 do
  ano corrente, ou seja, uma data no passado — o item desaparece. Para
  temporada que cruza o ano, usar `Em cartaz o mês todo` no começo do texto e
  escrever o fim por extenso: `"Em cartaz o mês todo, até 25 de janeiro de 2027"`.
- **Números soltos junto de um `/MM` viram lista de dias.** `"Desde 26/08, em
  cartaz até 25/01/2027"` é lido como "dias 26 e 25 de agosto" e o item some.
  Ficar nos formatos da tabela resolve.

Depois de escrever ou alterar um `period`, conferir mentalmente uma coisa só:
**este item deve continuar visível hoje?** Se a resposta for sim e o formato não
estiver na tabela, trocar por um que esteja.

## 6. Identidade do app (fixo)

- Nome do app: **Agenda Cultural SP**. Título visível no header: **Agenda
  Cultural — Centro de São Paulo** (não alterar a menos que pedido).
- `MONTH_LABEL`/`YEAR_LABEL` mudam a cada atualização; o nome e o título do
  app não.
- `LAST_UPDATED` reflete a data real da atualização, formato `DD/MM/AAAA`,
  sem contador de revisão.
- **Identidade visual "Modernist" (fixa a partir desta versão)**: fundo
  grafite (`#201e1d`) no header, tipografia Archivo (Google Fonts, pesos
  400/600/700/800) em caixa alta nos títulos, vermelho `#ec3013` como cor de
  destaque, fundo geral `#f3f2f2`. Grade de cards em `repeat(auto-fit,
  minmax(380px, 1fr))` — uma coluna em telas estreitas, duas ou mais em telas
  largas. Esse layout é fixo — não recriar do zero a cada atualização, só
  substituir os dados (`WEEKS`, `ONGOING`, `MONTH_LABEL`, `YEAR_LABEL`,
  `LAST_UPDATED`).

## 7. Priorização temporal

- Eventos na última semana de temporada (encerram em ≤7 dias) ganham nota
  "Últimos dias!" no início do `desc`.
- Eventos com datas múltiplas (ex: 06–08): mencionar no `desc` se algum dia
  específico tem preço especial ou atração extra, só quando for verdade.
- Quanto mais perto do fim do mês, mais a rodada rende no **mês seguinte** que
  no corrente — a agenda do mês corrente já está publicada e quase toda
  cumprida. Nessa altura, priorizar: (a) o que ainda acontece nos dias
  restantes, (b) a prévia do mês seguinte, (c) correções no que já está no ar.
- Nunca antecipar a virada de `MONTH_LABEL` "porque o mês está acabando" (ver
  1.1) — a virada acontece na primeira rodada do mês novo, e é ela que promove
  a prévia de `ONGOING` para `WEEKS`.

## 8. Relatório de atualização (obrigatório ao final)

Ao fim de cada atualização, reportar ao usuário. **Separar mês corrente e mês
seguinte** nos dois primeiros blocos — a cobertura dos dois é muito diferente e
misturá-las esconde buracos.

**✅ Locais confirmados** — quantos eventos por local, separando mês corrente e
prévia do mês seguinte.
**⚠️ Locais sem programação encontrada** — com nota factual do que foi
verificado (não "não encontrei nada", e sim "verifiquei X e a fonte diz Y"),
dizendo para qual dos dois meses. Indicar quais desses locais já haviam ficado
sem programação na rodada anterior e foram re-checados nesta vez (não apenas
repetidos sem nova busca) — um local pode ter publicado a agenda desde então.
**🔍 Novos locais pesquisados nesta rodada** (se houver).
**📊 Estatísticas gerais** — total de eventos, destaques (quantos do mês
corrente, quantos da prévia), fontes principais consultadas, quantos eventos
ganharam `url` própria (ver seção 5.1) vs. quantos ficaram sem por falta de
página específica.
**⚠️ Observações de acuracidade** — dado incerto, inferência feita, fonte
oficial fora do ar/substituída por alternativa, `web_fetch` bloqueado no
ambiente (ver 4.2), item descartado pelo cruzamento de dia da semana (ver 4.3).
**🔁 Migração de prévia** — nas rodadas de virada de mês, quais `id` saíram de
`ONGOING` para `WEEKS` e a confirmação de que nenhum `id` mudou.

Estas são **estatísticas descritivas do que foi encontrado**, não metas a
cumprir — se a cobertura real for menor que o esperado, isso deve aparecer
no relatório como está, não ser inflado.

---

## 9. Modos de execução

Duas rotinas agendadas usam este mesmo prompt-mestre em modos diferentes. O modo
vem dito no prompt do gatilho; **na dúvida, rodar em modo leve** — ele não
destrói nada.

### 9.1 Modo completo — dia 1º de cada mês

É a rodada que reconstrói a agenda:

1. Virar `MONTH_LABEL`/`YEAR_LABEL` para o novo mês corrente e atualizar
   `LAST_UPDATED`.
2. **Promover a prévia**: os itens do mês que está começando saem de `ONGOING` e
   entram em `WEEKS` **com o mesmo `id`**, trocando `period` por `date`
   (seção 1.1).
3. Varrer individualmente os locais da seção 2 e os da seção 3.
4. Fazer a busca editorial geral (4.9).
5. Montar a prévia do mês seguinte em `ONGOING` (seção 1.1).
6. Relatório completo da seção 8, com o bloco de migração de prévia.

### 9.2 Modo leve — diário, dias 2 a 31

Rodada barata, **aditiva e corretiva**. Pode:

- acrescentar evento novo confirmado, do mês corrente ou da prévia;
- corrigir ou anotar evento existente — cancelamento, mudança de data, elenco ou
  local, "Últimos dias!", esgotado;
- atualizar `LAST_UPDATED`, quando de fato alterar alguma coisa.

**Não pode, nunca:**

- apagar evento — o app já esconde sozinho o que encerrou (`hasEventEnded`);
- alterar `id`;
- mexer em `MONTH_LABEL`/`YEAR_LABEL` — a virada é exclusiva do modo completo;
- reescrever `WEEKS` ou `ONGOING` inteiros.

O escopo de cada rodada é a **busca editorial geral (4.9), sempre**, mais o
**grupo de locais do dia da semana**:

| Dia | Grupo |
|---|---|
| Segunda | Sesc: Pompeia, 14 Bis, Belenzinho, Avenida Paulista, 24 de Maio + guia "Em Cartaz" |
| Terça | JazzB, Blue Note, Casa de Francisca, Cine Joia |
| Quarta | Theatro Municipal/Praça das Artes, Sala São Paulo/Osesp, MASP, Pinacoteca |
| Quinta | CCBB, CCSP, Cinemateca, Japan House, Bunkyo |
| Sexta | Teatro Oficina, Vila Itororó, Mundo Pensante, Galeria Metrópole, feiras |
| Sábado | Locais da seção 3 (ampliação da curadoria) |
| Domingo | Só a busca editorial geral + revisão de acuracidade do que já está no ar |

Assim cada local é re-checado uma vez por semana e nenhuma rodada custa o que
custa a do dia 1º.

**Se nada mudou, não commitar e não notificar.** Dia sem novidade é o caso
normal do modo leve, não é falha. O relatório é curto: o que entrou, o que foi
corrigido, e o que o grupo do dia não rendeu.

---
