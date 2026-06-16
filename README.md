# Portfolio — Ivan Semchechem Lins
## Desenvolvedor de Software | Fullstack & Quantitativo

Por obrigações contratuais, NDAs e conformidade regulatória com empresas e investidores do mercado financeiro, a maior parte dos repositórios é privada. Este documento descreve o escopo real dos sistemas desenvolvidos e as tecnologias utilizadas em produção.

Estou disponível para apresentar arquiteturas, fluxos de dados e decisões de design em entrevista, sem expor código proprietário ou dados sensíveis.

---

## Projetos

### Backtest Pro — SaaS de Backtesting para Traders
Produto da alphaQuant que permite traders testarem estratégias de análise técnica com dados históricos reais, sem necessidade de programação.

**Repositório:** privado (projeto em sociedade)  
**Deploy:** [backtestpro.com.br](https://www.backtestpro.com.br/)

| Camada | Tecnologia |
|---|---|
| Frontend | HTML, CSS, JavaScript (single-file, Vercel) |
| Backend | Supabase (Postgres 17, Auth, Edge Functions em TypeScript) |
| Pagamentos | Mercado Pago (Pix + cartão) via Edge Function |
| Motor de Backtest | Python + MetaTrader 5 headless (pipeline de execução automática) |
| Deploy / CI | Vercel (deploy automático via push no GitHub) |

Destaques de arquitetura:
- EA universal parametrizado via JSON — zero geração de código em runtime
- 8 Edge Functions independentes: pagamento, webhook, suporte via chat, envio de e-mail, agendamento de backtest, verificação de assinaturas expiradas
- 7 migrations versionadas no Supabase (schema, segurança, fila de backtests, automação de e-mail, tickets de suporte, controle de roles)
- Autenticação com email e Google OAuth via Supabase Auth

---

### Clube da Lilica — E-commerce de Papelaria
E-commerce completo com loja pública e painel administrativo, desenvolvido para cliente final.

**Repositório:** [github.com/ivanslins/Clube-da-Lilica](https://github.com/ivanslins/Clube-da-Lilica) (privado)

| Camada | Tecnologia |
|---|---|
| Framework | Next.js 15 (App Router) |
| Linguagem | TypeScript |
| Estilização | Tailwind CSS |
| Banco de Dados | Supabase (PostgreSQL) |
| Autenticação | Supabase Auth |
| Pagamentos | Mercado Pago (Pix + cartão crédito/débito) |
| Imagens | Cloudinary |
| Deploy | Vercel |

Funcionalidades entregues:
- Loja pública: listagem com filtros, detalhe de produto, carrinho, checkout integrado
- Painel admin: CRUD de produtos, controle de estoque, upload de múltiplas imagens, gestão de categorias e pedidos
- Estoque em tempo real, design responsivo (mobile-first)

---

### Juridica AI — Assistente Jurídico Desktop
Aplicação desktop para análise e revisão de documentos jurídicos com auxílio de inteligência artificial, desenvolvida para cliente do setor jurídico.

**Repositório:** privado (cliente)

| Camada | Tecnologia |
|---|---|
| UI | React 18, Vite |
| Desktop | Electron 36 |
| Estilização | Tailwind CSS |
| Distribuição | Instalador NSIS (Windows), DMG (macOS) via electron-builder |

Funcionalidades:
- Carregamento e leitura de documentos do cliente
- Interface de revisão e histórico de análises
- Resumo de documentos via IA
- Distribuição como aplicativo desktop instalável (.exe / .dmg)

---

### Recebimento de Alugueis — Gestão Financeira Desktop
Aplicação desktop para emissão de recibos e controle de pagamentos de aluguel, desenvolvida para uso pessoal de cliente (Sr. Pires).

**Repositório:** privado (cliente)

| Camada | Tecnologia |
|---|---|
| UI | React 18, Vite |
| Desktop | Electron 31 |
| Banco de Dados | SQLite via sql.js (WASM) — 100% offline, sem servidor |
| Gráficos | Recharts |
| Estilização | Tailwind CSS |
| Distribuição | Instalador NSIS (Windows) via electron-builder |

Funcionalidades:
- Cadastro de imóveis e inquilinos
- Emissão de recibos com layout de impressão dedicado
- Dashboard com histórico de pagamentos e gráficos
- Banco de dados local (sem dependência de internet ou servidor externo)

---

### Fábrica de Estratégias — Agente de IA para Geração de Expert Advisors
Produto da alphaQuant que permite a qualquer trader criar um EA profissional descrevendo sua estratégia em linguagem natural, sem nenhum conhecimento de programação. Um agente de IA conduz a conversa, interpreta a estratégia, monta o código MQL5 combinando os módulos do framework e entrega o arquivo compilado diretamente na pasta do MetaTrader 5 do usuário — em minutos.

**Repositório:** privado (produto comercial)

| Componente | Descrição |
|---|---|
| Agente customizado | Instruído para atuar como especialista em estratégias de trading, conduz entrevista estruturada em português natural, valida entradas e gera o EA |
| Framework modular | 21 bibliotecas compiladas independentes (.ex5) cobertas por APIs documentadas (.mqh) |
| Templates parametrizados | `ModularEA_Template_v2` e `v3` — base que o agente preenche conforme a estratégia do cliente |
| APIs de integração | 21 arquivos `.mqh` documentando cada módulo: `OrderManagerAPI`, `RiskManagerAPI`, `TriggerSystemAPI`, etc. |
| Entrega automatizada | O agente salva o `.mq5` diretamente na pasta de Experts do MT5 do usuário via filesystem |

O EA entregue inclui automaticamente: gestão de risco completa (drawdown, exposição, horários), logging estruturado, recuperação de erros, trailing stop, OCO e todos os módulos relevantes à estratégia descrita.

---

### Framework de Expert Advisors — alphaQuant (MQL5)
Sistema modular de componentes para MetaTrader 5, base de todos os EAs de produção desenvolvidos para traders e fundos — e fundação técnica da Fábrica de Estratégias.

**Repositórios:** privados (NDA + segredo comercial)

Bibliotecas compiladas independentes com responsabilidade única:

| Módulo | Função |
|---|---|
| `OrderManager` | Envio, modificação e cancelamento de ordens |
| `PositionTracker` | Rastreamento de posições abertas |
| `RiskManager` | Travas automáticas: drawdown máximo, exposição por ativo |
| `StopLossManager` / `TakeProfitManager` | Gestão de stops com múltiplos critérios |
| `TrailingStopManager` | Trailing stop dinâmico |
| `TriggerSystem` / `TriggerMonitor` | Sistema de gatilhos por condições de mercado |
| `TrendDetector` / `MovingAverageSlope` | Análise de tendência e inclinação de médias |
| `BookAnalysis` / `TimeAndSalesReader` | Leitura de book de ofertas e fluxo de negócios |
| `OCOSystem` | Ordens condicionadas One-Cancels-the-Other |
| `Logger` | Logging estruturado para auditoria e diagnóstico |
| `DataExport` | Exportação de dados históricos |

---

### Sync-alphaQuant — Integração Python ↔ MT5
Sincronizador bidirecional entre Python e MetaTrader 5 para execução algorítmica e análise quantitativa em tempo real.

**Repositório:** [github.com/ivanslins/Sync-alphaQuant](https://github.com/ivanslins/Sync-alphaQuant) (privado)  
**Stack:** Python, MQL5, MQL4

---

### WFA-alphaQuant — Walk-Forward Analysis Automatizado
Pipeline em Python para otimização e validação de estratégias com WFA automatizado, integrado diretamente ao Strategy Tester do MT5.

**Repositório:** [github.com/ivanslins/WFA-alphaQuant](https://github.com/ivanslins/WFA-alphaQuant) (privado)  
**Stack:** Python (~2.4 MB de código), MQL5

---

## Stack Consolidada

**Linguagens:** Python, TypeScript, JavaScript, MQL5, MQL4, SQL  
**Frameworks / Runtimes:** React 18, Next.js 15, Electron, Vite, Node.js  
**Banco de Dados:** PostgreSQL (Supabase), SQLite (sql.js/WASM)  
**Cloud / Infra:** Supabase (Auth, Edge Functions, Migrations), Vercel (deploy, CI/CD), Cloudinary  
**Pagamentos:** Mercado Pago (Pix, cartão crédito/débito, webhooks)  
**Plataformas de Trading:** MetaTrader 5, MetaTrader 4  
**Distribuição Desktop:** Electron Builder, NSIS (Windows), DMG (macOS)  
**Ferramentas:** Git, GitHub, Tailwind CSS, Recharts

---

*Para discussões sobre arquitetura ou demonstrações que não infrinjam contratos vigentes, entre em contato: ivans.lins@gmail.com*
