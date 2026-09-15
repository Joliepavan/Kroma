**Entidade: Biorreator**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Biorreator` | Inteiro | **PK** |
| `Nome_Tag` | Texto (ex: BR-01) | Nenhuma |
| `Volume` | Decimal | Nenhuma |
| `agitacao_tipo` | Texto | Nenhuma |
| `Material` | Texto | Nenhuma |
| `Status` | Texto (Operação, Pausado) | Nenhuma |
| `Modelo` | Texto (Airlift, STR, Single-use)| Nenhuma |

**Entidade: Sensor**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Sensor` | Inteiro | **PK** |
| `ID_Biorreator` | Inteiro | **FK** (Refere a Biorreator) |
| `id_tipo` | Inteiro | **FK** (Refere a Sensor) |
| `Precisao` | Decimal | Nenhuma |
| `Data_Ultima_Calibracao` | Data | Nenhuma |
| `Especificacoes_Tecnicas`| Texto | Nenhuma |

**Entidade: Tipo_Parametro**
| `id_sensor` | Inteiro | **PK** |
| `Tipo` | Texto (pH, Temp, OD, Vazão, Umidade, Pressao, Velocidade) | Nenhuma |
| `Tipo` | Unidade | Nenhuma |

**Entidade: Leitura**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `id_leitura` | Inteiro (Auto-incremento) | **PK** |
| `id_sensor` | Inteiro | **FK** (Refere a Sensor) |
| `id_lote` | Inteiro | **FK** (Refere a Lote) |
| `valor_medido` | Decimal | Nenhuma |
| `data_hora` | Data e Hora | Nenhuma |

**Entidade: Lote**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `Numero_Lote` | Inteiro | **PK** |
| `ID_Biorreator` | Inteiro | **FK** (Refere a Biorreator) |
| `Linhagem_Cepa` | Texto | Nenhuma |
| `Meio_Cultura` | Texto | Nenhuma |
| `Data_Hora_Inicio` | Data e Hora | Nenhuma |
| `Data_Hora_Termino` | Data e Hora | Nenhuma |
| `Status` | Texto | Nenhuma |

**Entidade: Parametro_Otimo**
| :--- | :--- | :--- |
| `id_parametro` | Inteiro (Auto-incremento) | **PK** |
| `id_lote` | Inteiro | **FK** (Refere a Lote) |
| `id_tipo` | Inteiro | **FK** (Refere a Tipo_Parametro) |
| `valor_minimo` | Decimal | Nenhuma |
| `valor_maximo` | Decimal | Nenhuma |

**Entidade: Operador**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Operador` | Inteiro | **PK** |
| `Nome` | Texto | Nenhuma |
| `Registro_Profissional` | Texto | Nenhuma

**Entidade: Violacao_Parametro**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `id_violacao` | Inteiro | **PK** |
| `id_leitura` | Inteiro | **FK** (Refere a Leitura) |
| `id_parametro` | Inteiro | **FK** (Refere a Parametro_Otimo) |
| `delta_desvio` | Texto | Nenhuma |

**Entidade: Alerta**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `ID_Alerta` | Inteiro | **PK** |
| `id_violacao` | Inteiro | **FK** (Refere a Violacao_Parametro) |
| `ID_Operador`| Inteiro | **FK** (Refere a Operador) |
| `momento_desvio` | Data e Hora | Nenhuma |
| `momento_resposta` | Data e Hora | Nenhuma |
| `status` | Texto (enviado, em atendimento, concluído) | Nenhuma |
