# Almara Landing Page — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar um arquivo HTML único (`almara-landing.html`) com vídeo controlado por scroll, seção de localização e formulário de interesse, usando a identidade visual completa da marca Almara São Manoel.

**Architecture:** Arquivo HTML único com fontes OTF/TTF embutidas como base64 no `<style>`. O vídeo é referenciado como caminho relativo (`./VIDEO SITE.mp4`). O efeito scroll-driven usa `video.currentTime` mapeado ao progresso do scroll via `requestAnimationFrame`.

**Tech Stack:** HTML5, CSS3 (Custom Properties, Grid, sticky positioning), JavaScript vanilla (scroll event + rAF), Google Maps iframe embed.

**Assets de origem (todos em `/Users/lucasnetto/Downloads/site almara/`):**
- Fontes: `@FONTES 2/SWEET SANS PRO/SweetSansProLight.otf`, `SweetSansProMedium.otf`
- Fonte script: `@FONTES 2/GOLDEN HOPES/GoldenHopes.ttf`
- Logo SVG: `@LOGO/LOGO ALMARA - VERSAO PRINCIPAL/SVG/LOGO ALMARA - VERSAO PRINCIPAL-03.svg` (creme sobre escuro)
- Vídeo: `VIDEO SITE.mp4`

**Entrega:** `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html` + `VIDEO SITE.mp4` na mesma pasta.

---

### Task 1: Setup da pasta e cópia do vídeo

**Files:**
- Create: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html` (arquivo vazio inicial)
- Copy: `/Users/lucasnetto/Desktop/almara-landing/VIDEO SITE.mp4`

- [ ] **Step 1: Copiar o vídeo para a pasta de destino**

```bash
cp "/Users/lucasnetto/Downloads/site almara/VIDEO SITE.mp4" \
   "/Users/lucasnetto/Desktop/almara-landing/VIDEO SITE.mp4"
```

Esperado: nenhuma saída de erro.

- [ ] **Step 2: Verificar que o vídeo foi copiado**

```bash
ls -lh "/Users/lucasnetto/Desktop/almara-landing/"
```

Esperado: `VIDEO SITE.mp4` listado com tamanho > 0.

- [ ] **Step 3: Criar o arquivo HTML vazio**

```bash
touch "/Users/lucasnetto/Desktop/almara-landing/almara-landing.html"
```

---

### Task 2: Gerar base64 das fontes

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

Os comandos abaixo geram as strings base64 que serão usadas no `@font-face` da Task 3. Execute cada um, copie a saída e substitua os marcadores `BASE64_*` no HTML.

- [ ] **Step 1: Gerar base64 da SweetSansPro Light**

```bash
base64 -i "/Users/lucasnetto/Downloads/site almara/@FONTES 2/SWEET SANS PRO/SweetSansProLight.otf" | tr -d '\n'
```

Guarde o resultado como `$FONT_LIGHT` (string longa sem quebras de linha).

- [ ] **Step 2: Gerar base64 da SweetSansPro Medium**

```bash
base64 -i "/Users/lucasnetto/Downloads/site almara/@FONTES 2/SWEET SANS PRO/SweetSansProMedium.otf" | tr -d '\n'
```

Guarde como `$FONT_MEDIUM`.

- [ ] **Step 3: Gerar base64 da GoldenHopes**

```bash
base64 -i "/Users/lucasnetto/Downloads/site almara/@FONTES 2/GOLDEN HOPES/GoldenHopes.ttf" | tr -d '\n'
```

Guarde como `$FONT_GOLDEN`.

---

### Task 3: Esqueleto HTML com CSS variables, @font-face e reset

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Escrever o esqueleto base com fontes embutidas**

Substitua o conteúdo do arquivo pelo HTML abaixo, trocando `BASE64_LIGHT`, `BASE64_MEDIUM` e `BASE64_GOLDEN` pelas strings geradas na Task 2:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Almara São Manoel</title>
  <style>
    @font-face {
      font-family: 'SweetSansPro';
      src: url('data:font/otf;base64,BASE64_LIGHT') format('opentype');
      font-weight: 300;
      font-style: normal;
    }
    @font-face {
      font-family: 'SweetSansPro';
      src: url('data:font/otf;base64,BASE64_MEDIUM') format('opentype');
      font-weight: 500;
      font-style: normal;
    }
    @font-face {
      font-family: 'GoldenHopes';
      src: url('data:font/ttf;base64,BASE64_GOLDEN') format('truetype');
      font-weight: 400;
      font-style: normal;
    }

    :root {
      --brown-dark: #382F2D;
      --taupe: #857B79;
      --cream: #F0E6DD;
      --teal: #3F676B;
    }

    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      background-color: var(--brown-dark);
      color: var(--cream);
      font-family: 'SweetSansPro', sans-serif;
      font-weight: 300;
    }
  </style>
</head>
<body>
</body>
</html>
```

