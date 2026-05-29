# Almara São Manoel — Landing Page Design Spec

**Data:** 2026-05-29
**Produto:** Condomínio Ecoville São Manoel, Sorriso-MT
**Entrega:** Arquivo HTML único (`almara-landing.html`) + `VIDEO SITE.mp4` na mesma pasta

---

## Objetivo

Landing page de captura de leads para o empreendimento Almara São Manoel. O diferencial central é a seção hero com vídeo controlado pelo scroll, criando uma experiência imersiva que transmite sofisticação antes do usuário ler qualquer texto.

---

## Estrutura de Seções

| # | Seção | Background | Altura |
|---|-------|-----------|--------|
| 1 | Hero (scroll-driven video) | Vídeo + overlay | 400vh scroll / 100vh viewport |
| 2 | Localização | `#382F2D` (marrom escuro) | 100vh |
| 3 | Formulário de interesse | `#F0E6DD` (creme) | 100vh |
| 4 | Rodapé | `#382F2D` (marrom escuro) | auto |

---

## Seção 1 — Hero (Scroll-Driven Video)

### Comportamento técnico
- Container scroll: `height: 400vh`, `position: relative`
- Vídeo inner: `position: sticky; top: 0; height: 100vh; width: 100%`
- Vídeo: `muted`, `playsinline`, `preload="auto"`, `object-fit: cover`
- JavaScript mapeia `scrollY` dentro do container para `video.currentTime`:
  - `progress = scrolledWithinHero / (heroHeight - viewportHeight)` (0–1)
  - `video.currentTime = progress * video.duration`
- Evento `scroll` com `requestAnimationFrame` para suavidade

### Visual
- Overlay: `rgba(0,0,0,0.35)` sobre o vídeo
- Logo ALMARA SÃO MANOEL: SVG versão principal cor creme (`#F0E6DD`), centralizado horizontalmente, posicionado a 60% da altura vertical
- Tagline em duas linhas, centralizada, abaixo do logo:
  - Linha 1: `À ALTURA DA` — Sweet Sans Pro Light, caixa alta, `letter-spacing: 0.35em`, cor creme
  - Linha 2: `SUA ` + `história` — "SUA" em Sweet Sans Pro Light + "história" em Golden Hopes (~1.6× o tamanho do sans), cor creme
- Indicador de scroll: linha vertical animada com pulso suave no rodapé da tela, opacidade 0 após primeiros 100px de scroll

---

## Seção 2 — Localização

- Background: `#382F2D`
- Título: `LOCALIZAÇÃO` — Sweet Sans Pro Light, caixa alta, `letter-spacing: 0.4em`, cor creme
- Mapa: Google Maps iframe embed (Condomínio Ecoville São Manoel, Sorriso-MT), borda sutil `#4a3f3c`, border-radius 4px
- Endereço abaixo do mapa: Sweet Sans Pro Light, creme com 70% opacidade

---

## Seção 3 — Formulário de Interesse

- Background: `#F0E6DD` (creme)
- Título: `TENHA ACESSO EXCLUSIVO` — Sweet Sans Pro Light, caixa alta, `letter-spacing: 0.35em`, cor `#382F2D`
- Formulário com 3 campos: **Nome**, **E-mail**, **Telefone**
  - Desktop: 3 colunas lado a lado
  - Mobile: empilhados
  - Estilo: border-bottom apenas (sem border-box), fundo transparente, cor `#382F2D`
- Botão: `QUERO SABER MAIS` — background `#382F2D`, texto creme, Sweet Sans Pro Medium, `letter-spacing: 0.3em`, sem border-radius
- Ação do formulário: `mailto:` placeholder (substituir por endpoint real posteriormente)

---

## Seção 4 — Rodapé

- Background: `#382F2D`
- Logo Epicentro Empreendimentos centralizado — usar texto `EPICENTRO EMPREENDIMENTOS` em Sweet Sans Pro Light como placeholder (substituir por imagem quando o arquivo for fornecido)
- Texto: `© 2026 Almara São Manoel` — Sweet Sans Pro Light, creme com 50% opacidade, tamanho pequeno

---

## Paleta de Cores

| Nome | Hex | Uso |
|------|-----|-----|
| Marrom escuro | `#382F2D` | Background principal, botão |
| Taupe | `#857B79` | Elementos secundários |
| Creme | `#F0E6DD` | Texto sobre escuro, background formulário |
| Teal | `#3F676B` | Acento (hover states) |

---

## Tipografia

| Font | Arquivo | Uso |
|------|---------|-----|
| Sweet Sans Pro Light | `SweetSansProLight.otf` | Títulos, labels, corpo |
| Sweet Sans Pro Medium | `SweetSansProMedium.otf` | Botão CTA |
| Sweet Sans Pro Regular | `SweetSansProRegular.otf` | Corpo de texto |
| Golden Hopes | `GoldenHopes.ttf` | "história" na tagline |

Todas as fontes embarcadas como base64 no `<style>` do HTML.

---

## Assets

| Arquivo | Origem | Embedding |
|---------|--------|-----------|
| Logo SVG principal | `@LOGO/VERSAO PRINCIPAL/SVG/...-01.svg` | Inline no HTML |
| `VIDEO SITE.mp4` | `Downloads/site almara/` | Referência relativa (`./VIDEO SITE.mp4`) |
| Fontes OTF/TTF | `@FONTES 2/` | base64 no `<style>` |

---

## Responsividade

- Mobile first a partir de 375px
- Breakpoint principal: 768px (tablet/desktop)
- Hero: funciona em mobile com scroll touch (iOS `playsinline` obrigatório)
- Formulário: 1 coluna em mobile, 3 colunas em desktop

---

## Fora do Escopo

- Backend / envio real do formulário (placeholder `mailto:`)
- Analytics / pixel de conversão
- SEO (meta tags básicas incluídas, sem otimização avançada)
- Animações de entrada nas seções 2 e 3 (somente o hero é animado)
