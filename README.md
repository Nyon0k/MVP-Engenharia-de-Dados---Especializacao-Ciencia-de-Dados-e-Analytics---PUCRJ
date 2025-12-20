# MVP-Engenharia-de-Dados---Especializacao-Ciencia-de-Dados-e-Analytics---PUCRJ

# Data Domains & Validation Rules

Este documento descreve os **domínios permitidos**, **intervalos válidos** e **regras básicas de qualidade de dados**
para as colunas do dataset de crédito.

Essas regras servem como:
- Contrato de dados (Data Contract)
- Base para validações automáticas
- Referência de negócio e auditoria

---

## Domínios e Intervalos Permitidos das Colunas

### Convenções
- `+∞` = sem limite superior  
- `today` = data atual  
- Booleanos aceitam `{true, false}` (ou `{0,1}`, conforme padrão do dataset)

---

| Coluna | Tipo | Domínio / Intervalo Permitido | Aceita Nulo |
|------|------|-------------------------------|-------------|
| `uf` | set | `ac, al, ap, am, ba, ce, df, es, go, ma, mt, ms, mg, pa, pb, pr, pe, pi, rj, rn, rs, ro, rr, sc, sp, se, to` | Não |
| `regiao` | set | `norte, nordeste, centro-oeste, sudeste, sul` | Não |
| `canal` | set | `app, internet banking, agencia, correspondente` | Não |
| `produto` | set | `consignado, veiculo, pessoal, home_equity, cartao` | Sim |
| `escolaridade` | set | `medio, fundamental, pos, superior` | Não |
| `estado_civil` | set | `solteiro, divorciado, casado, viuvo` | Não |
| `vinculo_emprego` | set | `clt, autonomo, servidor, desempregado, empresario, estudante` | Não |
| `setor` | set | `educacao, saude, comercio, industria, ti, construcao, servicos, outro` | Não |
| `usa_internet_banking` | boolean | `true, false` | Não |
| `possui_cartao_credito` | boolean | `true, false` | Não |
| `possui_investimentos` | boolean | `true, false` | Não |
| `possui_seguro` | boolean | `true, false` | Não |
| `inadimplente` | set | `0, 1` | Não |
| `data_solicitacao` | date_range | `1900-01-01` até `today` | Não |
| `idade` | range | `0` a `120` | Não |
| `tempo_conta_anos` | range | `0` a `+∞` | Não |
| `tempo_emprego_anos` | range | `0` a `+∞` | Sim |
| `qtd_produtos_bancarios` | range_int | inteiros `0` a `+∞` | Não |
| `renda_mensal_atual` | range | `0` a `+∞` | Sim |
| `renda_mensal_anterior` | range | `0` a `+∞` | Sim |
| `atrasos_passados` | range_int | inteiros `0` a `+∞` | Não |
| `score_credito` | range | `0` a `1000` | Não |
| `parcelas` | range_int | inteiros `1` a `+∞` | Não |
| `valor_emprestimo` | range | `0` a `+∞` | Não |
| `taxa_juros_mensal` | range | `0` a `+∞` | Não |
| `valor_parcela` | range | `0` a `+∞` | Não |
| `dti` | range | `0` a `+∞` | Não |
| `frequencia_transacoes` | range | `0` a `+∞` | Não |
| `valor_emprestimos_anteriores` | range | `0` a `+∞` | Sim |
| `pd_true` | range | `0` a `1` | Não |
| `pd_model` | range | `0` a `1` | Não |
| `lgd` | range | `0` a `1` | Não |
| `ead` | range | `0` a `+∞` | Não |
| `valor_recuperado` | range | `0` a `+∞` | Não |

---

## Observações de Modelagem

- Campos categóricos utilizam **domínio fechado** para evitar *data drift*.
- Variáveis financeiras e de exposição não aceitam valores negativos.
- Variáveis de risco (`PD`, `LGD`) seguem domínio probabilístico `[0,1]`.
