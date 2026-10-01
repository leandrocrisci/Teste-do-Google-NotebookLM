# Plano de Estudos: Microsoft BizTalk Server & Modernização para Azure Integration Services

Este repositório contém o plano de estudos detalhado e a documentação técnica sobre o **Microsoft BizTalk Server**, cobrindo desde seus fundamentos de arquitetura e desenvolvimento on-premises até a estratégia de modernização e migração para o **Azure Integration Services (AIS)** e **Azure Logic Apps Standard**.

---

## 📌 Visão Geral do Projeto

O **Microsoft BizTalk Server** por mais de duas décadas serviu como o principal hub centralizado de integração empresarial (EAI) e mensageria entre parceiros comerciais (B2B) da Microsoft. Com o anúncio do ciclo de vida final do **BizTalk Server 2020** (com encerramento de vendas em **31 de março de 2027** e fim do suporte principal em **12 de abril de 2028**), este plano de estudos estabelece a ponte técnica necessária entre a arquitetura legada baseada em SQL Server / XML e a nova era de integração em nuvem serverless e distribuída no Azure.

---

## 📚 Estrutura do Plano de Estudos

O plano de estudos está estruturado em 6 módulos progressivos:

### Módulo 1: Fundamentos & Arquitetura do BizTalk Server
* **Arquitetura Publish/Subscribe**: Funcionamento do banco de dados **MessageBox** (SQL Server) e mecanismo de roteamento desacoplado por propriedades de contexto.
* **Adaptadores de Comunicação**: Conectividade nativa (HTTP, FTP, WCF, MSMQ, SQL, POP3/SMTP) e adaptadores LOB (SAP, Siebel, Oracle, JD Edwards).
* **Pipelines de Mensagens**: Processamento de entrada e saída — estágios de Decodificação/Codificação (MIME/SMIME), Desassemblagem/Montagem (Flat Files, EDI, XML), Validação XSD e Resolução de Remetente (Party Resolution).
* **Transformação de Dados**: Definindo contratos de mensagem com **Esquemas XSD** e regras de transformação visual com **Mapas (Maps/XSLT)** e Functoids.

### Módulo 2: Orquestrações & Fluxos de Trabalho
* **Orquestrações XLANG/C#**: Design de processos de negócio graficamente no Visual Studio e compilação em código .NET executável.
* **Gerenciamento de Estado**: Conceitos de persistência (dehydration / rehydration), transações atômicas e de longa duração, correlação de mensagens e padrões avançados (ex: Parallel Convoy).
* **Tratamento de Exceções**: Lógica de compensação e resiliência em falhas de comunicação.

### Módulo 3: Regras de Negócio (BRE) & Observabilidade (BAM)
* **Business Rule Engine (BRE)**: Execução de regras de negócio complexas via algoritmo RETE desacopladas do código da orquestração.
* **Business Activity Monitoring (BAM)**: Rastreamento ponta a ponta e relatórios em tempo real de KPIs para analistas de negócios.
* **Group Hub & Administration Console**: Monitoramento de instâncias suspensas, consultas de mensagens e gerenciamento operacional do BizTalk Group.

### Módulo 4: B2B, EDI & Padrões Avançados de Integração
* **Integração B2B / EDI**: Suporte nativo a padrões X12, EDIFACT, AS2, acordos de parceiros (Trading Partner Management - TPM) e validação de envelopes.
* **WCF LOB Adapter Framework**: Construção e consumo de serviços WCF e exposição de metadados fortemente tipados.
* **ESB Toolkit**: Composição de serviços, encadeamento de rotas dinâmicas (Itineraries) e gestão de erros centralizada.

### Módulo 5: Avaliação Estratégica & Ciclo de Vida
* **Marcos de Suporte**:
  * BizTalk Server 2016: Suporte estendido encerrado em janeiro de 2027.
  * BizTalk Server 2020: Fim de vendas em **31/03/2027**, Fim do suporte principal em **12/04/2028**, e ponte opcional de suporte estendido até **10/04/2030**.
* **Matriz de Riscos**: Escassez de talentos especializados, ausência de atualizações de segurança para novas ameaças e limitações de compatibilidade com protocolos modernos (TLS 1.3, OAuth 2.0).

