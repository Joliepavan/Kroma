**Entidade: Biorreator**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `id_biorreator` | Inteiro | **PK** |
| `nome_tag` | Texto (ex: BR-01) | Nenhuma |
| `volume` | Decimal | Nenhuma |
| `agitacao_tipo` | Texto | Nenhuma |
| `material` | Texto | Nenhuma |
| `status` | Texto (Operação, Pausado) | Nenhuma |
| `modelo` | Texto (Airlift, STR, Single-use)| Nenhuma |

**Entidade: Sensor**
| Atributo | Tipo/Descrição | Relação |
| :--- | :--- | :--- |
| `id_sensor` | Inteiro | **PK** |
| `id_biorreator` | Inteiro | **FK** (Refere a Biorreator) |
| `id_tipo` | Inteiro | **FK** (Refere a Sensor) |
| `precisao` | Decimal | Nenhuma |
| `data_ultima_calibracao` | Data | Nenhuma |
| `especificacoes_tecnicas`| Texto | Nenhuma |

**Entidade: Tipo_Parametro**
| `id_sensor` | Inteiro | **PK** |
| `tipo` | Texto (pH, Temp, OD, Vazão, Umidade, Pressao, Velocidade) | Nenhuma |
| `tipo` | Unidade | Nenhuma |

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
| `numero_lote` | Inteiro | **PK** |
| `id_biorreator` | Inteiro | **FK** (Refere a Biorreator) |
| `cepa` | Texto | Nenhuma |
| `batelada_tipo` | Texto (simples, alimentada)| Nenhuma |
| `meio_cultura` | Texto | Nenhuma |
| `data_hora_inicio` | Data e Hora | Nenhuma |
| `data_hora_termino` | Data e Hora | Nenhuma |
| `status` | Texto | Nenhuma |

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
| `id_operador` | Inteiro | **PK** |
| `nome` | Texto | Nenhuma |
| `registro_Profissional` | Texto | Nenhuma

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
| `id_alerta` | Inteiro | **PK** |
| `id_violacao` | Inteiro | **FK** (Refere a Violacao_Parametro) |
| `id_operador`| Inteiro | **FK** (Refere a Operador) |
| `momento_desvio` | Data e Hora | Nenhuma |
| `momento_resposta` | Data e Hora | Nenhuma |
| `status` | Texto (enviado, em atendimento, concluído) | Nenhuma |