- [ ] **Step 2: Abrir no navegador e verificar que a página carrega sem erros**

```bash
open "/Users/lucasnetto/Desktop/almara-landing/almara-landing.html"
```

Esperado: página em branco com fundo `#382F2D` (marrom escuro), sem erros no console.

- [ ] **Step 3: Commit**

```bash
cd "/Users/lucasnetto/Desktop/almara-landing" && git init && git add almara-landing.html && git commit -m "feat: HTML skeleton com fontes embutidas e CSS variables"
```

---

### Task 4: Seção Hero — container scroll e vídeo sticky

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar CSS da seção hero dentro do `<style>` existente, antes do `</style>`**

```css
/* ─── HERO ─── */
.hero-scroll-container {
  height: 400vh;
  position: relative;
}

.hero-sticky {
  position: sticky;
  top: 0;
  height: 100vh;
  width: 100%;
  overflow: hidden;
}

.hero-video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  padding-bottom: 12vh;
  gap: 1.5rem;
}
```

- [ ] **Step 2: Adicionar HTML da seção hero dentro do `<body>`**

```html
<!-- HERO -->
<section class="hero-scroll-container" id="hero">
  <div class="hero-sticky">
    <video class="hero-video" muted playsinline preload="auto" id="heroVideo">
      <source src="./VIDEO SITE.mp4" type="video/mp4">
    </video>
    <div class="hero-overlay" id="heroOverlay">
      <!-- logo e tagline serão adicionados na Task 5 -->
    </div>
  </div>
</section>
```

- [ ] **Step 3: Reabrir no navegador e verificar o vídeo**

Abrir `almara-landing.html`. O primeiro frame do vídeo deve preencher a tela inteira. Rolar a página deve continuar mostrando o vídeo parado (ainda sem JS). Não deve haver barras pretas.

---

### Task 5: JavaScript scroll-driven + indicador de scroll

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar CSS do indicador de scroll antes do `</style>`**

```css
.scroll-indicator {
  position: absolute;
  bottom: 3vh;
  left: 50%;
  transform: translateX(-50%);
  opacity: 1;
  transition: opacity 0.6s ease;
}

.scroll-indicator.hidden {
  opacity: 0;
  pointer-events: none;
}

.scroll-indicator-line {
  width: 1px;
  height: 44px;
  background: var(--cream);
  margin: 0 auto;
  animation: scrollPulse 1.8s ease-in-out infinite;
}

@keyframes scrollPulse {
  0%, 100% { opacity: 0.25; transform: scaleY(0.85); }
  50%       { opacity: 1;    transform: scaleY(1.15); }
}
```

- [ ] **Step 2: Adicionar indicador de scroll dentro de `.hero-sticky`, após `.hero-overlay`**

```html
<div class="scroll-indicator" id="scrollIndicator">
  <div class="scroll-indicator-line"></div>
</div>
```

- [ ] **Step 3: Adicionar o bloco `<script>` antes de `</body>`**

```html
<script>
  const video    = document.getElementById('heroVideo');
  const hero     = document.getElementById('hero');
  const indicator = document.getElementById('scrollIndicator');
  let ticking = false;

  function updateVideo() {
    const maxScroll = hero.offsetHeight - window.innerHeight;
    const progress  = Math.min(Math.max(window.scrollY / maxScroll, 0), 1);

    if (video.readyState >= 2 && video.duration) {
      video.currentTime = progress * video.duration;
    }

    if (window.scrollY > 80) {
      indicator.classList.add('hidden');
    } else {
      indicator.classList.remove('hidden');
    }
  }

  window.addEventListener('scroll', () => {
    if (!ticking) {
      requestAnimationFrame(() => { updateVideo(); ticking = false; });
      ticking = true;
    }
  });

  video.load();
</script>
```

- [ ] **Step 4: Verificar o scroll-driven no navegador**

Reabrir `almara-landing.html`. Ao rolar devagar, o vídeo deve avançar frame a frame. Ao rolar até 400vh, o vídeo deve estar no último frame. O indicador de scroll some após os primeiros 80px de scroll.