### Módulo 6: Modernização para Azure Integration Services (AIS)
* **Mapeamento Arquitetural de Componentes**:
  * `MessageBox DB` → **Azure Service Bus (Topics/Subscriptions)** / **Event Grid**
  * `Orchestration (.odx)` → **Azure Logic Apps Standard (Workflows)**
  * `Pipelines & Adapters` → **Logic Apps Connectors** + **Azure Functions**
  * `Maps (.btm / XSLT)` → **Logic Apps Data Mapper** / **Liquid Templates**
  * `Business Rules (BRE)` → **Azure Logic Apps Rules Engine (Public Preview)**
  * `Deployment (BTDF)` → **Bicep / ARM Templates + Azure DevOps YAML Pipelines**
* **Ferramentas de Automação de Migração**:
  * **Azure Logic Apps Migration Agent** (Extensão VS Code com agentes Copilot: `@migration-analyser`, `@migration-planner`, `@migration-converter`).
  * Conversores CLI: `ODXtoWFMigrator.exe` (Orquestrações → Logic Apps JSON), `BTMtoLMLMigrator.exe` (Mapas → LML/Liquid) e `BTPtoLA.exe` (Pipelines → Logic Apps Workflows).
* **Cenários Alternativos On-Premises**: Avaliação de plataformas como **CData Arc** e **Crosser** para cenários com requisitos estritos de soberania de dados ou ambientes air-gapped.

---

## 🗺️ Matriz de Mapeamento Técnico (BizTalk vs. Azure)

| Componente BizTalk Server | Equivalente no Azure Integration Services | Papel Arquitetural |
| :--- | :--- | :--- |
| **MessageBox** | Azure Service Bus / Event Grid | Barramento de mensageria pub-sub distribuído |
| **Orchestration (.odx)** | Azure Logic Apps Standard | Workflow de orquestração durável e com estado |
| **Pipelines & Adapters** | Logic Apps Connectors + .NET Local Functions | Recepção, validação, parsing e envio |
| **Transformations (.btm)** | Data Mapper / XSLT 3.0 / Liquid | Tradução e mapeamento de esquemas de dados |
| **Business Rules (BRE)** | Azure Logic Apps Rules Engine | Avaliação declarativa de regras de negócio |
| **Single Sign-On (SSO)** | Azure Key Vault + Managed Identity | Gestão segura de segredos e credenciais |
| **BAM / Tracking** | Application Insights + Business Process Tracking | Telemetria distribuída e observabilidade |

---

## 🚀 Como Utilizar Este Material de Estudo

1. **Revisão Teórica**: Estude os conceitos de mensageria e orquestração do BizTalk Server nos Módulos 1 a 4 para consolidar a base de integração empresarial.
2. **Análise de Aplicações Legadas**: Utilize os scripts e ferramentas de descoberta (`ODXtoWFMigrator`, `Migration Agent`) para inventariar orquestrações, mapas e pipelines existentes em seu ambiente BizTalk.
3. **Execução de Migração Incremental**:
   * **Fase 1: Discovery** — Grafo de dependências e análise de lacunas.
   * **Fase 2: Planning** — Mapeamento para padrões Azure Logic Apps Standard.
   * **Fase 3: Conversion** — Geração de workflows JSON e funções locais .NET.
   * **Fase 4: Validation & Cutover** — Execução paralela (dual-run) para garantir equivalência semântica antes do desligamento definitivo das rotas no BizTalk.

---

## 🛠️ Tecnologias & Ferramentas Utilizadas

* **On-Premises**: Microsoft BizTalk Server (2010/2016/2020), SQL Server, Visual Studio, XML/XSD/XSLT, XLANG.
* **Nuvem / AIS**: Azure Logic Apps Standard, Azure Service Bus, Azure API Management, Azure Functions, Azure Key Vault, Azure Bicep, Azure DevOps.
* **Ferramental de Migração**: Azure Logic Apps Migration Agent (VS Code), ODXtoWFMigrator, BTMtoLMLMigrator, BTPtoLA, Rules Composer.

---
*Plano de Estudos e Guia de Modernização de Integração Empresarial.*
