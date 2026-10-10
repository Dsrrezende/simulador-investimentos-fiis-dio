# simulador-fiis-excel

Ferramenta em Excel que simula investimentos em fundos imobiliários (FIIs): do aporte de cada mês ao patrimônio acumulado e aos dividendos mensais, com projeção de 2 a 30 anos e divisão do aporte por perfil de investidor.

> Todos os valores (aporte, salário, taxa) e percentuais são **exemplos**, não dados pessoais nem recomendação de investimento.

## Arquivo
`simulador_fiis.xlsx`, com duas abas: **Simulador** (a tela) e **Apoio** (tabela de perfis).

## Perguntas que a ferramenta responde

| Pergunta | Onde aparece (aba Simulador) |
|---|---|
| Quanto investir por mês? | `C6` (entrada, nome `aporte`) |
| Por quantos anos? | `C7` (entrada, nome `anos`) |
| Qual a taxa de rendimento mensal? | `C8` (vem das Configurações, `C26`) |
| Quanto de patrimônio vai acumular? | `C12` (nome `patrimonio`) |
| Quanto vai receber de dividendos por mês? | `C13` (nome `dividendos_mensais`) |

Extras: cenários de 2, 5, 10, 20 e 30 anos (`B18:D22`), simulação com a sugestão de 30% do salário (`C30:C31`) e divisão do aporte por perfil (`B36:D42`, com gráfico de pizza que muda junto com o perfil).

## Como VF e PROCV entram nos cálculos

**VF** (valor futuro): `=-VF(taxa_mensal; prazo_meses; aporte)`
- `taxa` = rendimento mensal; `nper` = número de meses (`anos*12`); `pgto` = aporte mensal.
- O sinal negativo existe porque o VF trata o aporte como saída de caixa e devolve o resultado negativo.
- Dividendos mensais = `patrimonio * taxa_mensal`.
- Nos cenários, a fórmula é repetida com o prazo da própria linha (`$B18*12`), com referência absoluta na coluna para não se perder ao arrastar.

**PROCV com chave composta**: na aba Apoio, a coluna A junta perfil e tipo (`=B2&"|"&C2`, ex.: `Moderado|Logística`). Na tela, cada percentual é buscado com:

`=PROCV(perfil&"|"&$B36; tabela_perfil; 4; FALSO)`

Assim um único PROCV encontra a linha certa mesmo havendo um mesmo tipo de fundo em vários perfis.

## Intervalos nomeados

| Nome | Referência | Uso |
|---|---|---|
| `aporte` | Simulador!C6 | aporte mensal |
| `anos` | Simulador!C7 | prazo em anos |
| `taxa_mensal` | Simulador!C8 | taxa mensal usada nos cálculos |
| `prazo_meses` | Simulador!C9 | `anos*12` |
| `patrimonio` | Simulador!C12 | resultado do VF |
| `dividendos_mensais` | Simulador!C13 | patrimônio × taxa |
| `salario` | Simulador!C25 | salário (exemplo) |
| `rendimento_carteira` | Simulador!C26 | rendimento mensal da carteira |
| `perc_aporte` | Simulador!C27 | % do salário sugerido (30%) |
| `sugestao_aporte` | Simulador!C28 | salário × 30% |
| `perfil` | Simulador!C34 | perfil escolhido na lista |
| `tabela_perfil` | Apoio!A2:D19 | tabela do PROCV |
| `lista_perfis` | Apoio!F2:F4 | origem da validação de dados |

## Percentuais por perfil

Criados por mim como **exemplo didático** (não vieram de estudo nem de recomendação). Cada perfil soma 100%, conferido na aba Apoio (coluna G) e na linha Total da tela.

| Tipo de fundo | Conservador | Moderado | Arrojado |
|---|---|---|---|
| Logística | 25% | 20% | 15% |
| Lajes Corporativas | 10% | 15% | 15% |
| Shoppings | 15% | 20% | 25% |
| Papel (CRI) | 30% | 20% | 10% |
| Fundo de Fundos | 15% | 15% | 20% |
| Híbrido | 5% | 10% | 15% |

## O que mudei em relação à ferramenta do Expert
- Taxa mensal ligada à configuração `rendimento_carteira`, evitando dois campos para a mesma informação.
- Linha extra com o total investido do próprio bolso.
- Simulação com a sugestão de 30% do salário (ideia de evolução do desafio).
- Gráfico de pizza da divisão do aporte, que muda com o perfil.
- Verificação automática "soma 100%" na tela e soma por perfil na aba Apoio.
- *(Ajuste aqui o que for diferente no seu caso.)*

## Evidência: mesma simulação em dois perfis
Aporte R$ 1.000, 20 anos, 0,8% a.m. → patrimônio R$ 721.131,22 e dividendos de R$ 5.769,05/mês.

| Conservador | Arrojado |
|---|---|
| ![Conservador](prints/perfil_conservador.png) | ![Arrojado](prints/perfil_arrojado.png) |