- [ ] **Step 5: Commit**

```bash
git add almara-landing.html && git commit -m "feat: hero com scroll-driven video e indicador de scroll"
```

---

### Task 6: Logo SVG inline e tagline tipográfica no hero

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar CSS do logo e tagline antes do `</style>`**

```css
.hero-logo {
  width: clamp(220px, 35vw, 520px);
}

.hero-logo svg {
  width: 100%;
  height: auto;
  display: block;
}

.hero-tagline {
  text-align: center;
  line-height: 1.6;
}

.tagline-line {
  display: block;
}

.tagline-sans {
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  letter-spacing: 0.35em;
  font-size: clamp(0.65rem, 1.3vw, 1rem);
  text-transform: uppercase;
  color: var(--cream);
}

.tagline-script {
  font-family: 'GoldenHopes', cursive;
  font-size: clamp(1.3rem, 2.6vw, 1.9rem);
  color: var(--cream);
  letter-spacing: 0.02em;
  vertical-align: middle;
}
```

- [ ] **Step 2: Substituir o comentário `<!-- logo e tagline -->` dentro de `.hero-overlay` pelo SVG inline e tagline**

O SVG abaixo é o logo-03 (creme `#f0e6dd`) com o `<rect>` de fundo removido para que o vídeo apareça por trás:

