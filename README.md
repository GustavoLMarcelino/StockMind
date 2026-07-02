# StockMind

**IA de Apoio à Decisão para Gestão de Estoque**

StockMind é um projeto de portfólio que aplica Inteligência Artificial à gestão de estoque, com foco em apoiar decisões estratégicas por meio da análise de dados históricos de movimentação, classificação de giro, previsão de demanda e sugestão priorizada de reposição de produtos.

O projeto é desenvolvido em parceria com um cliente externo real, a **Premium Baterias**, empresa de varejo especializada em baterias automotivas, para motos, caminhões e acessórios correlatos.

---

## Sobre o projeto

A Premium Baterias já mantém uma rotina operacional de controle de estoque, com registro de entradas e saídas. Esse controle, no entanto, é predominantemente reativo: mostra o que existe no estoque no momento, mas não oferece capacidade analítica para antecipar tendências, prever demanda ou sugerir reposição com base em comportamento histórico.

O StockMind nasce para preencher essa lacuna, adicionando uma camada de inteligência preditiva ao processo já existente — sem substituir o controle atual da empresa, mas elevando sua maturidade analítica.

O projeto também representa a aplicação integrada de conceitos de **Engenharia de Software**, **Ciência de Dados** e **Inteligência Artificial** a um cenário real de negócio, servindo como projeto de portfólio (RFC / TCC) do curso de Engenharia de Software da Católica SC.

---

## Objetivo

Desenvolver uma solução de inteligência artificial capaz de apoiar a gestão de estoque da Premium Baterias, analisando o histórico operacional da empresa para gerar previsões de demanda, classificação de giro, alertas de criticidade e sugestões de reposição priorizadas e justificadas.

---

## Público-alvo

- Gestor ou proprietário da loja
- Responsável por compras e abastecimento
- Usuários com conhecimento prático do negócio, sem necessidade de formação técnica em ciência de dados

---

## Principais funcionalidades

- Importação/consumo de histórico de movimentações de estoque
- Classificação automática de produtos por giro (alto, médio, baixo)
- Previsão de demanda futura por produto ou categoria
- Identificação de itens com risco de estoque crítico / ruptura
- Geração de lista priorizada de reposição, com quantidade estimada e justificativa
- Painel (dashboard) com indicadores de produtos críticos, giro, previsão e recomendações
- Comparação entre previsão anterior e consumo realizado
- Parametrização de estoque mínimo, horizonte de previsão e critérios de criticidade
- Histórico de previsões e recomendações emitidas

Veja a lista completa de requisitos funcionais e não funcionais no RFC (seções 2.3 e 2.4).

---

## Arquitetura

O sistema é dividido em quatro unidades principais (modelo C4 — Contexto, Containers e Componentes):

- **Interface Web** (React.js) — dashboards, previsões, produtos e recomendações
- **API Backend** (Node.js + Express) — regras de negócio, autenticação, orquestração entre frontend, banco de dados e IA
- **Módulo de IA** (Python) — classificação de giro, previsão de demanda e suporte à geração de recomendações
- **Banco de Dados** (PostgreSQL) — produtos, movimentações, usuários, previsões, recomendações e parâmetros

O frontend se comunica exclusivamente com a API; a API consulta os dados operacionais e aciona o módulo de IA quando necessário.

### Stack tecnológica

| Camada | Tecnologia |
|---|---|
| Frontend | React.js |
| Backend | Node.js + Express |
| IA / Análise preditiva | Python (pandas, scikit-learn, statsmodels) |
| Banco de dados | PostgreSQL |
| ORM | Prisma |
| Autenticação | JWT |
| Hospedagem | AWS |
| Diagramação técnica | Mermaid |

---

## Estratégia de Inteligência Artificial

O módulo de IA é desenvolvido de forma incremental, partindo de modelos simples e interpretáveis e evoluindo para técnicas mais robustas conforme a qualidade e o volume dos dados históricos permitirem:

1. Regras simples e médias móveis (linha de base)
2. Regressão linear simples e múltipla (previsão de demanda)
3. Regressão logística (classificação de risco de ruptura)
4. Árvore de decisão (classificação de giro e risco, com explicabilidade)
5. Random Forest (robustez com maior volume de dados)
6. MLP Regression (padrões não lineares de demanda)

A escolha do modelo final considera não apenas precisão, mas também interpretabilidade, custo computacional, facilidade de manutenção e aderência à realidade operacional da Premium Baterias.

Mais detalhes sobre variáveis de entrada, métricas de avaliação (MAE, RMSE, MAPE, acurácia, precisão, revocação) e critérios de escolha do modelo estão na seção 5.7 do RFC.

