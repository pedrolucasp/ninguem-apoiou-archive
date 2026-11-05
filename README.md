# Arquivo — Ninguém apoiou

> No futuro, todo mundo vai dizer que nunca apoiou.

Este repositório é o Arquivo: o registro canônico de recortes — atos datados
de apoio público a figuras da extrema-direita, um ato por recorte, com fonte,
transcrição e prova. Ele mora num repositório próprio, separado do código que
o lê, o valida e o renderiza, porque prova não guarda a si mesma quando mora
junto de quem a processa.

## Layout e regras

O contrato de layout, rascunho/publicação e imutabilidade está em
[ARCHIVE.md](ARCHIVE.md). Em resumo:

    clippings/<id>/clipping.json   um Recorte por diretório
    clippings/<id>/<hash>          mídia de prova pequena, comprometida no git
    media/<hash>                   mídia pesada, fora do git, referida no JSON
    manifest.json                  os IDs dos Recortes publicados

As decisões de fundo são ADRs do repositório de código
(github.com/pedrolucasp/ninguem-apoiou):
[ADR 0001](https://github.com/pedrolucasp/ninguem-apoiou/blob/trunk/docs/adr/0001-archive-registry-split.md)
— o Arquivo num repositório próprio, todo espelho vale o original — e
[ADR 0002](https://github.com/pedrolucasp/ninguem-apoiou/blob/trunk/docs/adr/0002-hash-named-tiered-media.md)
— mídia em camadas, nomeada pelo próprio hash.

## Clonando

    git clone <url deste repositório>

Qualquer Espelho vale tanto quanto o original: endereço não faz parte da
identidade, e nenhum código depende de um URL canônico. Clones completos em
outros sistemas git e no Software Heritage são o próprio Arquivo, não cópias
de segurança.

## Licença

O conteúdo do Arquivo é CC BY-SA 4.0 (LICENSE). O código que lê, valida,
renderiza e publica o Arquivo está sob MPL 2.0 no repositório de código.
