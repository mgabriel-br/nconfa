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
