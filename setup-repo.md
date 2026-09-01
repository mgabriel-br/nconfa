# setup-repo.md — Criação do repositório público `nconfa`

> Instruções para o Claude Code. Execute **hoje**, antes de qualquer código do
> pacote. O objetivo desta sessão é iniciar o registro público de desenvolvimento
> aberto exigido pelo JOSS (mínimo de seis meses de histórico público antes da
> submissão). Nada de implementação de funções aqui — isso é o `plano.md`.

---

## 0. Antes de começar

### 0.1 Preencha estes valores comigo

Não invente nenhum deles. Se algum estiver em branco, **pare e pergunte** antes
de criar arquivos.

```
NOME_COMPLETO      = 
EMAIL              = 
USUARIO_GITHUB     = 
ORCID              = 
AFILIACAO          = 
ANO                = 2026
NOME_DO_PACOTE     = nconfa
```

Ao longo deste documento, substitua os marcadores `{{NOME_COMPLETO}}`,
`{{EMAIL}}`, `{{USUARIO_GITHUB}}`, `{{ORCID}}`, `{{AFILIACAO}}`, `{{ANO}}` e
`{{PKG}}` pelos valores acima.

### 0.2 Verificações de ambiente

Rode e me mostre a saída antes de prosseguir:

```bash
gh auth status
git --version
R --version 2>/dev/null || echo "R não encontrado no PATH"
```

Se `gh auth status` falhar, pare — o resto depende dele.

### 0.3 Confirme a disponibilidade do nome

```r
# se o pacote 'available' não estiver instalado:
# install.packages("available")
available::available("nconfa")
```

Cole a saída no chat. Se houver colisão em CRAN, Bioconductor ou GitHub, **pare**
e me avise antes de criar o repositório. Alternativas em ordem de preferência:
`asymqca`, `necessa`, `configpath`.

---

## 1. Regras desta sessão

Três regras que valem para tudo abaixo:

1. **Nada de commit único.** Faça os commits na ordem da seção 4, um por vez,
   cada um com sua mensagem própria. O JOSS roda checagem automatizada sobre a
   distribuição dos commits; um despejo de repositório é sinal negativo.
2. **Nunca altere a data de commits.** Nada de `--date`, `GIT_AUTHOR_DATE` ou
   rebase para forjar histórico. Isso é fraude e é detectável.
3. **Não crie CI hoje.** O workflow `R-CMD-check` entra na Fase 0 do `plano.md`,
   quando já existir um pacote para checar. Um repositório cujo primeiro badge é
   vermelho é pior do que um sem badge.

---

## 2. Criar o diretório local e o esqueleto de arquivos

```bash
mkdir -p nconfa && cd nconfa
git init -b main
```

Crie os arquivos das subseções abaixo exatamente com o conteúdo indicado,
substituindo os marcadores. Não acrescente seções não pedidas.

---

### 2.1 `README.md`

````markdown
# nconfa

<!-- badges: start -->
**Status: em desenvolvimento inicial (pré-alfa).** A API é instável e pode mudar
sem aviso até a versão 0.1.0.
<!-- badges: end -->

Análise assimétrica integrada para pesquisa em negócios e ciências sociais:
Necessary Condition Analysis (NCA), fuzzy-set Qualitative Comparative Analysis
(fsQCA) e **Necessary Configuration Analysis (NConfA)** num único fluxo
reprodutível.

## Motivação

