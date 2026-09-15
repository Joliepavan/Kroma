**Dicionário de Dados - Sistema Kroma**

**Entidade: Biorreator**
* **Métodos:** Cadastrar biorreator, consultar histórico.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `ID_Biorreator` | Inteiro | **PK** (Chave Primária) |
| `Nome_Tag` | Texto (ex: BR-01) | NOT NULL |
| `Volume` | Decimal | > 0 |
| `Agitacao` | Texto | Nenhuma |
| `Material` | Texto | Nenhuma |
| `Status` | Texto | Operação, Manutenção, Pausado |


**Entidade: Sensor**
* **Métodos:** Cadastrar sensor, consultar informações.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `ID_Sensor` | Inteiro | **PK** |
| `ID_Biorreator` | Inteiro | **FK** (Refere a `Biorreator.ID_Biorreator`) |
| `Tipo` | Texto | pH, Temp, OD, Vazão |
| `Precisao` | Decimal | Nenhuma |
| `Data_Ultima_Calibracao`| Data | Nenhuma |
| `Especificacoes_Tecnicas`| Texto | Nenhuma |


**Entidade: Leitura**
* **Métodos:** Registrar medição contínua no banco, exibir histórico.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `ID_Leitura` | Inteiro (Auto-incremento) | **PK** |
| `ID_Sensor` | Inteiro | **FK** (Refere a `Sensor.ID_Sensor`) |
| `Valor_Medido` | Decimal | NOT NULL |
| `Data_Hora` | Data e Hora | NOT NULL |


**Entidade: Lote / Experimento**
* **Métodos:** Iniciar lote, finalizar lote, gerar relatórios de rastreabilidade.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `Numero_Lote` | Inteiro | **PK** |
| `ID_Biorreator` | Inteiro | **FK** (Refere a `Biorreator.ID_Biorreator`) |
| `ID_Operador` | Inteiro | **FK** (Refere a `Operador.ID_Operador`) |
| `Linhagem_Cepa` | Texto | NOT NULL |
| `Meio_Cultura` | Texto | NOT NULL |
| `Data_Hora_Inicio` | Data e Hora | NOT NULL |
| `Data_Hora_Termino` | Data e Hora | Pode ser nulo (se em andamento) |
| `Status` | Texto | Em andamento, Concluído, Pausado |


**Entidade: Parametro_Seguranca**
* **Métodos:** Definir faixas ideais por lote, salvar parâmetros.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `ID_Parametro` | Inteiro | **PK** |
| `Numero_Lote` | Inteiro | **FK** (Refere a `Lote.Numero_Lote`) |
| `Tipo_Variavel` | Texto | pH, Temperatura |
| `Valor_Minimo_Toleravel`| Decimal | NOT NULL |
| `Valor_Maximo_Toleravel`| Decimal | NOT NULL |


**Entidade: Alerta e Discrepância**
* **Métodos:** Gravar discrepância, disparar notificação de anomalia.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `ID_Alerta` | Inteiro | **PK** |
| `Numero_Lote` | Inteiro | **FK** (Refere a `Lote.Numero_Lote`) |
| `ID_Operador_Responsavel`| Inteiro | **FK** (Refere a `Operador.ID_Operador`) |
| `Descricao_Evento` | Texto | NOT NULL |
| `Data_Hora_Ocorrencia` | Data e Hora | NOT NULL |
| `Observacoes` | Texto longo | Pode ser nulo |


**Entidade: Operador**
* **Métodos:** Logar no sistema, assinar responsabilidade por um lote.

| Nome Atributo | Tipo / Descrição | Relações / Restrições |
| :--- | :--- | :--- |
| `ID_Operador` | Inteiro | **PK** |
| `Nome` | Texto | NOT NULL |
| `Registro_Profissional` | Texto | UNIQUE |
