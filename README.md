# CATE · Registo Fotográfico

Página para os funcionários da CATE enviarem fotografias ao longo de um
serviço. Cada registo é classificado por **tipo de serviço** (doméstico,
hotelaria, ar condicionado), por **momento** (recolha, reparação no local,
entrada em oficina, entrega, montagem, orçamento) e, onde se aplica, pela
**garantia**.

Funciona sem servidor, sem base de dados e sem login. É só HTML, CSS e
JavaScript — as fotografias são comprimidas no próprio telemóvel e entregues
à app de email através da partilha nativa do sistema.

**Projeto autónomo.** Não depende de nenhum outro sistema, não partilha código,
base de dados nem configuração com nada. É uma pasta de ficheiros que se publica
e se usa por si.

---

## Ficheiros

| Ficheiro | Para que serve |
|---|---|
| `index.html` | A aplicação inteira |
| `logo.png` | Logótipo do cabeçalho |
| `icon-192.png`, `icon-512.png` | Ícones do ecrã principal do telemóvel |
| `manifest.webmanifest` | Faz com que abra em ecrã inteiro, sem barra do browser |

Os quatro ficheiros têm de ficar na **mesma pasta**.

---

## Publicar

A página precisa de estar em **HTTPS** — a partilha de ficheiros não funciona
em `http://` nem a abrir o ficheiro diretamente do telemóvel.

**GitHub Pages** (o que está em uso):

1. Criar o repositório `registo-fotografico` em <https://github.com/new>,
   **público** (o Pages só é gratuito em repositórios públicos).
2. `git push -u origin main`
3. No repositório: *Settings* → *Pages* → *Source: Deploy from a branch* →
   ramo `main`, pasta `/ (root)` → *Save*.
4. Ao fim de um minuto fica em <https://joaocgos.github.io/registo-fotografico/>.

Cada `git push` para `main` republica o site automaticamente.

O ficheiro `.nojekyll` existe para o GitHub servir os ficheiros tal como estão,
sem os passar pelo Jekyll.

**Netlify Drop** é a alternativa sem repositório público: abrir
<https://app.netlify.com/drop> e arrastar a pasta para a página.

---

## Instalar no telemóvel do técnico

1. Abrir o endereço no browser (Chrome no Android, Safari no iPhone).
2. Menu do browser → **Adicionar ao ecrã principal**.
3. Passa a abrir como uma app, com ícone próprio e sem barra de endereço.

Convém ainda **guardar o endereço de destino nos contactos** do telemóvel.
Depois do primeiro envio, a app de email passa a sugerir o endereço sozinha e
o Android chega a mostrá-lo diretamente no menu de partilha.

---

## Como se usa

1. Escolher o **tipo de serviço**, depois o **momento** e, se se aplicar, a
   **garantia**. As escolhas aparecem uma a uma, e cada uma feita encolhe para
   uma linha com "alterar". O resto do formulário só aparece no fim — assim não
   é possível preencher tudo e descobrir que faltava classificar o registo.
2. Escrever o número do serviço e quem regista — o nome fica guardado no
   telemóvel e não é preciso repetir.
3. Fotografar as **etiquetas do equipamento** (marca, modelo, número de série).
   Cabem várias, para equipamentos com mais do que uma chapa.
4. Fotografar o **equipamento** — pela câmara ou escolhendo da galeria.
5. **Partilhar registo** → abre a partilha do telemóvel → escolher o email →
   **carregar em enviar dentro da app de email**.

O assunto sai já preenchido no formato `[RECOLHA] Serviço 26000123`, e o corpo
leva o técnico, a data e a hora a que cada fotografia foi realmente tirada.

### O botão partilha, não envia

Uma página web não consegue enviar um email sozinha nem saber se ele chegou a
sair. O que faz é entregar as fotografias à app de email, com o assunto e o
texto preenchidos — o envio tem mesmo de ser confirmado lá dentro.

Por isso o botão diz *Partilhar registo* e o ecrã final diz *Falta enviar o
email*. Nenhum dos dois promete o que a página não pode garantir.

Se a partilha correr mal — a app de email fechou, o rascunho perdeu-se,
escolheu-se a app errada — o ecrã final tem **Partilhar outra vez**, que repete
tudo com as mesmas fotografias. Estas só desaparecem quando se toca em
*Já enviei — novo registo*.

### Folha de serviço

Quando há um número de serviço preenchido — obrigatório ou não — aparece um
cartão para fotografar a folha de serviço. É opcional, cabem quatro fotografias
(frente, verso, páginas seguintes), e os ficheiros saem como
`26000123_recolha_dom_gar_folha_01.jpg`.

