# lumia-config

Configuração pública usada pelos apps **Música Direta** e **Tubo** do Lumia (repositório `lumia-app`).

O app baixa o arquivo `youtube.json` ao abrir. Quando o YouTube muda algo e as músicas
param de tocar, basta atualizar este arquivo — o app pega a mudança sozinho, sem reinstalar.

Não há nada pessoal aqui: só nomes de aparelhos, versões de apps e parâmetros públicos
que o próprio [yt-dlp](https://github.com/yt-dlp/yt-dlp) usa.

## Campos do `youtube.json`

| Campo | O que é |
|---|---|
| `versao` | número que sobe a cada atualização (o app mostra em "ajustes") |
| `musica` | como o app se apresenta ao YouTube Music para **buscar e navegar** |
| `web` | como o Tubo se apresenta ao YouTube "normal" (busca, canais, comentários) |
| `player` | lista de "aparelhos" que o app finge ser para **pegar o áudio**. Ele tenta na ordem; se um falhar, tenta o próximo |
| `formatosAudio` | formatos de áudio aceitos (o Lumia toca `audio/mp4`, ou seja, AAC) |
| `formatosVideo` | formatos de vídeo aceitos (o Lumia só decodifica H.264, `avc1`) |
| `tubo` | códigos dos filtros de busca e da aba de vídeos dos canais, e o endereço do SponsorBlock |
| `busca` | códigos dos filtros de busca (músicas, vídeos, álbuns...) |

## Como atualizar quando o YouTube mudar

1. Veja o que o yt-dlp mais recente usa: arquivo
   `yt_dlp/extractor/youtube/_base.py` (lista `INNERTUBE_CLIENTS`) e, em `_video.py`,
   `_DEFAULT_JSLESS_CLIENTS` (os clientes que funcionam **sem JavaScript**, que é o
   caso do Lumia).
2. Copie `clientName`, `clientVersion`, `userAgent` e os outros campos do cliente para
   uma entrada de `player` (o número `INNERTUBE_CONTEXT_CLIENT_NAME` vai em `clientNameId`).
3. Aumente `versao`, mude `atualizado`.
4. Teste no Mac antes de publicar, com a ferramenta do `lumia-app`:
   `python ferramentas/verificar_youtube.py --config /caminho/para/youtube.json` (Música Direta)
   `python ferramentas/verificar_tubo.py --config /caminho/para/youtube.json` (Tubo)
