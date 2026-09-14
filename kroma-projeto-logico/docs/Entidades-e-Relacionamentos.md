**Entidade: Biorreator**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Biorreator` | Inteiro | **PK** |
| `Nome_Tag` | Texto (ex: BR-01) | Nenhuma |
| `Volume` | Decimal | Nenhuma |
| `Agitacao` | Texto | Nenhuma |
| `Material` | Texto | Nenhuma |
| `Status` | Texto (Operação, Pausado) | Nenhuma |

**Entidade: Sensor**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Sensor` | Inteiro | **PK** |
| `ID_Biorreator` | Inteiro | **FK** (Refere a Biorreator) |
| `Tipo` | Texto (pH, Temp, OD, Vazão) | Nenhuma |
| `Precisao` | Decimal | Nenhuma |
| `Data_Ultima_Calibracao` | Data | Nenhuma |
| `Especificacoes_Tecnicas`| Texto | Nenhuma |

**Entidade: Leitura**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Leitura` | Inteiro (Auto-incremento) | **PK** |
| `ID_Sensor` | Inteiro | **FK** (Refere a Sensor) |
| `Valor_Medido` | Decimal | Nenhuma |
| `Data_Hora` | Data e Hora | Nenhuma |

**Entidade: Lote / Experimento**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `Numero_Lote` | Inteiro | **PK** |
| `ID_Biorreator` | Inteiro | **FK** (Refere a Biorreator) |
| `ID_Operador` | Inteiro | **FK** (Refere a Operador) |
| `Linhagem_Cepa` | Texto | Nenhuma |
| `Meio_Cultura` | Texto | Nenhuma |
| `Data_Hora_Inicio` | Data e Hora | Nenhuma |
| `Data_Hora_Termino` | Data e Hora | Nenhuma |
| `Status` | Texto | Nenhuma |

**Entidade: Parametro_Seguranca**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Parametro` | Inteiro | **PK** |
| `Numero_Lote` | Inteiro | **FK** (Refere a Lote) |
| `Tipo_Variavel` | Texto (pH, Temp) | Nenhuma |
| `Valor_Minimo_Toleravel`| Decimal | Nenhuma |
| `Valor_Maximo_Toleravel`| Decimal | Nenhuma |

**Entidade: Alerta e Discrepância**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Alerta` | Inteiro | **PK** |
| `Numero_Lote` | Inteiro | **FK** (Refere a Lote) |
| `ID_Operador_Responsavel`| Inteiro | **FK** (Refere a Operador) |
| `Descricao_Evento` | Texto | Nenhuma |
| `Data_Hora_Ocorrencia` | Data e Hora | Nenhuma |
| `Observacoes` | Texto longo | Nenhuma |

**Entidade: Operador**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Operador` | Inteiro | **PK** |
| `Nome` | Texto | Nenhuma |
| `Registro_Profissional`| Texto | Nenhuma |