Ao contrário das etiquetas e das fotografias, a folha pertence ao **serviço** e
não a um equipamento: por isso tem cartão próprio e o nome do ficheiro nunca
leva o prefixo `eq`, mesmo num registo com vários equipamentos.

Sem número de serviço o cartão não aparece — não haveria folha a que se referir.

### Fatura

Nos registos **em garantia** aparece um cartão para fotografar a fatura de
compra — é o que prova que o equipamento está coberto. É opcional e cabem três
fotografias. Se o cliente tiver a fatura no telemóvel, fotografa-se o ecrã. Os
ficheiros saem como `26000123_recolha_dom_gar_fatura_01.jpg`.

Tal como a folha de serviço, é do serviço e não de um equipamento: nunca leva o
prefixo `eq`. Não aparece em "fora de garantia" nem no Orçamento.

### O que fica escondido não é enviado

Algumas partes do formulário aparecem e desaparecem conforme as escolhas: a
folha precisa de número de serviço, a fatura precisa de "em garantia", e o
Orçamento troca os blocos de equipamento pelas fotografias do local. Se uma
dessas partes tiver fotografias e depois desaparecer — por exemplo, mudar para
Orçamento depois de fotografar equipamentos — essas fotografias **ficam de fora**
do email e do total. Só vai o que o técnico está a ver.

### Vários equipamentos

Um registo pode cobrir mais do que um equipamento — típico em hotelaria, onde
se vai lá uma vez e se mexe em cinco máquinas. O contador no cartão
*Equipamentos* faz aparecer um bloco por cada, com etiquetas e fotografias
próprias.

Isto existe para quem recebe o email conseguir dizer que fotografia pertence a
que máquina. Sem os blocos, cinco equipamentos dariam um monte único de fotos
indistinguíveis.

Baixar o contador não apaga nada em silêncio: se o último bloco tiver
fotografias, o contador recusa e pede que sejam removidas primeiro. E ao
partilhar, um bloco vazio no meio dos outros é assinalado pelo nome
("O equipamento 2 não tem fotografias") em vez de passar despercebido.

### As etiquetas

Cabem várias por equipamento — há máquinas com mais do que uma chapa, e às
vezes é preciso repetir uma que saiu tremida.

Se o serviço já tiver levado etiquetas noutro registo, aparece um aviso a
dizer em qual e quando. É só informação: não desaconselha repetir, porque uma
etiqueta a mais nunca fez mal e uma a menos obriga a voltar ao equipamento.
Esse aviso vive no telemóvel de quem as enviou, por isso outro técnico não o vê.

Quando um registo segue sem etiquetas, o email leva sempre uma linha a dizer
porquê — `Etiquetas: nenhuma neste registo — já enviadas no registo de Recolha
(07/09/2026 14:02).` ou `Etiquetas: nenhuma neste registo.` — para quem arquiva
distinguir uma omissão deliberada de um esquecimento.

### Sem rede em casa do cliente

Não é problema. O técnico tira as fotografias com a câmara normal do telemóvel
e mais tarde, já com rede, envia-as pela galeria. A página lê a data original
de cada fotografia (EXIF) e é essa que vai no email — fica sempre registada a
hora a que se esteve com o equipamento, não a hora do envio.

---

## Afinações

Tudo o que se mexe está no bloco `CONFIG`, no início do `<script>` do
`index.html`:

```js
const CONFIG = {
  EMAIL_CAIXA: "recolhas",          // o endereço é montado em tempo de
  EMAIL_DOMINIO: "cate.com.pt",     // execução, ver nota abaixo
  MAX_FOTOS: 6,         // fotografias POR equipamento
  MAX_ETIQUETAS: 2,     // etiquetas POR equipamento
  MAX_EQUIPAMENTOS: 20, // equipamentos declaráveis num registo
  AVISO_TAMANHO: 8 * 1024 * 1024,  // a partir daqui avisa que o registo é pesado
  MAX_LADO: 1280,     // píxeis no lado maior
  QUALIDADE: 0.72,    // 0 a 1
  ENDPOINT: null,
};
```

**Tipos, momentos e garantia** — as listas `SERVICOS`, `MOMENTOS` e `GARANTIAS`,
logo a seguir ao `CONFIG`. São independentes: uma recolha doméstica tanto pode
ser em garantia como fora dela. Para acrescentar, remover ou reordenar mexe-se
só aí — os botões, o prefixo do assunto, o corpo do email e os nomes dos
ficheiros são todos gerados a partir delas. As entradas de `SERVICOS` e
`GARANTIAS` levam também `curta`, o código que vai nos nomes dos ficheiros.

Cada entrada precisa de `chave` (vai no nome do ficheiro), `marca` (prefixo do
assunto, sem acentos), `nome`, `texto` (corpo do email), `icone` e, se fizer
sentido, `detalhe` para a segunda linha do botão — que pode ficar vazia.

