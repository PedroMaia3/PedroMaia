# Site de CV para recrutadores — Plano de implementação

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Atualizar o site de CV estático para recrutadores: CVs em PDF sem dados pessoais, textos alinhados com o CV 2026, meta tags de partilha e código limpo.

**Architecture:** Site estático (Bootstrap "Resume" + `css/custom.css`), sem build nem testes automáticos. Mudanças só em `index.html`, `css/custom.css` e uma nova pasta `cv/`. Verificação por `grep`, extração de texto dos PDFs e inspeção visual no navegador.

**Tech Stack:** HTML, CSS, Bootstrap 5.2.3, Font Awesome 6.3.0; Word (COM) para exportar PDF.

**Spec:** `docs/superpowers/specs/2026-11-20-cv-site-recrutadores-design.md`

## Global Constraints

- Site só em português. Mantém-se o template Resume + `css/custom.css` (sem redesenho nem novo framework).
- Fonte de verdade do conteúdo: `C:\Users\FA506\Documents\Pessoal\CV Pedro Maia 2026.docx` (PT) e `CV Pedro Maia EN 2026.docx`. Não inventar factos.
- Nenhum telemóvel nem morada no repositório. Cidade "Porto" pode ficar.
- Os `.docx` originais e a pasta de recibos nunca entram no repositório.
- Worten Resolve: **Mai 2018 – Jul 2018**.
- URL público: `https://pedromaia3.github.io/PedroMaia/`.
- Commits só quando o Pedro pedir; os textos reescritos são mostrados antes do commit.

## Review Focus

- O número de telemóvel nos PDFs, incluindo metadados, rodapés e texto oculto: não pode aparecer.
- Links `cv/...pdf` partidos (maiúsculas/minúsculas, espaços): o GitHub Pages distingue maiúsculas.
- Pré-visualização ao partilhar: `og:image` tem de ser URL absoluto e a imagem acessível.
- Ecrã de telemóvel: os botões de CV não podem transbordar nem quebrar o layout.
- Datas e cargos nos textos novos têm de coincidir com o CV (sem factos inventados).

---

### Task 1: Limpeza do código e dos ficheiros não usados

**Files:**
- Delete: `styles.css` (raiz), `imagens/e-goi.png`, `imagens/ispgaya.png`, `assets/img/profile.jpg`
- Modify: `index.html`

- [ ] **Step 1: Confirmar que nada referencia os ficheiros a apagar**

Run: `grep -rn "e-goi.png\|ispgaya.png\|profile.jpg" index.html css/custom.css js/scripts.js` e confirmar que `index.html` só usa `css/styles.css`.
Expected: sem resultados; `styles.css` da raiz não aparece em nenhum `href`.

- [ ] **Step 2: Apagar os quatro ficheiros** com `git rm`.

- [ ] **Step 3: Em `index.html`, remover** todos os blocos `<!-- ... -->` com HTML antigo na secção Habilidades, o `<ul class="list-inline dev-icons">` vazio, o `<div class="subheading mb-3"></div>` vazios e o `<hr class="m-0" />` duplicado antes de Habilidades. Manter comentários de uma linha que rotulam secções.

- [ ] **Step 4: Verificar**

Run: `grep -c "<ul>" index.html; grep -n "dev-icons\|<!--  *<ul\|<!-- *<ul" index.html`
Expected: sem blocos HTML comentados nem `dev-icons`; `grep -c 'hr class="m-0"' index.html` devolve 3 (entre as quatro secções).

- [ ] **Step 5: Commit** (quando o Pedro pedir): `chore: remove ficheiros e código não usados`

---

### Task 2: Meta tags e título

**Files:**
- Modify: `index.html` (`<head>`)

**Interfaces:**
- Produces: `og:image` = `https://pedromaia3.github.io/PedroMaia/imagens/profile.jpeg` (usado só aqui).

- [ ] **Step 1: Definir `<title>`** como `Pedro Maia — Programador Full-Stack` e preencher `<meta name="description">` (≤160 caracteres, PT: programador full-stack em Vue.js e ASP.NET, Porto) e `<meta name="author" content="Pedro Maia">`.

- [ ] **Step 2: Adicionar** `og:type=website`, `og:locale=pt_PT`, `og:title`, `og:description`, `og:url`, `og:image`, e `twitter:card=summary`, com os mesmos textos.

- [ ] **Step 3: Verificar**

Run: `grep -n "og:\|name=\"description\"\|<title>" index.html`
Expected: todas as etiquetas presentes e nenhuma com `content=""`.

- [ ] **Step 4: Commit** (quando pedido): `feat: meta description e etiquetas de partilha`

---

### Task 3: CVs em PDF sem dados pessoais

**Files:**
- Create: `cv/Pedro-Maia-CV-PT.pdf`, `cv/Pedro-Maia-CV-EN.pdf`
- Scratch (fora do repo): cópias editadas dos `.docx` no diretório scratchpad

- [ ] **Step 1: Copiar os dois `.docx`** para o scratchpad (nunca editar os originais).

