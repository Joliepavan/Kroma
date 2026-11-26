## 1. Requisitos Funcionais (RF)

* **RF01 — Gestão de Biorreatores:** O sistema deve permitir o cadastro, consulta e atualização de biorreatores, armazenando seu identificador único, modelo, capacidade volumétrica e material de construção.
* **RF02 — Gestão de Parâmetros Físico-Químicos:** O sistema deve manter um catálogo padronizado das grandezas monitoradas nos cultivos (ex.: pH, Temperatura, Oxigênio Dissolvido, Vazão) e suas respectivas unidades de medida.
* **RF03 — Gestão e Alocação de Sensores:** O sistema deve permitir o registro de sensores vinculados a um biorreator específico e a um único parâmetro medido, registrando modelo, resolução técnica e data da última calibração.
* **RF04 — Gestão de Operadores Técnicos:** O sistema deve manter o registro dos pesquisadores e operadores responsáveis pela condução dos cultivos, armazenando identificador, nome completo e formação acadêmica/técnica.
* **RF05 — Controle de Lotes e Cepas:** O sistema deve permitir a abertura e o encerramento de lotes de cultivo, vinculando uma ou mais cepas/linhagens biológicas, os biorreatores utilizados (com carimbos de data/hora de início e término) e os operadores responsáveis.
* **RF06 — Configuração de Condições Ótimas:** O sistema deve permitir a definição de faixas operacionais ideais (limite mínimo e limite máximo aceitável) por parâmetro físico-químico para cada lote de produção.
* **RF07 — Coleta Contínua de Leituras (Séries Temporais):** O sistema deve persistir continuamente as medições enviadas pelos sensores, registrando o valor medido, o sensor de origem, o parâmetro indicado e o carimbo exato de data e hora da coleta.
* **RF08 — Auditoria e Rastreabilidade de Lotes:** O sistema deve possibilitar a emissão de relatórios consolidados de rastreabilidade por lote, cruzando dados de equipamentos, operadores, condições ótimas configuradas e histórico de leituras que ultrapassaram os limites estabelecidos.

---

## 2. Requisitos Não Funcionais (RNF)

* **RNF01 — Integridade Referencial Estrita:** Todas as associações entre leituras, sensores, parâmetros, lotes, biorreatores e operadores devem ser garantidas por chaves primárias e estrangeiras no banco de dados, impedindo registros órfãos.
* **RNF02 — Imutabilidade e Auditabilidade Histórica:** As leituras coletadas e os dados de lotes encerrados devem ser preservados integralmente para fins de conformidade regulatória, sem sobrescrita destrutiva dos registros históricos.
* **RNF03 — Desempenho em Séries Temporais:** O banco de dados deve utilizar indexação composta (por sensor/parâmetro e carimbo temporal) para garantir baixa latência em consultas de rastreabilidade e detecção de anomalias em grandes volumes de medições.
* **RNF04 — Precisão Numérica Científica:** Valores de medições, resoluções de sensores e limites de faixas ótimas devem ser armazenados com tipos numéricos de ponto fixo (`DECIMAL`/`NUMERIC`), evitando erros de arredondamento inerentes a tipos de ponto flutuante.
* **RNF05 — Conformidade e Portabilidade SQL:** A estrutura física do banco de dados deve seguir o padrão ANSI SQL (implementado em PostgreSQL), garantindo portabilidade e padronização na definição de restrições de domínio.

---

## 3. Regras de Negócio (RN)

* **RN01 — Alocação Exclusiva de Sensor por Biorreator:** Cada sensor instalado monitora exatamente um biorreator por vez `(1,1)`, enquanto um biorreator pode ser monitorado simultaneamente por um ou mais sensores `(1,n)`.
* **RN02 — Unicidade de Grandeza por Sensor:** Cada sensor é dedicado à medição de exatamente um parâmetro físico-químico `(1,1)`, embora um mesmo parâmetro possa ser aferido por diversos sensores distintos `(1,n)`.
* **RN03 — Rastreabilidade Metrológica de Calibração:** Todo sensor cadastrado deve possuir obrigatoriamente a data de sua última calibração (`data_ultima_calibracao`) registrada, permitindo auditar a confiabilidade técnica das medições obtidas durante um cultivo.
* **RN04 — Alocação Temporal entre Biorreator e Lote:** Um biorreator pode fabricar diversos lotes ao longo do tempo `(0,n)` e um lote pode utilizar um ou mais biorreatores `(0,n)`. Toda alocação deve registrar obrigatoriamente `data_hora_inicio` e, no encerramento, `data_hora_fim`.
* **RN05 — Responsabilidade Técnica Obrigatória:** Nenhum lote de cultivo pode existir sem estar associado a pelo menos um operador técnico cadastrado `(1,n)`, podendo um operador conduzir múltiplos lotes `(1,n)`.
* **RN06 — Composição Biológica do Lote:** Todo lote deve especificar ao menos uma cepa ou linhagem biológica cultivada `(1,n)`, permitindo o rastreamento de monocultivos ou cocultivos.
* **RN07 — Definição e Consistência das Condições Ótimas:** Cada condição ótima pertence a exatamente um lote `(1,1)` e estabelece limites para um único parâmetro `(1,1)`, respeitando obrigatoriamente a regra matemática de que o limite mínimo não pode ultrapassar o limite máximo (`min_faixa <= max_faixa`).
* **RN08 — Consistência Cronológica do Lote:** A data e hora de término da fabricação de um lote em um biorreator (`data_hora_fim`), quando preenchida, deve ser estritamente posterior à data e hora de início (`data_hora_inicio`).
* **RN09 — Caracterização de Discrepância Operacional:** Toda leitura registrada cujo `valor_medido` seja estritamente inferior a `min_faixa` ou estritamente superior a `max_faixa` definida para aquele parâmetro no lote ativo durante o mesmo intervalo temporal configura uma anomalia operacional passível de auditoria.
* **RN10 — Vinculação Integral da Leitura:** Toda medição persistida no banco deve estar obrigatoriamente vinculada tanto ao sensor que a coletou `(1,1)` quanto ao parâmetro físico-químico indicado `(1,1)`, contendo carimbo `data_hora` e `valor_medido` não nulos.
* **RN11 — Delimitação de Escopo Operacional:** O banco de dados do Kroma atua exclusivamente na persistência, monitoramento e auditoria de dados, não executando comandos de atuação física em malha fechada (válvulas, bombas ou aquecedores) nem rotinas financeiras ou comerciais.
