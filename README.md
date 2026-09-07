# CATE · Registo de Recolhas

Página para os técnicos dos serviços de rua enviarem fotografias dos
equipamentos na **recolha em casa do cliente** e na **entrada em oficina**.

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

1. Criar o repositório `registo-recolhas` em <https://github.com/new>,
   **público** (o Pages só é gratuito em repositórios públicos).
2. `git push -u origin main`
3. No repositório: *Settings* → *Pages* → *Source: Deploy from a branch* →
   ramo `main`, pasta `/ (root)` → *Save*.
4. Ao fim de um minuto fica em <https://joaocgos.github.io/registo-recolhas/>.

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

1. Escrever o número do serviço (só dígitos, sem limite de comprimento) e o
   nome do técnico
   — o nome fica guardado e não é preciso repetir.
2. Escolher o momento: **Recolha** ou **Oficina**.
3. Fotografar a **etiqueta do equipamento** (marca, modelo, número de série).
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

### A etiqueta na oficina

A etiqueta nunca é obrigatória. Ao escolher **Oficina**, o cartão passa a estar
marcado como opcional, e se aquele serviço já tiver levado etiqueta na recolha,
aparece um aviso a dizer que não é preciso repetir.

Esse aviso vive no telemóvel de quem enviou a recolha: se for um técnico a
recolher e outro a dar entrada na oficina, o segundo não o vê. É por isso uma
sugestão, nunca um impedimento.

Quando um registo segue sem etiqueta, o email leva sempre uma linha a dizer
porquê — `Etiqueta: enviada no registo de recolha (07/09/2026 14:02).` ou
`Etiqueta: não incluída neste registo.` — para quem arquiva conseguir
distinguir uma etiqueta dispensada de uma esquecida.

### Sem rede em casa do cliente

Não é problema. O técnico tira as fotografias com a câmara normal do telemóvel
e mais tarde, já com rede, envia-as pela galeria. A página lê a data original
de cada fotografia (EXIF) e é essa que vai no email — fica sempre registada a
hora da recolha, não a hora do envio.

---

## Afinações

Tudo o que se mexe está no bloco `CONFIG`, no início do `<script>` do
`index.html`:

```js
const CONFIG = {
  EMAIL_CAIXA: "recolhas",          // o endereço é montado em tempo de
  EMAIL_DOMINIO: "cate.com.pt",     // execução, ver nota abaixo
  MAX_FOTOS: 12,      // fotografias do equipamento (a etiqueta conta à parte)
  MAX_LADO: 1280,     // píxeis no lado maior
  QUALIDADE: 0.72,    // 0 a 1
  ENDPOINT: null,
};
```

**Limite de fotografias** — `MAX_FOTOS`. Está em 12 por ser um valor folgado
para um registo completo. Se na prática ninguém passar de 5, baixa-se; se
faltarem, sobe-se. É o único sítio a alterar.

**Tamanho dos ficheiros** — com 1280 px e qualidade 0.72, uma fotografia de
telemóvel passa de 3–5 MB para cerca de 120–180 KB, e continua a deixar ler o
número de série de uma etiqueta. Um registo completo (etiqueta + 4 fotos) fica
à volta de 600 KB. Se for preciso mais detalhe, subir `MAX_LADO` para 1600.

---

## Envio automático (para mais tarde, se fizer falta)

Neste momento o técnico tem de escolher a app de email no menu de partilha e
confirmar o envio lá dentro. Não é envio automático a sério, e a página não
consegue saber se a mensagem chegou a sair.

Para eliminar esse passo é preciso um endpoint que receba as fotografias e as
reencaminhe por email. Basta apontar `CONFIG.ENDPOINT` para o seu URL e o botão
passa a enviar sozinho — o resto da página já está preparado. O endpoint recebe
um `multipart/form-data` com os campos `para`, `assunto`, `corpo`, `servico`,
`momento` e as imagens em `fotos`.

Nesse modo o ecrã final passa a dizer *Registo enviado*, porque aí a resposta
do servidor é confirmação a sério.

Só vale a pena se a fricção se revelar um incómodo real: obriga a uma conta de
serviço de email e a uma chave guardada no servidor.

---

## Nome dos ficheiros

Os anexos saem como `26000123_recolha_etiqueta.jpg`, `26000123_recolha_01.jpg`,
`26000123_oficina_01.jpg` — ou seja, `{serviço}_{momento}_{n}.jpg`.

O padrão é fixo e previsível de propósito: se algum dia se quiser arquivar esta
caixa de correio automaticamente, o número do serviço e o momento lêem-se do
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