- [ ] **Step 2: Em cada cópia**, remover o número de telemóvel e o separador `|` seguinte da linha de contacto, mantendo e-mail, Porto, LinkedIn e website. Também limpar propriedades do documento (autor/empresa) se contiverem dados além do nome. Usar PowerShell com Word COM (`Word.Application`, `Find.Execute`/`Replace`, `ExportAsFixedFormat` para PDF).

- [ ] **Step 3: Exportar** para `cv/Pedro-Maia-CV-PT.pdf` e `cv/Pedro-Maia-CV-EN.pdf` (nomes sem espaços, com maiúsculas exatas).

- [ ] **Step 4: Verificar que o telemóvel não existe**

Run (em cada PDF): extrair texto (ex.: `python -I -c "import pypdf..."` ou leitura do PDF) e procurar os dígitos do número de telemóvel (com e sem espaços); confirmar também que a mesma pesquisa no binário devolve 0.
Expected: 0 ocorrências; o e-mail e o LinkedIn aparecem; o PDF abre e mantém o layout (ler visualmente a 1.ª página).

- [ ] **Step 5: Commit** (quando pedido): `feat: adiciona CV em PDF (PT e EN) sem contactos privados`

---

### Task 4: Reescrita dos textos e layout dos botões

**Files:**
- Modify: `index.html` (secções Sobre, Experiência, Formação, Habilidades), `css/custom.css` (botões)

**Interfaces:**
- Consumes: `cv/Pedro-Maia-CV-PT.pdf`, `cv/Pedro-Maia-CV-EN.pdf` (Task 3).
- Produces: classe `.cv-actions` (contentor flex com wrap) e `.btn-cv` em `css/custom.css`.

- [ ] **Step 1: Cabeçalho/Sobre.** Por baixo de "Pedro Maia": subtítulo "Programador Full-Stack · Porto". Adicionar `<div class="cv-actions">` com: "Descarregar CV (PT)" → `cv/Pedro-Maia-CV-PT.pdf` e "Download CV (EN)" → `cv/Pedro-Maia-CV-EN.pdf` (ambos `download`), mais e-mail e LinkedIn. Reescrever os dois parágrafos de apresentação (curtos, stack atual primeiro, uma frase por ideia, sem factos novos).

- [ ] **Step 2: CSS.** `.cv-actions { display:flex; flex-wrap:wrap; gap:.75rem }` e `.btn-cv` com a cor de destaque `#bd5d38` já usada no `custom.css`; em ecrãs <576px, botões em largura total.

- [ ] **Step 3: Experiência.** Alinhar cada entrada com o CV 2026. Tech4home e Multiverso mantêm-se. Finerge: intranet com scraping de notícias e partilha de documentos (HTML, CSS, PHP, SQL), suporte via ServiceDesk Plus, redes Cisco. E-goi: adicionar "consumo e manipulação de dados de APIs externas". Estágios antigos (Nova Informática 2019, C.A.O.S, Worten) em formato compacto de uma linha cada, Worten com **Maio de 2018 – Julho de 2018**. Acrescentar o projeto Braço Direito (SONAE/BIT, 2018) em uma linha. Manter o link do relatório de estágio.

- [ ] **Step 4: Habilidades.** Substituir pelas quatro categorias do CV: Programação (HTML, CSS, JavaScript, jQuery, Vue.js, PHP, ASP.NET, Python, SQL/MySQL), Redes e Sistemas, Suporte e Ferramentas, Metodologias e IA (Agile/Scrum, Git, LLMs e Agentic Workflows). Mostrar como chips/listas curtas, sem o ícone Angular. Condensar "Competências Interpessoais" numa linha.

- [ ] **Step 5: Novas secções curtas** "Idiomas" (Português nativo; Inglês: compreensão C1, leitura C1, escrita B2, oral B1/B2) e "Informação adicional" (Carta de condução A e B). Acrescentar o link "Idiomas" à navegação só se ficar uma secção própria.

- [ ] **Step 6: Mostrar o resultado ao Pedro.** Abrir `index.html` no navegador do Claude em desktop e em 375px de largura; capturar e apresentar os textos finais para revisão antes do commit.

- [ ] **Step 7: Verificar links e consistência**

Run: `grep -o 'href="cv/[^"]*"' index.html; ls cv; grep -n "Angular" index.html` (e procurar também os dígitos do telemóvel)
Expected: dois links que correspondem exatamente aos ficheiros em `cv/`; sem telemóvel nem Angular. Datas e cargos conferidos com o CV, linha a linha.

- [ ] **Step 8: Commit** (quando pedido): `feat: textos orientados a recrutadores e botões de CV`

---

## Auto-revisão

- **Cobertura da spec:** PDFs (Task 3), textos (Task 4), meta (Task 2), limpeza (Task 1), Worten 2018 (Task 4), cidade mantida (Task 3). Fora de âmbito respeitado.
- **Consistência:** nomes dos PDFs e `.cv-actions` coincidem entre tarefas.
- **Nota:** projeto sem testes automatizados; a verificação é por `grep`, extração de texto e inspeção visual.