```html
<div class="hero-logo">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1920 1080" aria-label="Almara São Manoel">
    <style>.lc1{fill:#f0e6dd}</style>
    <g>
      <g>
        <path class="lc1" d="M374.52,456.04c-1.42,9.24-7.58,17.3-20.61,17.3h-26.54c-12.8,0-22.51,11.85-25.35,22.98-3.56,23.93,1.66,33.41,1.66,33.41h-22.27l59.71-165.63h26.07l60.42,165.63h-26.06l-27.01-73.69ZM353.91,468.6c15.16,0,19.9-14.69,14.69-29.14l-24.88-68.48-31.99,88.62c-2.61,7.35-4.5,13.74-6.16,19.9,5.45-6.4,13.03-10.9,21.8-10.9h26.54Z"/>
        <path class="lc1" d="M522.37,529.73v-165.86h24.41v161.12h27.48c54.74,0,77.25-16.35,77.25-16.35l-11.85,21.09h-117.29Z"/>
        <path class="lc1" d="M751.02,462.2c.47,47.63,17.06,67.53,17.06,67.53h-21.8v-165.86h26.78l61.13,137.67,60.66-137.67h24.4v165.86h-24.4v-154.01l-62.08,140.98-5.92,13.03h-6.87l-68.95-155.44v87.91Z"/>
        <path class="lc1" d="M1107.14,456.04c-1.42,9.24-7.58,17.3-20.61,17.3h-26.54c-12.8,0-22.51,11.85-25.35,22.98-3.56,23.93,1.66,33.41,1.66,33.41h-22.27l59.71-165.63h26.07l60.42,165.63h-26.06l-27.01-73.69ZM1086.52,468.6c15.16,0,19.9-14.69,14.69-29.14l-24.88-68.48-31.99,88.62c-2.61,7.35-4.5,13.74-6.16,19.9,5.45-6.4,13.03-10.9,21.8-10.9h26.54Z"/>
        <path class="lc1" d="M1254.99,363.87h73.22c51.89.24,60.66,24.41,60.66,40.28s-8.77,40.04-60.66,40.04c-42.41,0-36.96,39.33,12.56,39.33,61.84,0,61.61,36.25,51.89,46.2,0-6.63-3.32-21.56-50.23-21.56-63.03,0-68.72-68.71-14.45-68.71,29.85,0,34.12-13.74,34.12-35.31s-14.22-35.31-49.76-35.31h-32.93v160.89h-24.41v-165.86Z"/>
        <path class="lc1" d="M1585.52,456.04c-1.42,9.24-7.58,17.3-20.61,17.3h-26.54c-12.8,0-22.51,11.85-25.35,22.98-3.56,23.93,1.66,33.41,1.66,33.41h-22.27l59.71-165.63h26.07l60.42,165.63h-26.06l-27.01-73.69ZM1564.91,468.6c15.16,0,19.9-14.69,14.69-29.14l-24.88-68.48-31.99,88.62c-2.61,7.35-4.5,13.74-6.16,19.9,5.45-6.4,13.03-10.9,21.8-10.9h26.54Z"/>
      </g>
      <g>
        <path class="lc1" d="M680.65,690.26c-1.9-1.7-5.93-3.94-11.77-3.94-4.09,0-8.27,1.46-8.27,5.45s5.25,4.28,10.41,4.57c5.54.39,14.49.92,14.49,9.44,0,7.39-6.57,10.36-14.2,10.36-8.07,0-13.23-3.36-16.68-6.52l2.58-2.92c2.68,2.48,7.1,5.84,14.2,5.84,5.4,0,9.87-1.95,9.87-6.23,0-4.77-4.86-5.5-10.26-5.89-6.86-.44-14.64-.92-14.64-8.27s6.96-9.44,12.65-9.44c6.56,0,11.72,2.63,14.15,4.62l-2.53,2.92Z"/>
        <path class="lc1" d="M718.98,715.3l14.74-31.76h3.99l14.74,31.76h-4.62l-4.33-9.53h-15.86l-4.28,9.53h-4.38ZM727.49,678.15c1.6-2.67,3.06-3.84,4.96-3.84,1.27,0,2.19.54,3.36,1.36.92.68,1.8,1.31,2.77,1.31,1.17,0,1.99-1.12,2.77-2.24l2.38,1.31c-1.46,2.48-3.01,3.89-5.01,3.89-1.31,0-2.53-.58-3.4-1.26-1.07-.78-1.8-1.41-2.68-1.41-1.12,0-1.9,1.22-2.63,2.24l-2.53-1.36ZM729.14,702.37h12.84l-6.27-13.86h-.29l-6.27,13.86Z"/>
        <path class="lc1" d="M803.41,682.72c9.82,0,17.7,7.44,17.7,16.63s-7.88,16.78-17.7,16.78-17.61-7.44-17.61-16.78,7.73-16.63,17.61-16.63ZM803.41,712.39c7.44,0,13.28-5.84,13.28-13.03s-5.84-12.89-13.28-12.89-13.18,5.79-13.18,12.89,5.74,13.03,13.18,13.03Z"/>
        <path class="lc1" d="M927.68,715.3v-24.07h-.24l-11.48,19.31h-.87l-11.48-19.31h-.24v24.07h-4.04v-31.76h4.48l11.87,19.99h.1l11.87-19.99h4.28v31.76h-4.23Z"/>
        <path class="lc1" d="M968.58,715.3l14.74-31.76h3.99l14.74,31.76h-4.62l-4.33-9.53h-15.86l-4.28,9.53h-4.38ZM978.75,702.37h12.84l-6.27-13.86h-.29l-6.27,13.86Z"/>
        <path class="lc1" d="M1066.97,683.55v31.76h-3.84l-20.18-24.37h-.05v24.37h-4.23v-31.76h3.84l20.18,24.37h.05v-24.37h4.23Z"/>
        <path class="lc1" d="M1122.71,682.72c9.82,0,17.7,7.44,17.7,16.63s-7.88,16.78-17.7,16.78-17.61-7.44-17.61-16.78,7.73-16.63,17.61-16.63ZM1122.71,712.39c7.44,0,13.28-5.84,13.28-13.03s-5.84-12.89-13.28-12.89-13.18,5.79-13.18,12.89,5.74,13.03,13.18,13.03Z"/>
        <path class="lc1" d="M1195.57,700.47h-12.84v11.09h22.42v3.75h-26.65v-31.76h25.53v3.75h-21.3v9.44h12.84v3.75Z"/>
        <path class="lc1" d="M1246.79,683.55v28.01h20.04v3.75h-24.27v-31.76h4.23Z"/>
      </g>
    </g>
  </svg>
</div>
<div class="hero-tagline">
  <span class="tagline-line"><span class="tagline-sans">À altura da</span></span>
  <span class="tagline-line"><span class="tagline-sans">Sua </span><span class="tagline-script">história</span></span>
</div>
```

- [ ] **Step 3: Verificar no navegador**

Reabrir. O logo ALMARA SÃO MANOEL deve aparecer em creme sobre o vídeo. Abaixo, "À ALTURA DA / SUA" em sans-serif espaçado e "história" em script cursivo, ambos em creme.

- [ ] **Step 4: Commit**

```bash
git add almara-landing.html && git commit -m "feat: logo SVG inline e tagline tipográfica no hero"
```

---

### Task 7: Seção Localização

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar CSS da seção localização antes do `</style>`**