**Exceções por momento.** Nem todos os momentos precisam das mesmas coisas, e
essas diferenças declaram-se na própria entrada em vez de ficarem espalhadas
pelo código. Existem duas marcas:

    numeroOpcional: true
    etiquetaObrigatoria: true
    campoRecebidoPor: true
    ajudaEtiquetas: "..."
    ajudaFotos: "..."
    semEquipamentos: true
    clienteSempre: true
    camposContacto: true
    campoPretendido: true
    semGarantia: true

Está ligada nos três momentos em que o serviço pode ainda não existir: a
**Recolha** e a **Reparação no local** (vai-se a casa do cliente e o serviço é
aberto depois) e o **Orçamento** (as fotografias servem justamente para o
*produzir*).

Fica desligada na **Entrada Oficina**, na **Entrega** e na **Montagem**, onde o
serviço já existe — exigir o número é o que garante que o registo fica ligado
ao processo.

Com a marca ligada, o rótulo passa a "Número do serviço (opcional)" e aparece o
campo **Nome do cliente** — que fica obrigatório se o número ficar vazio. Um
registo tem sempre de ser atribuível a alguém: ou pelo serviço, ou pelo cliente.

`etiquetaObrigatoria` está ligada na **Entrada Oficina** e na **Montagem**: é pela chapa que a
oficina identifica a máquina que acabou de entrar, por isso não se deixa
partilhar sem ela. O cartão passa a dizer "Etiquetas · obrigatória" e, se
faltar, a validação aponta o equipamento em causa. Nos outros momentos as
etiquetas continuam opcionais — há equipamentos sem chapa legível, e obrigar
criaria um beco sem saída.

`ajudaEtiquetas` e `ajudaFotos` substituem o texto de ajuda dessas secções. O
mesmo campo pede coisas diferentes conforme o momento: na Entrada Oficina a
chapa serve para identificar a máquina que entrou, na Montagem é o que dá acesso
à garantia do equipamento novo. E num equipamento acabado de instalar não há
danos anteriores a documentar, por isso a Montagem pede "o equipamento instalado
e as ligações feitas" em vez de "o estado do equipamento e qualquer dano
visível". Quando o momento não define nenhum texto, usa-se o genérico.

`campoRecebidoPor` está ligada na **Entrega** e na **Montagem**: faz aparecer um campo opcional
"Recebido por". Numa entrega, quem recebeu é parte da prova — sem isso o registo
mostra o estado do equipamento mas não que chegou às mãos de alguém. Uma
instalação acaba da mesma maneira, com o equipamento entregue a funcionar. Fica
opcional para não travar a partilha quando não se soube. O valor só entra no
email nos momentos onde o campo existe.

### O Orçamento é outra coisa

As últimas quatro marcas existem por causa dele. O Orçamento não é um registo de
equipamento: é um **levantamento do local** para depois se planear a execução.
Não há máquina para identificar, logo não há blocos de equipamento nem
etiquetas — `semEquipamentos` troca tudo isso por um cartão único,
"Fotografias do local".

Em contrapartida, o que quem orçamenta precisa é de saber **de quem** e **onde**,
e sobretudo **o que lhe estão a pedir**: `clienteSempre` exige o nome mesmo
havendo número de serviço, `camposContacto` acrescenta morada (obrigatória) e
contacto (opcional), e `campoPretendido` acrescenta um campo obrigatório
"O que o cliente pretende". Quem vai pôr um preço nas fotografias não esteve lá;
sem isto, veria imagens sem saber o que se lhe pede.

Também não tem garantia (`semGarantia`): um levantamento de local não tem
estado de garantia, por isso o seletor nem aparece. Os ficheiros saem sem `eq`,
sem `etiqueta` e sem garantia: `26000123_orcamento_dom_01.jpg`.

Sem número, o assunto identifica pelo cliente
(`[RECOLHA · DOMESTICO · GARANTIA] Cliente: Maria Fernandes`) e os ficheiros levam um
carimbo de data e hora à cabeça (`20260909_2322_recolha_dom_gar_01.jpg`), para
que dois registos do mesmo tipo no mesmo dia não deem ficheiros com o mesmo
nome.

**Limites por equipamento** — `MAX_FOTOS` e `MAX_ETIQUETAS` valem para *cada*
equipamento, não para o registo todo: com três equipamentos declarados cabem
três vezes isto. Os valores são provisórios; ajustar com o uso real.

**Tamanho dos ficheiros** — com 1280 px e qualidade 0.72, uma fotografia de
telemóvel passa de 3–5 MB para cerca de 120–180 KB, e continua a deixar ler o
número de série de uma etiqueta. Um registo completo (2 etiquetas + 4 fotos) fica
à volta de 600 KB. Se for preciso mais detalhe, subir `MAX_LADO` para 1600.