---

## Integração do Módulo de IA (MLOps)

O módulo de IA é exposto como serviço interno (FastAPI), acessado apenas pela API backend. A comunicação separa inferência online (síncrona, para o dashboard) de processamento em lote (assíncrono, para treinamento/re-treinamento).

Cada modelo treinado é registrado com versão, data, dados de origem e métricas, permitindo comparação **champion/challenger** antes de promover uma nova versão a produção, além de monitoramento contínuo para identificar degradação (*data drift*) comparando previsão x consumo real.

Mais detalhes na seção 5.6 do RFC.

---

## Segurança e Privacidade

- Autenticação via e-mail/senha com tokens JWT
- Controle de acesso por perfil de usuário (Gestor, Compras, Operacional, Administrador Técnico)
- Boas práticas alinhadas ao OWASP Top 10
- Conformidade com princípios básicos da LGPD (finalidade, necessidade, segurança e transparência)
- Backups automatizados e procedimento de recuperação documentado

Detalhes completos na seção 6 do RFC.

---

## Planejamento do projeto

O desenvolvimento segue 8 marcos ao longo de 12 semanas, do levantamento de requisitos até a documentação final e entrega do MVP. Veja a tabela completa de marcos na seção 7.1 do RFC.

### Governança de código e versionamento

- Repositório Git hospedado no GitHub
- Estratégia de branches: **GitHub Flow** (branch `main` sempre estável; desenvolvimento em `feature/...` via Pull Requests)
- Revisão de código obrigatória antes de qualquer merge
- **Conventional Commits** para histórico e changelog
- Versionamento **SemVer** (`MAJOR.MINOR.PATCH`), com prefixo `0.x.y` durante o desenvolvimento e `v1.0.0` reservado para o marco de entrega do MVP
- Rastreabilidade requisito–código: cada branch/PR referencia o requisito atendido (ex: RF03)
- Proteção de branch: testes e verificações automáticas precisam passar antes do merge

### Estratégia de testes

| Nível | Ferramenta | Objetivo |
|---|---|---|
| Unitário | Jest (Node/React), pytest (Python) | Validar funções e regras de negócio isoladamente |
| Integração | Supertest + banco de testes | Validar a API com a persistência de dados |
| Ponta a ponta | Cypress / Playwright | Validar fluxos críticos pela interface |
| Modelo de IA | pytest | Validar faixa/formato das saídas e evitar regressão de métricas |

### CI/CD

Pipeline automatizado (GitHub Actions) acionado a cada Pull Request e integração na `main`: lint/análise estática → testes automatizados → build (frontend, backend e imagem do serviço de IA) → deploy em homologação → aprovação manual → deploy em produção (AWS).

---

## MVP proposto

| Funcionalidade | Descrição no MVP |
|---|---|
| Importação de dados | Leitura de dados estruturados de produtos e movimentações |
| Dashboard | Indicadores principais: produtos monitorados, risco de ruptura, itens de alto giro |
| Classificação de giro | Classificação automática em alto, médio e baixo giro |
| Previsão de demanda | Estimativa de saída futura com base no histórico disponível |
| Recomendação de reposição | Lista priorizada com quantidade sugerida e justificativa |
| Histórico de previsões | Registro para comparação futura com o consumo real |

---

## Métricas de sucesso (KPIs)

| Indicador | Meta |
|---|---|
| MAPE da previsão | ≤ 20% |
| Tempo de resposta das consultas do painel | ≤ 500 ms |
| Cobertura dos produtos monitorados | 100% dos ativos elegíveis |
| Redução de rupturas em itens prioritários | 20%–30% |
| Satisfação do gestor com as recomendações | ≥ 8/10 |
| Taxa de aproveitamento das sugestões | ≥ 70% consideradas úteis |

---

## Estrutura do projeto

```text
StockMind/
├── data/              # Base de dados para análise e treinamento
├── notebooks/         # Exploração, tratamento e análise dos dados
├── models/            # Modelos preditivos treinados
├── src/               # Código-fonte principal da aplicação
├── docs/              # Documentação do projeto (RFC, diagramas, etc.)
└── README.md          # Documentação principal
```

---

## Documentação completa

O detalhamento completo do projeto — contexto e problema, personas, casos de uso, requisitos funcionais e não funcionais, wireframes, diagramas C4, modelo de dados, estratégia de IA, segurança, planejamento e riscos — está disponível no **RFC v1.3** em `docs/`.

---

## Status

Projeto em desenvolvimento, como trabalho de portfólio da disciplina de Engenharia de Software (Católica SC), em parceria com a Premium Baterias.
