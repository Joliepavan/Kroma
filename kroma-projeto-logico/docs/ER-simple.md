| Entidade | Atributos e Relacionamentos (PK/FK) | Métodos (Ações do Sistema) |
| :--- | :--- | :--- |
| **Biorreator** | `ID_Biorreator` (PK)<br>`Nome_Tag`<br>`Volume`<br>`Agitacao`<br>`Material`<br>`Status` | Cadastrar biorreator, consultar histórico |
| **Sensor** | `ID_Sensor` (PK)<br>`ID_Biorreator` (FK)<br>`Tipo`<br>`Precisao`<br>`Data_Ultima_Calibracao`<br>`Especificacoes_Tecnicas` | Cadastrar sensor, consultar informações |
| **Leitura** | `ID_Leitura` (PK)<br>`ID_Sensor` (FK)<br>`Valor_Medido`<br>`Data_Hora` | Registrar medição contínua, exibir histórico |
| **Lote / Experimento** | `Numero_Lote` (PK)<br>`ID_Biorreator` (FK)<br>`ID_Operador` (FK)<br>`Linhagem_Cepa`<br>`Meio_Cultura`<br>`Data_Hora_Inicio`<br>`Data_Hora_Termino`<br>`Status` | Iniciar lote, finalizar lote, gerar relatórios de rastreabilidade |
| **Parametro_Seguranca** | `ID_Parametro` (PK)<br>`Numero_Lote` (FK)<br>`Tipo_Variavel`<br>`Valor_Minimo_Toleravel`<br>`Valor_Maximo_Toleravel` | Definir faixas ideais por lote, salvar parâmetros |
| **Alerta e Discrepância**| `ID_Alerta` (PK)<br>`Numero_Lote` (FK)<br>`ID_Operador_Responsavel` (FK)<br>`Descricao_Evento`<br>`Data_Hora_Ocorrencia`<br>`Observacoes` | Gravar discrepância, disparar notificação de anomalia |
| **Operador** | `ID_Operador` (PK)<br>`Nome`<br>`Registro_Profissional` | Logar no sistema, assinar responsabilidade por lote |