---

## Envio automático (para mais tarde, se fizer falta)

Neste momento o técnico tem de escolher a app de email no menu de partilha e
confirmar o envio lá dentro. Não é envio automático a sério, e a página não
consegue saber se a mensagem chegou a sair.

Para eliminar esse passo é preciso um endpoint que receba as fotografias e as
reencaminhe por email. Basta apontar `CONFIG.ENDPOINT` para o seu URL e o botão
passa a enviar sozinho — o resto da página já está preparado. O endpoint recebe
um `multipart/form-data` com os campos `para`, `assunto`, `corpo`, `numero`,
`momento`, `tipo`, `garantia` (só quando se aplica) e as imagens em `fotos`.

Nesse modo o ecrã final passa a dizer *Registo enviado*, porque aí a resposta
do servidor é confirmação a sério.

Só vale a pena se a fricção se revelar um incómodo real: obriga a uma conta de
serviço de email e a uma chave guardada no servidor.

---

## Nome dos ficheiros

Com um só equipamento:

    26000123_oficina_dom_fg_etiqueta_01.jpg
    26000123_oficina_dom_fg_01.jpg

Com vários, entra o número do equipamento — que só aparece quando é preciso,
para não alongar o nome no caso comum:

    26000123_recolha_hot_gar_eq1_etiqueta_01.jpg
    26000123_recolha_hot_gar_eq2_01.jpg

O assunto vai por extenso, porque é lido por pessoas:
`[OFICINA · DOMESTICO · FORA GARANTIA] Serviço 26000123`. Nos ficheiros, o tipo
e a garantia entram por códigos curtos — `dom`, `hot`, `ac` e `gar`, `fg` —
porque é o que mais pesa no comprimento do nome.

O padrão é fixo e previsível de propósito: se algum dia se quiser arquivar esta
caixa de correio automaticamente, o número do serviço e o tipo lêem-se do
nome do ficheiro e do assunto, sem depender de nada do lado da página.

---

## Segurança e privacidade

A app não tem servidor, não tem base de dados e não guarda fotografias em lado
nenhum: elas existem na memória do telemóvel até serem entregues à app de email.
Não há dependências externas nem código de terceiros — o que reduz a superfície
de ataque a quase nada.

**As fotografias não levam a morada do cliente.** A recompressão passa a imagem
por um `canvas`, o que descarta todos os metadados do original — incluindo as
coordenadas GPS que os telemóveis gravam. A data em que a fotografia foi tirada
é lida *antes* disso e vai no texto do email, por isso guarda-se o que é útil e
deita-se fora o que exporia a casa do cliente. Verificado com uma imagem de
teste com GPS: sai sem qualquer bloco EXIF.

**O endereço de destino não aparece por extenso no código.** É montado em tempo
de execução a partir de duas partes, e a página traz `noindex` e um
`robots.txt`. Isto trava os robôs que recolhem emails para spam e mantém a
página fora dos motores de busca. Não é segredo — quem abrir a página vê o
endereço no rodapé — mas deixa de ser apanhável automaticamente.

**A página é aberta a quem tiver o endereço.** Não tem autenticação, por opção:
pôr logins nos telemóveis dos técnicos custaria mais do que protege. Quem
descobrir o URL consegue produzir um email com o formato certo — mas consegue
igualmente escrever um email à mão para a mesma caixa, por isso a app não
acrescenta poder nenhum a um atacante.

> **Importante para o futuro:** se algum dia se arquivar esta caixa
> automaticamente, **não confiar no assunto nem no nome dos ficheiros** para
> decidir o que é legítimo. Esses são falsificáveis por qualquer pessoa. O único
> sinal de confiança é o **remetente** do email, que deve ser validado contra
> uma lista de endereços conhecidos dos técnicos.

**Riscos residuais assumidos:**

- O nome do técnico é escrito à mão e não é verificado. Quem assina de facto é a
  conta de email que envia.
- O `localStorage` guarda o nome do técnico e os números de serviço que já
  levaram etiqueta. Num telemóvel partilhado, outra pessoa com acesso ao mesmo
  browser consegue lê-los.
- A página pode ser embebida noutro site (`frame-ancestors` só se define por
  cabeçalho HTTP, que o GitHub Pages não deixa configurar). Sem ações
  privilegiadas na página, o proveito para um atacante é nulo.

---

## Compatibilidade

A partilha de ficheiros funciona no Chrome do Android e no Safari do iPhone
(iOS 15+). Em telemóveis onde não exista, a página descarrega as fotografias já
comprimidas e abre o email com o destinatário e o assunto preenchidos, ficando
só a faltar anexá-las.
