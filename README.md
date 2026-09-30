# Workshop Databricks — Hands-on para Data Engineering e Data Analytics

## Visão Geral

Workshop hands-on onde cada participante executa as etapas junto com o facilitador, criando artefatos reais no workspace sobre dados de suporte de TI.

| Horário | Duração | Atividade |
|---------|---------|----------|
| 09:00–09:10 | 10 min. | Abertura |
| 09:10–09:25 | 15 min. | Unity Catalog — Governança, Linhagem e Segurança |
| 09:25–10:00 | 35 min. | Lakeflow Designer — ETL Visual (No-code) |
| 10:00–10:20 | 20 min. | Metric View — Camada Semântica Centralizada |
| 10:20–10:40 | 20 min. | SQL Editor — Assistente AI e AI Functions |
| 10:40–10:50 | 10 min. | Coffee Break |
| 10:50–11:50 | 60 min. | Genie Agents |
| 11:50–12:00 | 10 min. | AI/BI Dashboard + Genie One |

**Público-alvo:** Engenheiros de Dados e Analistas de Dados

**Dados utilizados:** Tickets de suporte de TI (~2.300 tickets, atendentes, SLA e comentários) + 6 PDFs de políticas e procedimentos internos (para Knowledge Agent)

---

## Estrutura do Repositório

```
Workshop-Plataforma-DataEng-DataAnalysts/
├── README.md                              ← Este arquivo
├── 00_Config                              ← Configuração central (catalog/schema)
├── 00_Setup_Workshop                      ← Script de setup (importa CSVs como tabelas bronze)
├── Workshop DataEng e DataAnalysts        ← Roteiro hands-on (siga este notebook)
└── data/                                  ← Dados do workshop (versionados no git)
    ├── tb_tickets.csv                     ← 2.331 tickets de suporte
    ├── tb_agents.csv                      ← 2.330 registros de atendentes
    ├── tb_sla.csv                         ← 2.330 registros de SLA
    ├── tb_tickets_comments.csv            ← 142 comentários de tickets
    └── pdf/                               ← Políticas e procedimentos internos (para Knowledge Agent)
        ├── 01_SLA_e_Niveis_de_Servico.pdf
        ├── 02_Processos_de_Escalacao.pdf
        ├── 03_Seguranca_e_Controle_de_Acesso.pdf
        ├── 04_Qualidade_e_Padroes_de_Atendimento.pdf
        ├── 05_Gestao_de_Mudancas_e_CMDB.pdf
        └── 06_Continuidade_de_Servico_e_DR.pdf
```

---

## Pré-requisitos

- Workspace Databricks com Unity Catalog habilitado
- Acesso a um catalog com permissão de criar schemas e tabelas
- Compute Serverless habilitado
- AI Functions habilitadas (para SQL Editor e Genie Agents)
- Feature Preview "Analyze Files in Volumes with Genie Agents" habilitada (para a seção 5.2)

---

## Passo 1 — Clonar o Repositório no Databricks

1. No workspace Databricks, vá em **Workspace** no menu lateral
2. Navegue até a pasta onde deseja clonar (ex: sua pasta de usuário)
3. Clique em **⚙️** (kebab menu) > **Create** > **Git Folder**
4. Cole a URL do repositório Git:
   ```
   https://github.com/rodrigocosin/Workshop-Plataforma-DataEng-DataAnalysts
   ```
5. Escolha o branch `main` e clique em **Create Git Folder**
6. Aguarde a clonagem — todos os notebooks e CSVs serão importados automaticamente

> 💡 **Alternativa:** Se não usar Git, você pode importar os notebooks manualmente via **Import** na pasta de destino.

---

## Passo 2 — Configurar Catalog e Schema

1. Abra o notebook **`00_Config`**
2. Na primeira célula, altere as variáveis:
   - `CONFIG_CATALOG`: nome do catalog (ex: `meu_catalog`)
   - `CONFIG_SCHEMA`: nome do schema (ex: `rcosin`)
3. Execute a célula para validar a configuração

