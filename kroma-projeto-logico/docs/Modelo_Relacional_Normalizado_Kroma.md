# Modelo Relacional Normalizado — Sistema Kroma

Este documento apresenta o modelo relacional do Kroma obtido após o mapeamento do DER (`Modelo_Logico_Kroma_Atualizado.pdf`, notação de Chen). O DER não representa FKs nem tipos de dados: as chaves estrangeiras foram deduzidas pelas regras de mapeamento da seção 2, e os tipos seguem o `Dicionario_de_Dados.md`.

## 1. Notação utilizada (representação formal de uma relação)

Cada relação é escrita como **R(A1, A2, ..., An)**, em que R é o nome da relação e A1...An são seus atributos.

- **Atributos em negrito** compõem a chave primária (PK).
- `FK` indica a chave estrangeira e a relação que ela referencia.
- Os nomes seguem o padrão do dicionário de dados: letras minúsculas separadas por `_`.

## 2. Regras de mapeamento aplicadas

1. **Entidade → relação**: cada entidade vira uma relação, e o identificador vira a PK.
2. **Relacionamento 1:N → FK no lado N**: Biorreator 1:N Sensor (`monitora`), Parametro 1:N Sensor (`mede`), Sensor 1:N Leituras (`registra`) e Lote 1:N Condicao_otima (`possui`).
3. **Relacionamento N:N → relação associativa**: recebe as PKs das duas entidades como FKs, que formam a PK composta, mais os atributos próprios. Gerou `FABRICA` (Biorreator × Lote) e `E_CONDUZIDO` (Operador × Lote).
4. **Atributo de relacionamento 1:N vai para o lado N**: `data_hora` de `registra` vai para `LEITURA`.
5. **Atributo multivalorado → relação própria**: `cepa` (1,n) de Lote vira `CEPA_LOTE(id_lote, cepa)`, o que atende à 1FN.

## 3. Esquema relacional

**PARAMETRO**(**parametro_usado**, unidade_grandeza)

**BIORREATOR**(**id_biorreator**, modelo, capacidade, material)

**SENSOR**(**id_sensor**, modelo, resolucao, data_ultima_calibracao, id_biorreator, parametro_usado)
- FK: id_biorreator → BIORREATOR(id_biorreator)
- FK: parametro_usado → PARAMETRO(parametro_usado)

**LEITURA**(**id_leitura**, valor_medido, data_hora, id_sensor, parametro_usado)
- FK: id_sensor → SENSOR(id_sensor)
- FK: parametro_usado → PARAMETRO(parametro_usado)

**LOTE**(**id_lote**)

**CEPA_LOTE**(**id_lote**, **cepa**)
- FK: id_lote → LOTE(id_lote)

**OPERADOR**(**id_operador**, nome, formacao)

**FABRICA**(**id_biorreator**, **id_lote**, data_hora_inicio, data_hora_fim)
- FK: id_biorreator → BIORREATOR(id_biorreator)
- FK: id_lote → LOTE(id_lote)

**E_CONDUZIDO**(**id_operador**, **id_lote**)
- FK: id_operador → OPERADOR(id_operador)
- FK: id_lote → LOTE(id_lote)

**CONDICAO_OTIMA**(**id_lote**, **parametro_usado**, min_faixa, max_faixa)
- FK: id_lote → LOTE(id_lote)
- FK: parametro_usado → PARAMETRO(parametro_usado)

## 4. Tipos de dados

| Relação | Atributo | Tipo |
| :--- | :--- | :--- |
| PARAMETRO | parametro_usado | Texto |
| PARAMETRO | unidade_grandeza | Texto |
| BIORREATOR | id_biorreator, capacidade | Inteiro |
| BIORREATOR | modelo, material | Texto |
| SENSOR | id_sensor, resolucao, id_biorreator | Inteiro |
| SENSOR | modelo | Texto |
| SENSOR | data_ultima_calibracao | Data e hora |
| SENSOR | parametro_usado | Igual ao de PARAMETRO |
| LEITURA | id_leitura, id_sensor | Inteiro |
| LEITURA | valor_medido | Decimal |
| LEITURA | data_hora | Data e hora |
| LEITURA | parametro_usado | Igual ao de PARAMETRO |
| LOTE | id_lote | Inteiro |
| CEPA_LOTE | id_lote | Inteiro |
| CEPA_LOTE | cepa | Texto |
| OPERADOR | id_operador | Inteiro |
| OPERADOR | nome, formacao | Texto |
| FABRICA | id_biorreator, id_lote | Inteiro |
| FABRICA | data_hora_inicio, data_hora_fim | Data e hora |
| E_CONDUZIDO | id_operador, id_lote | Inteiro |
| CONDICAO_OTIMA | id_lote | Inteiro |
| CONDICAO_OTIMA | parametro_usado | Igual ao de PARAMETRO |
| CONDICAO_OTIMA | min_faixa, max_faixa | Decimal |

