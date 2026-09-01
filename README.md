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
remotes::install_github("mgabriel-br/nconfa")
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
[issues](https://github.com/mgabriel-br/nconfa/issues) com o rótulo
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

MIT © 2026 Marcelo Luiz Dias da Silva Gabriel
