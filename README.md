# MVP-Engenharia-de-Dados---Especializacao-Ciencia-de-Dados-e-Analytics---PUCRJ

## Introdução ao projeto

O projeto consiste na aplicação dos conceitos de engenharia de dados na elaboração de um ambiente analitico para a área de risco de crédito. 

## Contextualizando
O Risco de Crédito está relacionado à possibilidade de um cliente não cumprir suas obrigações financeiras, como atrasar ou deixar de pagar um empréstimo. Para lidar com esse risco, instituições financeiras utilizam modelos analíticos que estimam indicadores como a Probabilidade de Default (PD), a Exposição ao Default (EAD) e a Perda Esperada (PE), permitindo avaliar o risco tanto de clientes individuais quanto da carteira como um todo.

Para que essas análises sejam confiáveis e escaláveis, é fundamental que os dados estejam organizados e modelados de forma adequada. Nesse contexto, a criação de um Datamart de Risco de Crédito permite estruturar informações relevantes (cliente, geografia, tempo, setor e canal) em um modelo analítico, facilitando consultas, análises comparativas e a visualização do risco sob diferentes perspectivas, com o intuito de entender comportamentos e tendencias para aprimoramento de modelos que auxiliam na mitigação de riscos.

## Perguntas a serem respondidas

### Geografia:
- Quais UFs com maior EAD (último mês)?
- Quais Regiões com maior EAD (último mês)?
- Quais UFs com maior Perda Esperada no último mês?
- Quais UFs somam 80% do EAD (último mês)?
- Qual a taxa de recuperação por UF (valor_recuperado / perda_esperada) no último mês?
- Qual a evolução mensal de EAD por UF (últimos 12 meses)?
- Qual o crescimento de EAD por UF (mês atual vs mês anterior)?

### Clientes:
- Qual a relação entre tempo de conta e risco?
- Qual a relação entre tempo de emprego e risco?
- A PD Modelo e PD Real por setor estão muito diferentes?
- A PD Modelo e PD Real por escolaridade estão muito diferentes?
- Qual o risco (Perda Esperada) por escolaridade?
- A PD Modelo e PD Real por faixa etária estão muito diferentes?
- Qual a inadimplência média por idade?

As análises de Geografia permitem identificar concentrações regionais de risco e exposição ao crédito, enquanto as análises de Cliente exploram como o perfil etário influencia o comportamento de risco, score e perda esperada da carteira.

## Sobre o projeto

### Processo

O processo de ETL paraa criação do Data Mart foi desenvolvido em 4 etapas:

- Etapa 1 - Ingestão de dados:

Esta é a mais simples de todas, é nessa em que os dados são extraídos da sua fonte, que é o google drive, e apenas processado para virar uma tabela no catalogo do databricks, sem processamento nenhum.

- Etapa 2 - Higienização e adequação no schema:

Aqui é a etapa em que a tabela criada previamente na camada bronze recebe tratamentos e validações para garantir que os dados fiquem padronizados de acordo com as regras de domínio, formatos e negócio.

- Etapa 3 - Criação do esquema estrela:

Por fim, é aqui que o Data Mart é criado, a partir da tabela da camada silver são criadas as dimensões e fatos. As dimensões são: dim_tempo, dim_localizacao, dim_cliente, dim_produto e dim_canal. As fatos são: fato_risco_cliente_mes e fato_solicitacao_credito. O esquema estrela gerado e suas variáveis e tipos pode ser vistas na imagem a seguir. 

![Star Schema](star_schema_mvp_engdados_puc.png)

- Etapa 4 - Analises

Etapa em que o Data Mart está preparado para ser utilizado a fim de responder as perguntas elaboradas.

## Data Domains & Validation Rules

Este documento descreve os **domínios permitidos**, **intervalos válidos** e **regras básicas de qualidade de dados**
para as colunas do dataset de crédito.

Essas regras servem como:
- Contrato de dados (Data Contract)
- Base para validações automáticas
- Referência de negócio e auditoria

---

### Domínios e Intervalos Permitidos das Colunas

#### Convenções
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

## Dicionário das variáveis

