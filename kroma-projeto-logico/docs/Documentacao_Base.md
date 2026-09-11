# Documentação Base do Sistema - Kroma

## 1. Integrantes do Grupo
* Ana Beatriz Vaz de Souza
* Elisa Correia Visconde
* Emilly Ranny Dias Silva
* Jolie Lopes Pavan
* Thyago Divino

---

## 2. Nome do Projeto e Área do Negócio
* **Nome do Sistema:** Kroma - Sistema de Monitoramento e Rastreabilidade de Biorreatores
* **Área de Negócio:** Biotecnologia Aplicada, Automação de Processos Biológicos e Gestão de Laboratórios de Pesquisa e Desenvolvimento (P&D)

---

## 3. Objetivo do Sistema
O objetivo principal do sistema Kroma é prover uma infraestrutura de dados relacional centralizada, confiável e modular para o armazenamento, monitoramento contínuo, auditabilidade e rastreabilidade de processos fermentativos e cultivos biológicos em biorreatores.

O sistema visa garantir a integridade dos dados operacionais e o histórico contínuo das variáveis de processo (como pH, temperatura e oxigênio dissolvido), permitindo a rastreabilidade total de lotes de produção e o suporte a auditorias regulatórias no setor de biotecnologia.

---

## 4. Cenário e Contexto Operacional
Em ambientes de cultivo biológico, como laboratórios de P&D, plantas-piloto e indústrias biotecnológicas, biorreatores mantêm microrganismos e linhagens celulares sob condições ambientais rigorosamente controladas para a síntese de bioinsumos (biofármacos, vacinas, enzimas e biocombustíveis).

A sensibilidade biológica desses organismos exige monitoramento constante. Oscilações não detectadas em parâmetros críticos resultam na perda de lotes inteiros, contaminação de meios de cultura e prejuízos financeiros expressivos. Além do risco produtivo, há um forte gargalo regulatório: a produção de bioinsumos exige documentação e rastreabilidade total dos processos para auditorias e conformidade. Sem uma estrutura de dados confiável, torna-se inviável comprovar a qualidade e o histórico de cada lote.

As soluções comerciais atuais (como os sistemas SCADA industriais) são altamente custosas e engessadas, focadas prioritariamente na grande indústria. O Kroma surge como uma solução acessível, modular e focada na camada de dados, atuando como uma plataforma centralizada para persistência, auditoria e observabilidade de cultivos biológicos.

### Público-Alvo:
1. Laboratórios de pesquisa universitários e plantas-piloto;
2. Startups e pequenas/médias empresas de biotecnologia;
3. Indústrias do setor alimentício (processos fermentativos) e de biocombustíveis.

---

## 5. Escopo do Sistema do Banco de Dados

### 5.1. Escopo Incluído (O que será modelado)
Ao longo do semestre, será modelado o banco de dados relacional para suportar os seguintes pilares operacionais do negócio:

* **Gestão de Equipamentos e Sensores:** Cadastro de biorreatores, tipos de sensores alocados (pH, temperatura, O₂ dissolvido, vazão) e suas especificações técnicas.
* **Coleta e Séries Temporais:** Armazenamento contínuo das leituras enviadas pelos sensores, registrando valor, data/hora e identificador da medição.
* **Controle de Lotes e Experimentos:** Registro dos cultivos (lotes), incluindo tipo de linhagem/cepa utilizada, meio de cultura, operador responsável e horário de início/término.
* **Parâmetros e Regras de Segurança:** Definição de limites ideais (faixas toleráveis de pH e temperatura) por lote, gerando histórico de alertas/discrepâncias para auditoria.
* **Rastreabilidade e Conformidade:** Estruturação de relatórios consolidados do ciclo de vida completo de cada lote produzido para validação e auditoria regulatória.

### 5.2. Escopo Excluído (Fora de Escopo)
Para garantir o alinhamento técnico e o cumprimento dos prazos do projeto de banco de dados, os seguintes itens não farão parte do escopo de modelagem:

* **Atuação Física e Controle em Malha Fechada:** O banco de dados e o sistema não enviarão comandos de acionamento ou desativação direta de atuadores (bombas de dosagem, aquecedores, válvulas de aeração). O escopo limita-se à recepção, persistência e auditoria de dados.
* **Desenvolvimento de Firmware e Comunicação Serial/Hardware:** A codificação dos microcontroladores (Arduino, ESP32, Raspberry Pi) e a camada física de captura de sinal elétrico dos sensores não farão parte da modelagem de dados.
* **Análise Preditiva e Algoritmos de Machine Learning:** O sistema registrará regras estáticas de alertas com base em faixas mínimas e máximas configuradas. Não haverá modelagem para inferência estatística, inteligência artificial ou previsão automatizada de falhas de cultivo.
* **Gestão Financeira, Faturamento e Vendas:** O sistema foca estritamente no processo produtivo biológico e na observabilidade do laboratório. Módulos de custos de insumos, faturamento de lotes, precificação ou gestão financeira comercial não serão contemplados no banco.
