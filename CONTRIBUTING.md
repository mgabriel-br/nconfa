# Como contribuir

Obrigado pelo interesse. Este projeto está em desenvolvimento inicial e
contribuições de qualquer tamanho são bem-vindas.

## Relatar um problema

Abra uma [issue](https://github.com/mgabriel-br/nconfa/issues) incluindo:

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