Métodos assimétricos respondem a perguntas que a regressão não responde: uma
condição é *necessária* para o resultado? Quais *combinações* de condições são
*suficientes*? Existem implementações maduras em R para as duas primeiras
perguntas — os pacotes [`NCA`](https://cran.r-project.org/package=NCA) (Dul &
Buijs) e [`QCA`](https://cran.r-project.org/package=QCA) (Duşa).

O que falta é a terceira etapa. A NConfA (Rasoolimanesh & Olya, 2025) aplica a
lógica de necessidade às *configurações* identificadas pela fsQCA, e não a
condições isoladas — ou seja, testa se um caminho suficiente é também
indispensável. Até onde sabemos, não existe implementação pública dela em R.
O `nconfa` fornece essa etapa e a encadeia às anteriores, com calibração,
diagnósticos e relato em linguagem acessível a quem decide.

## Escopo

Dentro do escopo:

- calibração fuzzy com diagnóstico de âncoras fora da faixa observada;
- NCA bivariada (CE-FDH, CR-FDH, CE-VRS, CR-VRS, QR), tamanhos de efeito e
  tabelas de gargalo, sobre o pacote `NCA`;
- fsQCA (tabela verdade e minimização booleana) sobre o pacote `QCA`;
- construção de configurações candidatas a partir dos termos suficientes, com
  limiar de frequência parametrizável;
- NConfA: consistência, cobertura e RoN das configurações candidatas;
- interface Shiny opcional sobre a biblioteca central;
- relato em linguagem simples dos resultados integrados.

Fora do escopo:

- reimplementar NCA ou QCA (o `nconfa` depende deles, não os substitui);
- soluções intermediárias de fsQCA (expectativas direcionais por condição);
- QCA de conjuntos nítidos ou multivalorados;
- inferência causal — os resultados são associações na amostra fornecida.

## Instalação

Ainda não disponível. Quando houver um release:

```r
# install.packages("remotes")
remotes::install_github("{{USUARIO_GITHUB}}/nconfa")
```

O `nconfa` instala sem `NCA` e sem `QCA`; ambos ficam em `Suggests` e são
exigidos apenas pelas funções que de fato os usam.

## Exemplo pretendido

A API abaixo é o alvo do desenvolvimento e ainda não funciona.

```r
library(nconfa)

spec <- list(
  CATT = calib_spec("likert5"),
  EAT  = calib_spec("likert5"),
  RP   = calib_spec("percentile")
)

res <- nconfa_pipeline(
  data       = demo_survey,
  outcome    = "RP",
  conditions = c("CATT", "EAT", "CUAT", "EGA", "INV"),
  spec       = spec,
  nec_cut    = 0.90
)

summary(res)
```

## Terminologia

Rasoolimanesh & Olya (2025) usam o mesmo par de fórmulas (suas Eq. 1 e Eq. 2)
para testar suficiência e necessidade, chamando-as de "consistência" e
"cobertura" em ambos os casos. Isso diverge da convenção mais difundida (Ragin,
2006; Schneider & Wagemann, 2012; Dul, 2016b), na qual o uso da Eq. 2 para
necessidade costuma ser chamado de *consistência de necessidade*.

O `nconfa` mantém a nomenclatura da fonte do método e documenta a
correspondência explicitamente. Uma vinheta dedicada tratará do assunto.

## Roadmap

O desenvolvimento está organizado em fases, acompanháveis nas
[issues](https://github.com/{{USUARIO_GITHUB}}/nconfa/issues) com o rótulo
`roadmap`. Sugestões e relatos são bem-vindos — veja
[CONTRIBUTING.md](CONTRIBUTING.md).

## Referências

- Dul, J. (2016a). Necessary Condition Analysis (NCA): Logic and methodology of
  "necessary but not sufficient" causality. *Organizational Research Methods*,
  19(1), 10–52.
- Dul, J. (2016b). Identifying single necessary conditions with NCA and fsQCA.
  *Journal of Business Research*, 69(4), 1516–1523.
- Ragin, C. C. (2006). Set relations in social research: Evaluating their
  consistency and coverage. *Political Analysis*, 14(3), 291–310.
- Rasoolimanesh, S. M., & Olya, H. (2025). Necessary Configuration Analysis.
  *The Service Industries Journal*, 45(15–16), 1303–1312.
- Schneider, C. Q., & Wagemann, C. (2012). *Set-Theoretic Methods for the Social
  Sciences*. Cambridge University Press.

## Licença

MIT © {{ANO}} {{NOME_COMPLETO}}
````

---

### 2.2 `LICENSE.md`

```
MIT License

Copyright (c) {{ANO}} {{NOME_COMPLETO}}

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

> Nota: na Fase 0 do `plano.md`, `usethis::use_mit_license("{{NOME_COMPLETO}}")`
> vai gerar um arquivo `LICENSE` com apenas `YEAR` e `COPYRIGHT HOLDER` (o
> formato que o CRAN exige) e reescrever este `LICENSE.md`. Isso é esperado, não
> é conflito.

---

### 2.3 `CONTRIBUTING.md`

```markdown
# Como contribuir

Obrigado pelo interesse. Este projeto está em desenvolvimento inicial e
contribuições de qualquer tamanho são bem-vindas.

## Relatar um problema

Abra uma [issue](https://github.com/{{USUARIO_GITHUB}}/nconfa/issues) incluindo:

- o que você esperava que acontecesse e o que aconteceu;
- um exemplo reprodutível mínimo (use `reprex::reprex()` se possível);
- a saída de `sessionInfo()`, incluindo as versões de `NCA` e `QCA`.

Para questões metodológicas — discordância sobre uma fórmula, um limiar padrão,
ou a nomenclatura adotada — abra uma issue com o rótulo `metodologia` e cite a
fonte. Esse tipo de discussão é bem-vindo e será registrado publicamente.

## Enviar código

1. Faça um fork e crie um branch a partir de `main`.
2. Siga o estilo tidyverse; rode `styler::style_pkg()` antes de commitar.
3. Toda função nova precisa de teste em `tests/testthat/` e documentação
   roxygen2 com `@examples` executável.
4. Rode `devtools::check()` e garanta 0 erros e 0 warnings.
5. Descreva a mudança em `NEWS.md`.
6. Abra o pull request explicando o problema resolvido, não só o que mudou.

Para mudanças grandes, abra uma issue antes para discutirmos a abordagem.

## Escopo

Antes de propor uma funcionalidade, verifique a seção "Escopo" do README.
Reimplementar o que os pacotes `NCA` e `QCA` já fazem bem está fora do escopo;
melhorar a integração entre eles e a etapa NConfA está dentro.

## Código de conduta

Ao participar, você concorda com o [Código de Conduta](CODE_OF_CONDUCT.md).
```

---

### 2.4 `CODE_OF_CONDUCT.md`

Baixe o Contributor Covenant 2.1 e ajuste o contato:

```bash
curl -sSL https://raw.githubusercontent.com/EthicalSource/contributor_covenant/release/content/version/2/1/code_of_conduct.md \
  -o CODE_OF_CONDUCT.md
```

Depois substitua o marcador de contato do arquivo baixado por `{{EMAIL}}`. Se o
download falhar (rede bloqueada), me avise em vez de escrever um código de
conduta do zero.

---

### 2.5 `SUPPORT.md`

```markdown
# Suporte e governança

## Onde pedir ajuda

- **Bugs e comportamento inesperado:** abra uma
  [issue](https://github.com/{{USUARIO_GITHUB}}/nconfa/issues).
- **Dúvidas de uso e questões metodológicas:** use as
  [Discussions](https://github.com/{{USUARIO_GITHUB}}/nconfa/discussions).

## Expectativas

Este projeto é mantido por {{NOME_COMPLETO}} ({{AFILIACAO}}) como parte de
atividade de pesquisa. Não há garantia de resposta em prazo definido, mas o
compromisso atual é responder issues em até duas semanas.

Enquanto a versão estiver abaixo de 1.0.0, mudanças que quebram a API podem
ocorrer entre versões menores e serão sempre registradas em `NEWS.md`.

## Manutenção

Manutenedor único no momento. Interessados em co-manutenção podem abrir uma
issue.
```

---

### 2.6 `AI-USAGE.md`

```markdown
# Divulgação de uso de IA generativa

O JOSS exige divulgação transparente do uso de IA generativa em software,
documentação e artigo. Este arquivo é mantido atualizado ao longo do
desenvolvimento e serve de base para a seção "AI usage disclosure" do artigo.

## Ferramentas utilizadas

| Ferramenta | Versão / modelo | Onde foi usada |
|---|---|---|
| Claude Code | (registrar) | código, documentação, testes |

## Natureza e escopo da assistência

(Atualizar a cada fase concluída. Exemplos do tipo de registro esperado:
geração de esqueleto de funções a partir de especificação escrita por humano;
refatoração do script Shiny original em funções puras; scaffolding de testes;
revisão de texto de documentação.)

## Decisões de design tomadas por humano

As decisões abaixo foram tomadas pelo autor, não pela ferramenta:

- arquitetura em biblioteca central com Shiny como interface opcional;
- separação de `fs_consistency()` e `fs_coverage()` em funções distintas para
  acomodar a divergência terminológica entre fontes;
- `NCA` e `QCA` como dependências sugeridas, não obrigatórias;
- recusa do uso de `assignInNamespace()` e reimplementação do gráfico de teto;
- limiar de frequência das configurações candidatas como parâmetro, não fixo.

## Verificação

Todo código assistido por IA foi revisado linha a linha pelo autor e é coberto
por testes automatizados em `tests/testthat/`. O autor assume responsabilidade
integral pela correção, originalidade e conformidade de licenciamento do
material submetido.

Nenhuma ferramenta de IA foi usada em comunicação com editores ou revisores.
```

---

### 2.7 `NEWS.md`

```markdown
# nconfa (development version)

- Repositório público criado; escopo, licença e roadmap definidos.
```

---

### 2.8 `.gitignore`

```
.Rproj.user
.Rhistory
.RData
.Ruserdata
*.Rproj
.DS_Store
inst/doc
docs/
*.tar.gz
*.Rcheck/
.Renviron
```

---

### 2.9 `NOTAS-DIVERGENCIAS.md`

```markdown
# Divergências e decisões pendentes

Registro de pontos em que a implementação difere do script Shiny original, ou
em que uma decisão exigiu julgamento. Cada entrada deve dizer o que foi feito,
por quê, e se precisa de revisão do autor.

_(vazio no momento)_
```

---

## 3. Verificação antes de publicar

Rode e me mostre a saída:

```bash
ls -la
grep -rn "{{" . --include="*.md" || echo "OK: nenhum marcador não substituído"
```

Se `grep` encontrar qualquer `{{`, corrija antes de seguir.

---

## 4. Commits

Faça exatamente estes seis commits, nesta ordem, um por vez:

```bash
git add LICENSE.md .gitignore
git commit -m "Add MIT license and R gitignore"

git add README.md
git commit -m "Add README with project scope and methodological rationale"

git add CONTRIBUTING.md CODE_OF_CONDUCT.md
git commit -m "Add contributing guide and code of conduct"

git add SUPPORT.md
git commit -m "Add support and governance expectations"

git add AI-USAGE.md
git commit -m "Add AI usage disclosure, per JOSS policy"

git add NEWS.md NOTAS-DIVERGENCIAS.md
git commit -m "Add changelog and divergence log"
```

---

## 5. Publicar no GitHub

```bash
gh repo create nconfa \
  --public \
  --source=. \
  --remote=origin \
  --push \
  --description "Necessary Configuration Analysis (NConfA) integrada com NCA e fsQCA em R"

gh repo edit --enable-issues --enable-discussions
gh repo edit --add-topic r --add-topic r-package --add-topic qca \
             --add-topic fsqca --add-topic nca --add-topic nconfa \
             --add-topic necessary-condition-analysis \
             --add-topic configurational-analysis
```

Confirme que o GitHub detectou a licença:

```bash
gh repo view --json licenseInfo,visibility,description
```

---

## 6. Rótulos, milestone e issues de roadmap

### 6.1 Rótulos

```bash
gh label create roadmap     --color 0E8A16 --description "Fase planejada do desenvolvimento" || true
gh label create metodologia --color 5319E7 --description "Questão estatística ou terminológica" || true
gh label create joss        --color FBCA04 --description "Requisito de submissão ao JOSS" || true
```

### 6.2 Milestone

```bash
gh api repos/{{USUARIO_GITHUB}}/nconfa/milestones \
  -f title="v0.1.0" \
  -f description="Biblioteca central completa: calibração, NCA, fsQCA, NConfA e relato."
```

### 6.3 Issues

Crie uma issue por fase. Para cada uma, o corpo deve conter: objetivo em uma
frase, lista de funções/arquivos previstos, e critérios de aceite — extraia tudo
do `plano.md`, sem reescrever o método.

```bash
gh issue create --label roadmap --milestone "v0.1.0" \
  --title "Fase 0 — Esqueleto do pacote e CI" \
  --body "Objetivo: criar a estrutura do pacote R e a integração contínua.

Escopo:
- usethis::create_package(), use_mit_license(), use_testthat(3), use_roxygen_md()
- DESCRIPTION com Imports (rlang, cli, stats, utils) e Suggests (QCA, NCA, ggplot2, shiny, DT, testthat, knitr, rmarkdown, withr, vdiffr)
- R/utils-checks.R com os helpers de dependência opcional
- workflow R-CMD-check via usethis::use_github_action()

Aceite: devtools::check() com 0 erros e 0 warnings; CI verde no primeiro push.

Detalhes em plano.md, seção 6, Fase 0."
```

Repita o mesmo padrão para as demais, com títulos:

| Issue | Título |
|---|---|
| Fase 1 | Álgebra de conjuntos fuzzy (`R/sets.R`) |
| Fase 2 | Calibração e diagnóstico de âncoras (`R/calibrate.R`) |
| Fase 3 | Leitura de CSV e dados de demonstração |
| Fase 4 | NCA: execução, efeitos, gargalos e gráfico em ggplot2 |
| Fase 5 | fsQCA: tabela verdade e minimização |
| Fase 6 | NConfA: configurações candidatas e necessidade |
| Fase 7 | Relato: print, summary e tradução para linguagem simples |
| Fase 8 | App Shiny sobre a biblioteca, vinhetas e README final |

Crie ainda três issues fora do milestone:

```bash
gh issue create --label joss \
  --title "Preparar submissão ao JOSS" \
  --body "Requisitos de triagem verificados em setup-repo.md e no plano.md.

- [ ] Repositório público por mais de seis meses com desenvolvimento ativo
- [ ] Evidência de uso em pesquisa (artigo, preprint ou fluxo documentado)
- [ ] Releases com tag, CHANGELOG, testes, CI, CONTRIBUTING, governança
- [ ] paper.md entre 750 e 1750 palavras com as seções obrigatórias
- [ ] AI-USAGE.md completo e refletido na seção de divulgação do artigo
- [ ] Release final com tag e DOI no Zenodo

Referência: https://joss.readthedocs.io/en/latest/submitting.html"

gh issue create --label metodologia \
  --title "Documentar a divergência terminológica coverage vs. consistência de necessidade" \
  --body "Rasoolimanesh & Olya (2025) usam o mesmo par de fórmulas para suficiência e necessidade, chamando-as consistência e cobertura. Ragin (2006), Schneider & Wagemann (2012) e Dul (2016b) chamam o uso da Eq. 2 para necessidade de consistência de necessidade.

Decisão: manter a nomenclatura da fonte do método e documentar a correspondência.

Entregas: vinheta terminologia.Rmd com tabela comparativa; blocos @details em fs_consistency() e fs_coverage()."

gh issue create --label roadmap \
  --title "Roadmap do nconfa" \
  --body "Issue guarda-chuva. Fases em plano.md, seção 6. Acompanhe pelo rótulo roadmap e pelo milestone v0.1.0."
```

Fixe a última:

```bash
gh issue pin <NUMERO_DA_ISSUE_ROADMAP>
```

---

## 7. Adicionar os documentos de planejamento

O `plano.md` e este `setup-repo.md` devem ficar no repositório — fazem parte do
registro de desenvolvimento aberto e demonstram que o projeto foi planejado
antes de implementado.

```bash
mkdir -p .github
cp ../plano.md ./plano.md          # ajuste o caminho se necessário
cp ../setup-repo.md ./setup-repo.md
git add plano.md setup-repo.md
git commit -m "Add implementation plan and repository setup instructions"
git push
```

---

## 8. Critérios de aceite desta sessão

Antes de encerrar, confirme cada item e me reporte:

- [ ] `available::available("nconfa")` sem colisão (saída colada no chat)
- [ ] repositório público, clonável e navegável sem login
- [ ] licença MIT detectada pelo GitHub (`gh repo view --json licenseInfo`)
- [ ] issues e discussions habilitadas
- [ ] README com escopo, motivação, limitações e referências
- [ ] CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT, AI-USAGE, NEWS presentes
- [ ] 7 commits distintos e com mensagens descritivas
- [ ] 12 issues criadas (9 de fase, 1 JOSS, 1 metodologia, 1 roadmap fixada)
- [ ] milestone v0.1.0 criado
- [ ] nenhum marcador `{{...}}` restante em nenhum arquivo
- [ ] nenhuma data de commit alterada

Reporte no chat: URL do repositório, lista dos commits (`git log --oneline`),
lista das issues (`gh issue list`), e qualquer coisa que exigiu julgamento seu.

---

## 9. O que fazer depois de hoje

Não comece a Fase 0 nesta mesma sessão. O histórico precisa mostrar iteração ao
longo do tempo, não uma rajada única.

Ritmo sugerido: uma fase por semana ou quinzena, cada uma fechando sua issue e,
ao fim das Fases 6 e 8, um release com tag (`v0.1.0` e `v0.2.0`). Durante os seis
meses de espera, use o pacote na sua própria pesquisa — é essa evidência que vira
o *Research impact statement* do artigo, e é o requisito mais difícil de
construir retroativamente.
