# Arquivo — layout e contrato

Este é o contrato do repositório do Arquivo (Archive): o que mora onde, o
que é rascunho, o que é publicado e o que não pode ser reescrito. O texto
canônico é mantido junto do código, em `docs/archive/ARCHIVE.md`; a cópia
na raiz do Arquivo deve ser idêntica.

## Dois repositórios, nenhum dentro do outro

O projeto tem dois repositórios git separados:

- o repositório de código: CLI, renderização do mural, validação. Não
  guarda Recorte nem mídia.
- o Arquivo: `clippings/`, `media/`, `manifest.json` e este contrato.

Nenhum está dentro do outro, e não há submódulo. O código abre um
checkout do Arquivo por caminho — `clipping new` e `clipping release`
exigem um checkout local — ou clona uma URL git num cache, que é o que
`clipping check` e `clipping build` fazem quando recebem um endereço.
Qualquer Espelho serve: a identidade do Arquivo é o conteúdo, não o
endereço.

## O que mora aqui

    clippings/<id>/clipping.json   um Recorte por diretório
    clippings/<id>/<hash>          mídia de prova pequena, comprometida no git
    media/<hash>                   mídia pesada, fora do git, referida no JSON
    manifest.json                  os IDs dos Recortes publicados

O diretório de cada Recorte leva o próprio id, e `clipping.json` segue o
formato do pacote `schema` — o mesmo que o validador lê. Nada entra no
JSON que o schema não conhece.

## Transcrição e mídia

A transcrição vai embutida no `clipping.json` (campo `transcript`):
é a prova textual mínima, e qualquer clone, mesmo sem rede, já carrega
a frase dita.

A prova visual pequena — print, frame e o que mais for `image` ou `other`
até 10 MiB — entra no git, em `clippings/<id>/`, com o próprio hash como
nome. Vídeo, áudio e qualquer arquivo acima de 10 MiB ficam fora do git,
em `media/`, também nomeados pelo hash do conteúdo.

O `clipping.json` guarda só a referência, e a referência é o hash, não um
caminho nem uma URL. Como o nome do arquivo é o hash, conferir
integridade não exige manifesto: basta hashear os bytes e comparar com o
nome. E o mesmo arquivo tem o mesmo nome em qualquer lugar — dentro do
git, em `media/`, num Espelho, num IPFS —, então mover mídia de camada é
copiar os bytes para outro lugar, sem tocar no JSON.

## Rascunho e publicação

Um Recorte entra como rascunho, direto na árvore de trabalho, com o
diretório dele e os arquivos de mídia que lhe tocarem. Rascunho é o que
não consta no `manifest.json`:

    { "published": ["politico-declara-suporte-1hx5ca"] }

ID na lista exige que o diretório dele exista no disco; fora da lista,
Recorte é rascunho legítimo — o mural não o mostra, mas o validador
continua cobrando os mesmos deveres de prova. Publicar é atualizar a
lista no `manifest.json` e etiquetar a árvore do git: a etiqueta é o
evento de publicação, e o `manifest.json` da árvore etiquetada é quem
responde por quem está publicado. `clipping release` faz as duas coisas
juntas, recusando árvore suja, e `--dry-run` mostra o plano sem escrever;
`clipping new` só escreve rascunho.

## Imutabilidade

A coisa que não muda é a árvore etiquetada: o que uma etiqueta mostra
continua mostrando para sempre, este é o histórico do git. Sobre o resto:

1. Recorte publicado entra em lançamento, e cada lançamento carrega
   sempre a versão atual dele; a versão de um lançamento anterior fica
   alcançável pela etiqueta antiga.
2. Correção é commit novo, nunca reescrita: o commit que altera um
   `clipping.json` publicado cita, na mensagem, o hash do arquivo
   anterior. Versão velha e versão nova convivem no histórico.
3. Retratação não é Recorte novo: a marca entra no recorte do Apoio
   original, em lançamento posterior, pelo mesmo caminho da correção.

## O gancho de validação

O validador é o pacote `check`, comando `clipping check` (exit `0`
limpo, `1` achados, `2` erro operacional). O CI do Arquivo roda o mesmo
comando a cada push; a lista completa de deveres de prova está no
próprio pacote. É esse CI também quem guarda a imutabilidade: um Recorte
publicado só se altera em commit que cita o hash anterior — divergência
sem referência é build vermelho, não debate.

## Licença

O conteúdo do Arquivo é CC BY-SA 4.0; o código que o lê e o renderiza é
MPL 2.0. As licenças não se misturam: clonar o Arquivo não traz código,
e clonar o código não traz conteúdo.