```css
/* ─── LOCALIZAÇÃO ─── */
.location-section {
  background: var(--brown-dark);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 80px 40px;
  gap: 40px;
}

.section-title {
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  font-size: clamp(0.6rem, 1.1vw, 0.85rem);
  letter-spacing: 0.45em;
  text-transform: uppercase;
  color: var(--cream);
}

.location-map {
  width: 100%;
  max-width: 900px;
  height: 420px;
  border: 1px solid rgba(240,230,221,0.15);
  border-radius: 4px;
  overflow: hidden;
}

.location-map iframe {
  width: 100%;
  height: 100%;
  border: none;
  filter: grayscale(30%) sepia(20%);
}

.location-address {
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  font-size: clamp(0.6rem, 1vw, 0.8rem);
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(240,230,221,0.6);
  text-align: center;
}
```

- [ ] **Step 2: Adicionar HTML da seção localização depois da seção hero, antes de `</body>`**

```html
<!-- LOCALIZAÇÃO -->
<section class="location-section">
  <h2 class="section-title">Localização</h2>
  <div class="location-map">
    <iframe
      src="https://maps.google.com/maps?q=Condom%C3%ADnio+Ecoville+S%C3%A3o+Manoel,+Sorriso,+MT&output=embed"
      allowfullscreen
      loading="lazy"
      referrerpolicy="no-referrer-when-downgrade">
    </iframe>
  </div>
  <p class="location-address">Condomínio Ecoville São Manoel &nbsp;·&nbsp; Sorriso, Mato Grosso</p>
</section>
```

- [ ] **Step 3: Verificar no navegador**

Rolar além do hero. A seção de localização deve aparecer com fundo marrom escuro, título espaçado e o mapa do Google. O endereço deve aparecer abaixo do mapa em creme com opacidade reduzida.

- [ ] **Step 4: Commit**

```bash
git add almara-landing.html && git commit -m "feat: seção de localização com mapa embed"
```

---

### Task 8: Seção Formulário de Interesse

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar CSS do formulário antes do `</style>`**

```css
/* ─── FORMULÁRIO ─── */
.form-section {
  background: var(--cream);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 80px 40px;
  gap: 52px;
}

.form-title {
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  font-size: clamp(0.6rem, 1.1vw, 0.85rem);
  letter-spacing: 0.45em;
  text-transform: uppercase;
  color: var(--brown-dark);
  text-align: center;
}

.contact-form {
  width: 100%;
  max-width: 860px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 52px;
}

.form-fields {
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 36px;
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.form-field label {
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  font-size: 0.6rem;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  color: var(--brown-dark);
}

.form-field input {
  background: transparent;
  border: none;
  border-bottom: 1px solid rgba(56,47,45,0.4);
  padding: 8px 0 10px;
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  font-size: 0.9rem;
  color: var(--brown-dark);
  outline: none;
  width: 100%;
  transition: border-color 0.3s;
}

.form-field input:focus {
  border-bottom-color: var(--brown-dark);
}

.form-field input::placeholder {
  color: rgba(56,47,45,0.35);
}

.form-submit {
  background: var(--brown-dark);
  color: var(--cream);
  border: none;
  padding: 18px 56px;
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 500;
  font-size: 0.65rem;
  letter-spacing: 0.38em;
  text-transform: uppercase;
  cursor: pointer;
  transition: background 0.3s ease;
}

.form-submit:hover {
  background: var(--teal);
}
```

- [ ] **Step 2: Adicionar HTML do formulário depois da seção de localização, antes de `</body>`**

```html
<!-- FORMULÁRIO -->
<section class="form-section">
  <h2 class="form-title">Tenha acesso exclusivo</h2>
  <form class="contact-form" id="contactForm">
    <div class="form-fields">
      <div class="form-field">
        <label for="name">Nome</label>
        <input type="text" id="name" name="name" placeholder="Seu nome completo" required>
      </div>
      <div class="form-field">
        <label for="email">E-mail</label>
        <input type="email" id="email" name="email" placeholder="seu@email.com" required>
      </div>
      <div class="form-field">
        <label for="phone">Telefone</label>
        <input type="tel" id="phone" name="phone" placeholder="(00) 00000-0000" required>
      </div>
    </div>
    <button type="submit" class="form-submit">Quero saber mais</button>
  </form>
</section>
```

- [ ] **Step 3: Adicionar handler do formulário no bloco `<script>` existente, antes do último `</script>`**

```javascript
document.getElementById('contactForm').addEventListener('submit', function(e) {
  e.preventDefault();
  const name = document.getElementById('name').value.trim();
  alert('Obrigado, ' + name + '! Em breve nossa equipe entrará em contato.');
  this.reset();
});
```