## 5. Cardinalidades e integridade referencial

| Relacionamento (DER) | Cardinalidade | Mapeamento |
| :--- | :--- | :--- |
| Biorreator — monitora — Sensor | Um biorreator tem vários sensores; cada sensor pertence a um biorreator | FK `id_biorreator` em SENSOR |
| Parametro — mede — Sensor | Cada sensor mede um parâmetro; um parâmetro pode ser medido por vários sensores | FK `parametro_usado` em SENSOR |
| Sensor — registra — Leituras | Um sensor registra várias leituras | FK `id_sensor` em LEITURA; `data_hora` do relacionamento vai para LEITURA |
| Parametro — indica — Leituras | Um parâmetro é indicado por várias leituras | FK `parametro_usado` em LEITURA |
| Biorreator — fabrica — Lote | N:N, (0,n) nos dois lados | Relação associativa FABRICA, com `data_hora_inicio` e `data_hora_fim` |
| Lote — é conduzido — Operador | N:N, (1,n) nos dois lados | Relação associativa E_CONDUZIDO |
| Lote — possui — Condicao_otima | Um lote tem várias condições ótimas | FK `id_lote` em CONDICAO_OTIMA |
| Parametro — define — Condicao_otima | Um parâmetro é definido em várias condições ótimas | FK `parametro_usado` em CONDICAO_OTIMA, compondo a PK |
| Lote — cepa | Atributo multivalorado (1,n) | Relação CEPA_LOTE |

## 6. Verificação de normalização

| Relação | 1FN | 2FN | 3FN | Observação |
| :--- | :---: | :---: | :---: | :--- |
| PARAMETRO | Sim | Sim | Sim | `unidade_grandeza` depende da PK inteira |
| BIORREATOR | Sim | Sim | Sim | PK simples e atributos atômicos |
| SENSOR | Sim | Sim | Sim | `id_biorreator` e `parametro_usado` são FKs |
| LEITURA | Sim | Sim | Não, se `parametro_usado` for mantido | Dependência transitiva `id_leitura → id_sensor → parametro_usado` (ver seção 7) |
| LOTE | Sim | Sim | Sim | Só tem a PK |
| CEPA_LOTE | Sim | Sim | Sim | Só atributos de chave; resolve o atributo multivalorado |
| OPERADOR | Sim | Sim | Sim | PK simples |
| FABRICA | Sim | Sim | Sim | Início e fim dependem do par completo (biorreator, lote) |
| E_CONDUZIDO | Sim | Sim | Sim | Só tem atributos de chave |
| CONDICAO_OTIMA | Sim | Sim | Sim | `min_faixa` e `max_faixa` dependem do par completo (lote, parâmetro) |

## 7. Pontos do Modelo que precisam de Confirmação

1. **PK de `CONDICAO_OTIMA`.** O DER marca apenas `id_lote` como chave e traz (1,1) nos dois lados de `define`. Foi adotado Parametro 1:N Condicao_otima, com PK (`id_lote`, `parametro_usado`), para permitir limites de pH e de temperatura no mesmo lote (RN05).
2. **Relacionamento `indica`.** O DER traz (1,1) nos dois lados; foi adotado 1:N, pois um parâmetro é indicado por muitas leituras. Como o sensor já determina o parâmetro, a FK `parametro_usado` em LEITURA é redundante e fere a 3FN. Para eliminá-la, basta remover `indica` e chegar ao parâmetro por `id_sensor`.
3. **Relacionamento `mede`.** Foi adotado que cada sensor mede um único parâmetro, com a FK em SENSOR.
4. **Tipos de dados.** O DER não informa tipos. `parametro_usado` (Texto) e `min_faixa` e `max_faixa` (Decimal) foram assumidos.
5. **PK de `FABRICA`.** Com (`id_biorreator`, `id_lote`), um lote não passa duas vezes pelo mesmo biorreator. Caso isso seja possível, `data_hora_inicio` deve entrar na chave.
6. **Do lote às leituras.** LEITURA não tem `id_lote`. A consulta por lote (RF04 e RF09) exige cruzar FABRICA, SENSOR e a janela de tempo, o que é ambíguo se dois lotes se sobrepuserem no mesmo biorreator.
7. **Cobertura de requisitos.** CONDICAO_OTIMA atende RF06 e RN05. RF07 e RF08 (alertas e discrepâncias) não são cobertos, pois o DER não tem entidades de violação nem de alerta. LOTE não guarda meio de cultura, o que deixa a RN07 incompleta.
