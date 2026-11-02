# Arquivo — layout e contrato

Este documento é a especificação do layout do repositório do Arquivo (Archive):
onde cada coisa mora, o que é rascunho e o que é publicado, e a regra "dura" de
imutabilidade.

## O que mora aqui

    clippings/<id>/clipping.json   um Recorte por diretório
    clippings/<id>/<hash>          mídia de prova pequena, comprometida no git
    media/<hash>                   mídia pesada, fora do git, só o nome no JSON
    manifest.json                  os IDs dos Recortes publicados

O diretório de cada Recorte leva o próprio id, e `clipping.json` segue o
formato do `schema` — o mesmo que o validador lê. Nada entra no JSON que o
schema não conhece. Esse `schema` é um contrato em Go (que vive no repositório
da aplicação).

## Transcrição e mídia

A transcrição vai embutida no `clipping.json` (campo `transcript`), porque ela
é a prova textual mínima: qualquer clone, sem rede nenhuma, já carrega o
conteúdo dito. O que acompanha o Recorte no git é a prova visual pequena/leve —
prints, mídias e outros (`other`) —, guardada em `clippings/<id>/` com o
próprio hash como nome.

Vídeo e áudio ficam fora do git, em `media/`, sempre com o hash do conteúdo
como nome do arquivo: conferir integridade não exige manifesto nenhum,
`hash(arquivo) == nome` e pronto (ADR 0002). A régua aqui pra saber se entra no
repo ou não é 10 MiB: imagens/vídeos maiores que esse tamanho é considerado
"mídia pesada" e vai para `media/`, porque o repositório com o Arquivo não pode
custar caro de clonar.

## Licença

O conteúdo do Arquivo é CC BY-SA 4.0, o código deste repositório é MPL 2.0; a
decisão toda está no spec do repositório do Arquivo.