- [ ] **Step 4: Verificar no navegador**

Rolar até a seção do formulário. Fundo deve ser creme, campos com border-bottom marrom, botão escuro. Hover no botão deve mudar para teal. Submeter o formulário deve mostrar o alert e limpar os campos.

- [ ] **Step 5: Commit**

```bash
git add almara-landing.html && git commit -m "feat: seção de formulário de interesse com CTA"
```

---

### Task 9: Rodapé

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar CSS do rodapé antes do `</style>`**

```css
/* ─── RODAPÉ ─── */
footer {
  background: var(--brown-dark);
  padding: 48px 40px;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: center;
}

.footer-brand {
  font-family: 'SweetSansPro', sans-serif;
  font-weight: 300;
  font-size: 0.6rem;
  letter-spacing: 0.45em;
  text-transform: uppercase;
  color: rgba(240,230,221,0.45);
}

.footer-divider {
  width: 1px;
  height: 24px;
  background: rgba(240,230,221,0.2);
  margin: 4px auto;
}
```

- [ ] **Step 2: Adicionar HTML do rodapé depois do formulário, antes de `</body>`**

```html
<!-- RODAPÉ -->
<footer>
  <p class="footer-brand">Epicentro Empreendimentos</p>
  <div class="footer-divider"></div>
  <p class="footer-brand">© 2026 Almara São Manoel</p>
</footer>
```

- [ ] **Step 3: Verificar no navegador**

O rodapé deve aparecer com fundo marrom escuro, texto em creme com baixa opacidade, espaçamento generoso.

- [ ] **Step 4: Commit**

```bash
git add almara-landing.html && git commit -m "feat: rodapé com crédito Epicentro Empreendimentos"
```

---

### Task 10: Responsividade mobile

**Files:**
- Modify: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Adicionar media queries antes do `</style>`**

```css
/* ─── MOBILE ─── */
@media (max-width: 768px) {
  .hero-logo {
    width: clamp(160px, 70vw, 300px);
  }

  .tagline-sans {
    font-size: 0.6rem;
    letter-spacing: 0.25em;
  }

  .tagline-script {
    font-size: 1.15rem;
  }

  .hero-overlay {
    padding-bottom: 10vh;
    gap: 1rem;
  }

  .location-section,
  .form-section {
    padding: 60px 24px;
  }

  .location-map {
    height: 260px;
  }

  .form-fields {
    grid-template-columns: 1fr;
    gap: 28px;
  }

  .form-submit {
    width: 100%;
    padding: 18px 24px;
  }

  footer {
    padding: 40px 24px;
  }
}
```

- [ ] **Step 2: Verificar no navegador com DevTools**

Abrir DevTools (F12) → Toggle device toolbar (Ctrl+Shift+M) → Selecionar iPhone 14 Pro (390px). Verificar:
- Logo redimensiona corretamente
- Tagline legível
- Formulário em 1 coluna
- Mapa com altura adequada
- Nenhum scroll horizontal

- [ ] **Step 3: Commit**

```bash
git add almara-landing.html && git commit -m "feat: responsividade mobile (breakpoint 768px)"
```

---

### Task 11: Verificação final completa

**Files:**
- Read: `/Users/lucasnetto/Desktop/almara-landing/almara-landing.html`

- [ ] **Step 1: Abrir a página no navegador e percorrer o fluxo completo**

```bash
open "/Users/lucasnetto/Desktop/almara-landing/almara-landing.html"
```

Checklist de verificação:
- [ ] Primeiro frame do vídeo aparece imediatamente ao carregar
- [ ] Scroll lento avança o vídeo frame a frame
- [ ] Scroll até 400vh exibe o último frame do vídeo
- [ ] Indicador de scroll some após 80px de scroll
- [ ] Seção de localização renderiza com mapa do Google Maps
- [ ] Formulário possui 3 campos em desktop
- [ ] Hover no botão muda para teal `#3F676B`
- [ ] Submit do formulário mostra alert com o nome e limpa os campos
- [ ] Rodapé aparece com créditos

- [ ] **Step 2: Testar em mobile via DevTools**

Abrir DevTools → iPhone 14 Pro → verificar:
- [ ] Formulário em 1 coluna
- [ ] Sem overflow horizontal
- [ ] Scroll-driven funciona com touch scroll

- [ ] **Step 3: Commit final**

```bash
git add almara-landing.html && git commit -m "feat: landing page Almara São Manoel completa"
```