| Variável                       | Descrição                                               |
| ------------------------------ | ------------------------------------------------------- |
| `id_cliente`                   | Identificador único do cliente                          |
| `data_solicitacao`             | Data em que o crédito foi solicitado                    |
| `uf`                           | Unidade Federativa (estado) do cliente                  |
| `regiao`                       | Região do Brasil correspondente ao estado               |
| `idade`                        | Idade do cliente                                        |
| `tempo_conta_anos`             | Tempo de relacionamento do cliente com o banco, em anos |
| `escolaridade`                 | Grau de instrução do cliente                            |
| `estado_civil`                 | Estado civil do cliente                                 |
| `vinculo_emprego`              | Tipo de vínculo empregatício do cliente                 |
| `setor`                        | Setor de atuação profissional do cliente                |
| `tempo_emprego_anos`           | Tempo no emprego atual, em anos                         |
| `canal`                        | Canal utilizado para contratar o crédito                |
| `produto`                      | Tipo de crédito contratado                              |
| `usa_internet_banking`         | Indica se o cliente utiliza internet banking            |
| `possui_cartao_credito`        | Indica se o cliente possui cartão de crédito ativo      |
| `possui_investimentos`         | Indica se o cliente possui investimentos no banco       |
| `possui_seguro`                | Indica se o cliente possui seguro contratado            |
| `qtd_produtos_bancarios`       | Quantidade de produtos bancários ativos                 |
| `renda_mensal_atual`           | Renda mensal atual declarada pelo cliente               |
| `renda_mensal_anterior`        | Renda mensal declarada no período anterior              |
| `atrasos_passados`             | Quantidade de atrasos em pagamentos anteriores          |
| `score_credito`                | Score de crédito do cliente                             |
| `parcelas`                     | Número de parcelas do contrato                          |
| `valor_emprestimo`             | Valor do empréstimo contratado                          |
| `taxa_juros_mensal`            | Taxa de juros mensal aplicada ao contrato               |
| `valor_parcela`                | Valor da parcela do empréstimo                          |
| `dti`                          | Proporção entre o valor da parcela e a renda mensal     |
| `frequencia_transacoes`        | Frequência mensal de transações bancárias               |
| `valor_emprestimos_anteriores` | Valor total de empréstimos contratados anteriormente    |
| `pd_true`                      | Probabilidade real de inadimplência observada           |
| `inadimplente`                 | Indicador de inadimplência do cliente                   |
| `ead`                          | Valor exposto no contrato de crédito                    |
| `lgd`                          | Proporção da perda financeira em caso de inadimplência  |
| `valor_recuperado`             | Valor recuperado após inadimplência                     |
| `pd_model`                     | Probabilidade de inadimplência estimada por modelo      |

## Qualidade dos dados

A análise de qualidade dos dados foi realizada na Etapa 2 – Higienização e Adequação no Schema, com foco em garantir consistência e confiabilidade para as análises de risco de crédito.

Foi feita uma avaliação atributo a atributo para identificar valores ausentes, outliers, inconsistências de domínio e tipos de dados inadequados. Variáveis com valores ausentes foram tratadas de forma controlada, preservando a coerência das análises, enquanto outliers plausíveis foram mantidos para não distorcer o perfil real da carteira.

As variáveis categóricas passaram por padronização de valores e validação de domínio, assegurando consistência entre categorias como UF, região, canal e produto. Também houve adequação dos tipos de dados para evitar problemas em cálculos e agregações.

## Respondendo as perguntas

### Geografia:
- GP1 - Quais UFs com maior EAD (último mês)?

![Resposta GP1](imagens/gp1.png)

- GP2 - Quais Regiões com maior EAD (último mês)?

![Resposta GP2](imagens/gp2.png)

- GP6 - Quais UFs com maior Perda Esperada no último mês?

![Resposta GP6](imagens/gp6.png)

- GP10 - Quais UFs somam 80% do EAD (último mês)?

![Resposta GP10](imagens/gp10.png)

- GP7 - Qual a taxa de recuperação por UF (valor_recuperado / perda_esperada) no último mês?

![Resposta GP7](imagens/gp7.png)

- GP8 - Qual a evolução mensal de EAD por UF (últimos 12 meses)?

![Resposta GP8](imagens/gp8.png)

- GP9 - Qual o crescimento de EAD por UF (mês atual vs mês anterior)?

![Resposta GP9](imagens/gp9.png)

### Clientes:
- CP1 - Qual a relação entre tempo de conta e risco?

![Resposta CP1](imagens/cp1.png)

- CP2 - Qual a relação entre tempo de emprego e risco?

![Resposta CP2](imagens/cp2.png)

- CP3 - A PD Modelo e PD Real por setor estão muito diferentes?

![Resposta CP3](imagens/cp3.png)

- CP7 - A PD Modelo e PD Real por escolaridade estão muito diferentes?

![Resposta CP7](imagens/cp7.png)

- CP4 - Qual o risco (Perda Esperada) por escolaridade?

![Resposta CP4](imagens/cp4.png)

- CP5 - A PD Modelo e PD Real por faixa etária estão muito diferentes?

![Resposta CP5](imagens/cp5.png)

- CP6 - Qual a inadimplência média por idade?

![Resposta CP6](imagens/cp6.png)

