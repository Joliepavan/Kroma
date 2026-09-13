# **Levantamento de Requisitos — Sistema Kroma**

---

### **1\. Requisitos Funcionais (RF)**

&nbsp;

| ID | Nome do Requisito | Descrição |
| :---: | :---- | :---- |
| **RF01** | Cadastro de Biorreatores | O sistema deve permitir o cadastro de biorreatores (equipamentos), registrando suas especificações técnicas. *\[Módulo: Gestão de Equipamentos e Sensores\]* |
| **RF02** | Cadastro de Sensores | O sistema deve permitir o cadastro de sensores associados a um biorreator, informando o tipo (pH, temperatura, oxigênio dissolvido, vazão) e suas especificações técnicas. *\[Módulo: Gestão de Equipamentos e Sensores\]* |
| **RF03** | Registro Contínuo de Leituras | O sistema deve armazenar continuamente as leituras enviadas pelos sensores, registrando valor, data/hora e identificador único de cada medição. *\[Módulo: Coleta e Séries Temporais\]* |
| **RF04** | Consulta ao Histórico de Leituras | O sistema deve permitir a consulta ao histórico de leituras de sensores associadas a um biorreator e/ou lote, dando suporte ao monitoramento contínuo e à auditoria. *\[Módulo: Coleta e Séries Temporais\]* |
| **RF05** | Cadastro de Lotes/Experimentos | O sistema deve permitir o registro de lotes (cultivos/experimentos), incluindo linhagem/cepa utilizada, meio de cultura, operador responsável e horário de início e término. *\[Módulo: Controle de Lotes e Experimentos\]* |
| **RF06** | Definição de Parâmetros de Segurança por Lote | O sistema deve permitir a definição, para cada lote, de limites ideais (faixas toleráveis) de pH e temperatura. *\[Módulo: Parâmetros e Regras de Segurança\]* |
| **RF07** | Registro de Alertas e Discrepâncias | O sistema deve registrar um histórico de alertas/discrepâncias sempre que uma leitura de sensor estiver fora da faixa tolerável definida para o lote, para fins de auditoria. *\[Módulo: Parâmetros e Regras de Segurança\]* |
| **RF08** | Emissão de Alertas de Anomalia | O sistema deve emitir alertas de anomalia com base em regras estáticas de limites mínimos e máximos previamente configurados. *\[Módulo: Parâmetros e Regras de Segurança\]* |
| **RF09** | Geração de Relatórios de Rastreabilidade | O sistema deve gerar relatórios consolidados do ciclo de vida completo de cada lote produzido, para fins de validação e auditoria regulatória. *\[Módulo: Rastreabilidade e Conformidade\]* |
| **RF10** | Suporte a Auditorias Regulatórias | O sistema deve disponibilizar os dados de rastreabilidade, histórico de leituras e histórico de alertas de forma estruturada, dando suporte à comprovação de qualidade e conformidade em auditorias externas. *\[Módulo: Rastreabilidade e Conformidade\]* |

---

### **2\. Requisitos Não Funcionais (RNF)**

&nbsp;

| ID | Categoria | Descrição |
| :---: | :---- | :---- |
| **RNF01** | Confiabilidade / Integridade de Dados | O sistema deve garantir a integridade dos dados operacionais e o histórico contínuo das variáveis de processo, evitando perda ou corrupção de registros. |
| **RNF02** | Auditabilidade | O sistema deve manter registros auditáveis de leituras, alertas e lotes, permitindo a reconstrução do histórico completo de um processo para auditorias regulatórias. |
| **RNF03** | Modularidade | A arquitetura de dados deve ser modular, permitindo persistência, auditoria e observabilidade de forma independente e expansível. |
| **RNF04** | Disponibilidade / Continuidade | A camada de persistência de dados deve suportar o armazenamento contínuo de leituras de sensores, sem interrupções que comprometam o monitoramento em tempo real dos cultivos. |
| **RNF05** | Conformidade Regulatória | O sistema deve atender às exigências regulatórias do setor de biotecnologia quanto à documentação e à rastreabilidade dos processos produtivos. |
| **RNF06** | Arquitetura de Dados | O sistema deve ser estruturado sobre um banco de dados relacional centralizado como infraestrutura única de persistência. |

---

### **3\. Regras de Negócio (RN)**

&nbsp;

| ID | Regra | Requisitos Impactados |
| :---: | :---- | :---- |
| **RN01** | O sistema não deve enviar comandos de acionamento ou desativação a atuadores físicos (bombas de dosagem, aquecedores, válvulas de aeração); seu escopo se limita à recepção, persistência e auditoria de dados, sem atuação em malha fechada. | RF08 |
| **RN02** | Está fora do escopo o desenvolvimento de firmware e a comunicação serial/hardware com microcontroladores (Arduino, ESP32, Raspberry Pi); a modelagem cobre apenas a camada de dados. | RF02, RF03 |
| **RN03** | Os alertas de anomalia devem se basear exclusivamente em regras estáticas de faixas mínimas e máximas configuradas; não haverá modelagem de inferência estatística, IA ou previsão automatizada de falhas. | RF07, RF08 |
| **RN04** | O sistema não deve contemplar módulos de gestão financeira, faturamento, vendas, custos de insumos ou precificação de lotes. | — (delimita escopo geral do banco de dados) |
| **RN05** | Todo lote deve possuir limites toleráveis de pH e temperatura definidos para que o monitoramento e a geração de alertas sejam possíveis. | RF06, RF07, RF08 |
| **RN06** | Toda leitura de sensor armazenada deve conter obrigatoriamente valor, data/hora e identificador único da medição. | RF03 |
| **RN07** | Todo lote registrado deve conter obrigatoriamente linhagem/cepa, meio de cultura, operador responsável e horários de início e término. | RF05 |

&nbsp;