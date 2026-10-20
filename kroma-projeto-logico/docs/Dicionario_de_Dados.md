# Dicionário de Dados — Sistema Kroma

Este dicionário registra as tabelas, atributos, tipos e chaves da mudança do
modelo lógico relacional descrita para o projeto. Os nomes foram padronizados
com letras minúsculas e `_`; `Parametro+Leituras` foi escrito como
`parametro_leituras`, pois `+` não é apropriado para um nome relacional sem
delimitação especial.

**Legenda:** PK = chave primária; FK = chave estrangeira; UNIQUE = valor único.
Tipos e restrições não especificados na descrição do modelo são indicados como
“não informado”, sem inferir regras adicionais.

## Tabelas

### Biorreator

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `id_biorreator` | Inteiro | **PK** |
| `modelo` | Texto | — |
| `capacidade` | Inteiro | — |
| `material` | Texto | — |

### Sensor

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `id_sensor` | Inteiro | **PK** |
| `resolucao` | Inteiro | — |
| `modelo` | Texto | — |
| `data_ultima_calibracao` | Data e hora | — |
| `id_biorreator` | Inteiro | **FK** → `Biorreator.id_biorreator` |
| `data_hora` | Data e hora | — |

### Parametro_Leituras

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `unidade_grandeza` | Texto | — |
| `parametro_medido` | Decimal | **Parte da PK composta**, conforme indicado no modelo |
| `id_sensor` | Inteiro | **Parte da PK composta** e **FK** → `Sensor.id_sensor` |
| `valor_medido` | Decimal | — |
| `id_leitura` | Inteiro | Chave não indicada na descrição |

### Fabrica

Tabela associativa entre `Biorreator` e `Lote`.

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `id_biorreator` | Inteiro | **Parte da PK composta** e **FK** → `Biorreator.id_biorreator` |
| `id_lote` | Inteiro | **Parte da PK composta** e **FK** → `Lote.id_lote` |
| `data_hora_inicio` | Data e hora | — |
| `data_hora_fim` | Data e hora | — |

### Lote

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `id_lote` | Inteiro | **PK** |
| `cepa` | Texto | **UNIQUE** |

### Operador

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `id_operador` | Inteiro | **PK** |
| `nome` | Texto | — |
| `formacao` | Texto | — |

### E_Conduzido

Tabela associativa entre `Operador` e `Lote`.

| Atributo | Tipo | Relação / restrição |
| :--- | :--- | :--- |
| `id_operador` | Inteiro | **Parte da PK composta** e **FK** → `Operador.id_operador` |
| `id_lote` | Inteiro | **Parte da PK composta** e **FK** → `Lote.id_lote` |

## Relacionamentos e cardinalidades

| Relacionamento | Cardinalidade descrita |
| :--- | :--- |
| `Biorreator` — `Sensor` | Um biorreator pode ter vários sensores; cada sensor pertence a exatamente um biorreator. |
| `Sensor` — `Parametro_Leituras` | A descrição indica duas linhas de relacionamento, ambas com (1,1) no lado de `Sensor` e (1,n) no lado de `Parametro_Leituras`. |
| `Biorreator` — `Fabrica` — `Lote` | N:N, resolvido por `Fabrica`; cada entidade pode participar de zero a muitos registros associativos, conforme as cardinalidades fornecidas. |
| `Lote` — `Cepa` | A descrição informa uma ligação direta e cardinalidade (1,n) no lado de `Cepa`, mas não define uma tabela `Cepa` nem as chaves necessárias para implementá-la. |
| `Lote` — `E_Conduzido` — `Operador` | N:N, resolvido por `E_Conduzido`; a descrição informa cardinalidade (1,n) para ambos os lados. |

## Pontos do modelo que precisam de confirmação

- `id_sensor` aparece duas vezes visualmente em `Parametro+Leituras`. Foi
  registrado uma única vez, pois uma tabela relacional não pode ter duas
  colunas com o mesmo nome. A descrição não permite saber se as duas linhas
  representam relacionamentos diferentes que exigiriam FKs distintas.
- `id_leitura` não foi marcado como chave na descrição fornecida. Por isso,
  este dicionário não o declara PK; a chave primária de
  `Parametro_Leituras` foi registrada conforme os atributos indicados como
  chave (`parametro_medido` e `id_sensor`).
- O texto da entidade `Lote` contém `cepacepa`; foi normalizado para o atributo
  `cepa`, marcado como único conforme a descrição. Como também é citado um
  relacionamento com uma possível tabela `Cepa`, é necessário confirmar se
  `cepa` é somente atributo de `Lote` ou uma entidade separada. Se for tabela,
  sua PK e a FK correspondente em `Lote` ainda precisam ser definidas.
- `data_hora` foi mantido em `Sensor`, onde aparece na lista de atributos
  fornecida. Sua finalidade não foi especificada.
