# Site de CV orientado a recrutadores — design (versão curta)

## Intenção
O site `pedromaia3.github.io/PedroMaia` é enviado a recrutadores em candidaturas. Tem de, em poucos segundos, dizer quem é o Pedro, o que faz hoje e como o contactar ou levar o CV.

## Objetivos
- Conteúdo alinhado com o CV 2026 (a fonte de verdade): stack atual (Vue.js, C#/ASP.NET) visível, textos mais diretos e orientados a resultados.
- Botões "Descarregar CV" (PT e EN) em destaque, mais e-mail e LinkedIn.
- Meta description e etiquetas de partilha (Open Graph) para o link ficar apresentável no LinkedIn, WhatsApp e Google.
- Limpar o código: blocos comentados, `<ul>` vazio, `<hr>` duplicado, ficheiros não usados.

## Fora de âmbito
- Redesenho visual ou troca de framework (mantém-se o template Resume + `custom.css`).
- Versão do site em inglês (só PT; o EN existe apenas como PDF).
- Analytics, formulário de contacto, link para GitHub (não pedido).

## O que seria um fracasso
- Dados pessoais (telemóvel, morada) no repositório público ou no histórico do git.
- Inventar factos que não estão no CV.

## Abordagens consideradas
1. **PDF público sem dados sensíveis (escolhida pelo Pedro).** Mantém só nome, e-mail, LinkedIn, site e cidade. Simples e sem risco de privacidade.
2. PDF completo alojado fora do repositório (Drive/OneDrive). Rejeitada: dá acesso ao telemóvel a quem tiver o link.
3. PDF completo no repositório com palavra-passe. Rejeitada: o ficheiro continua público e a proteção é fraca.

## Decisões
**CV em PDF**
- Gerados a partir dos dois `.docx` de `C:\Users\FA506\Documents\Pessoal`, com o telemóvel removido e a cidade mantida (não é uma morada). Ficam em `cv/` do site.
- Os `.docx` originais nunca entram no repositório. A pasta de recibos de vencimento não é tocada.

**Textos (PT)**
- Cabeçalho: nome, "Programador Full-Stack", localização (Porto) e botões (CV PT, CV EN, LinkedIn, e-mail).
- Secção Sobre: reescrita curta com a stack atual e o tipo de contextos onde trabalhou.
- Experiência: cargos recentes com mais detalhe, alinhado com o CV (ex.: Finerge com a intranet, o scraping de notícias e as redes Cisco). Experiências antigas ficam em formato compacto. Tudo continua listado.
- Formação: sem alterações de factos.
- Habilidades: reorganizadas nos mesmos grupos do CV (Programação, Redes e Sistemas, Suporte e Ferramentas, Metodologias e IA), com Vue.js, ASP.NET, Git, Scrum e LLMs. Angular e Python só ficam se constarem do CV (Python consta). Sem ícones de Angular.
- Novas secções curtas: Idiomas e Carta de condução (A e B), como no CV. O projeto Braço Direito (SONAE/BIT, 2018) entra numa linha.
- A lista genérica de "Competências Interpessoais" é condensada numa linha.

**Técnico**
- `<title>`: "Pedro Maia — Programador Full-Stack". Meta description, `og:title`, `og:description`, `og:image` (foto de perfil) e `og:url` com endereço absoluto.
- Remover: `styles.css` da raiz, `imagens/e-goi.png`, `imagens/ispgaya.png`, `assets/img/profile.jpg` (nenhum está referenciado no `index.html`), blocos HTML comentados, `<ul class="dev-icons">` vazio e o `<hr>` duplicado.

## As minhas escolhas (para o Pedro confirmar)
- **Worten Resolve:** uso **Mai 2018 – Jul 2018**, como está no CV 2026. A mensagem falava de verão de 2016, mas o curso só começou em setembro de 2016, por isso 2016 não bate certo.
- Mantenho a cidade (Porto) nos PDFs e no site.
- Os ficheiros soltos na raiz (apresentação e relatório de estágio) ficam como estão. O relatório está ligado na E-goi; a apresentação `.odp`/`.pptx` não está ligada a nada, e fica a tua decisão se sai do repositório público.

## Fica para quem constrói
- Método de conversão para PDF (Word está instalado; exportação via Word) e verificação de que o PDF não contém o telemóvel.
- Redação final dos textos, mostrada ao Pedro antes de qualquer commit.
- Verificação visual em desktop e telemóvel antes de entregar.