> Todos os notebooks herdam essas variáveis automaticamente via `%run ./00_Config`. As células de prompt no roteiro geram nomes totalmente qualificados a partir dessas variáveis — não há hardcodes a ajustar manualmente.

---

## Passo 3 — Executar o Setup

1. Abra o notebook **`00_Setup_Workshop`**
2. Execute todas as células em ordem
3. O setup irá:
   - Usar o catalog e schema definidos no `00_Config`
   - Importar os 4 CSVs como tabelas Delta (bronze) no Unity Catalog:
     - `tb_tickets` (2.331 registros)
     - `tb_agents` (2.330 registros)
     - `tb_sla` (2.330 registros)
     - `tb_tickets_comments` (142 registros)
4. Confirme que a célula de validação exibe as 4 tabelas com as contagens corretas

---

## Passo 4 — Seguir o Roteiro Hands-on

Abra o notebook **`Workshop DataEng e DataAnalysts`** e siga as seções em ordem. Cada seção produz um artefato que é reutilizado nas etapas seguintes.

### Seção 1 — Unity Catalog
- Navegar pelo catalog e inspecionar as tabelas bronze
- Gerar descrição da tabela com **AI Suggested Description**
- Gerar comentários de colunas com **AI generate**
- Observar permissões, lineage e insights

### Seção 2 — Lakeflow Designer
- Criar um fluxo visual com 4 prompts (Silver, Gold por país, Gold por atendente, qualidade)
- Os prompts já vêm prontos na célula de apoio, com catalog/schema dinâmicos
- Publicar e validar as tabelas criadas no Catalog

### Seção 3 — Metric View
- Criar a Metric View `mv_support_metrics` via Genie Code usando o prompt da célula de apoio
- Validar com prompts de consulta (1 dimensão + 1 medida → 3 dimensões + 3 medidas)

### Seção 4 — SQL Editor
- Executar consultas básicas e testar o assistente AI
- Executar AI Functions: `ai_analyze_sentiment`, `ai_query`
- Queries prontas na célula de apoio, com catalog/schema dinâmicos

### Seção 5 — Genie Agents
- **5.0** Criar a sala Genie manualmente com a Metric View + perguntas de validação
- **5.1** Melhorar a sala com o Genie Code (prompt na célula de apoio)
- **5.2** Adicionar um Volume como fonte (Knowledge Agent / RAG em Beta)

### Seção 6 — AI/BI Dashboard
- Criar um dashboard usando o prompt da célula de apoio
- Associar a sala Genie ao dashboard (Settings > Enable Genie > Link existing agent)
- Testar a análise conversacional diretamente no dashboard

### Seção 7 — Genie One
- Conhecer a interface simplificada para usuários consumidores
- Salas, dashboards, agente geral, tarefas agendadas e aba Discovery

---

## Checklist Pré-Workshop

- [ ] Repo clonado no workspace
- [ ] `00_Config` configurado com catalog e schema do ambiente
- [ ] `00_Setup_Workshop` executado com sucesso (4 tabelas bronze criadas)
- [ ] Compute Serverless disponível para os participantes
- [ ] AI Functions habilitadas no workspace
- [ ] Feature Preview "Analyze Files in Volumes with Genie Agents" habilitada (para seção 5.2)
- [ ] Volume com documentos de política de suporte criado no schema (para seção 5.2)

---

## Notas Importantes

- **Catalog/Schema configurável:** todas as células de prompt geram nomes totalmente qualificados a partir das variáveis `CATALOG` e `SCHEMA` do `00_Config`. Não há referências fixas a ajustar manualmente.
- **Dados reais:** os CSVs na pasta `data/` contêm dados exportados, não sintéticos. São versionados no git e importados pelo setup.
- **Convenção de nomenclatura:** tabelas bronze usam prefixo `tb_`; tabelas criadas no Designer usam prefixo `designer_`.
- **AI Functions:** requerem um endpoint de Foundation Model habilitado no workspace.
- **Lakeflow Designer:** pode requerer feature preview habilitada no workspace.
