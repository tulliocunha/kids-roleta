# Globoplay Kids — Abordagem 2 (Roleta)

Protótipo de TV (1920×1080, escalado por transform) do Globoplay Kids, versão Roleta.
Navegação por D-pad (setas / OK / Voltar) e por clique.

## Testar localmente

O arquivo carrega assets por caminho relativo, então precisa de um servidor HTTP
(abrir o `index.html` direto do disco bloqueia os fetches).

    cd pacote-roleta
    python3 -m http.server 8000
    # abra http://localhost:8000

## Subir no GitHub Pages

1. Crie um repositório e envie **o conteúdo desta pasta** na raiz do branch.
2. Settings → Pages → Source: `main` / `/ (root)`.
3. O `.nojekyll` já está incluído (o Jekyll ignoraria as pastas com `_`, como `_ds/`).

## Estrutura

    index.html   protótipo (uma única página, seis telas)
    support.js   runtime
    _ds/         Playkit 3.0 — Globoplay Design System (tokens + bundle)
    icons/       ícones dos universos e do seletor
    posters/     capas (bg) e logos dos títulos
    uploads/     artes dos IPs, stickers, avatares, preview.mp4, clipe do aviso

## Telas

perfis → idade → home (roleta de universos + leque de cartazes) → busca → ver tudo → player.
Busca e Ver tudo são telas irmãs da home: entram pela esquerda e pela direita, com o menu
superior, o avatar e o logo fixos por cima da transição.

## Controles

- **↑ / ↓** : alterna entre menu superior, leque de cartazes e roleta de universos
- **← / →** : cartazes do universo; no menu superior, troca de tela na hora
- **OK / Enter** : assistir, entrar no universo, digitar no teclado da busca
- **Voltar / Esc / Backspace** : volta uma tela (na busca, apaga uma letra)

## Notas para navegador de TV

- Teclas aceitas por `e.key` **e** por `keyCode`: 37–40 (direcionais), 13 (OK), 27, 8,
  **10009** (Voltar do Tizen/Samsung), **461** (Voltar do webOS/LG), 166 (Roku/genérico).
- O listener fica em `window` e chama `preventDefault()`, então a TV não rola a página
  nem move o foco nativo do navegador junto com o foco do protótipo.
- Nenhum elemento entra na ordem de foco nativa (`tabIndex` só negativo) e `*:focus{outline:none}`
  desliga o anel do navegador: o único foco visível é o do protótipo — anel branco de 4px
  pulsando entre 100% e 10% de opacidade, do lado de fora do container.
- Todo alvo focável também responde a clique/toque, para testar com mouse ou controle
  com ponteiro (Magic Remote da LG).
- O palco reescala em `resize`/`orientationchange` e em cinco tentativas depois do load
  (TVs relatam o viewport com atraso). Funciona em 1280×720 e 1920×1080.
- Vídeos (`preview.mp4`, clipe do aviso de dormir) só são montados se o arquivo decodificar;
  se a TV não tocar o formato, a arte estática continua no lugar.
