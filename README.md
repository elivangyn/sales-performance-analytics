📊 Executive Sales \& Performance Dashboard
📌 Contexto de Negócio \& Desafio

Uma empresa do setor varejista enfrentava dificuldades no acompanhamento consolidado do seu histórico de vendas (2017 a 2019), com dados dispersos em diferentes bases anuais e falta de visibilidade sobre:

Performance e Atingimento de Metas Anuais.

Volatilidade de Faturamento e Crescimento Year-over-Year (YoY).

Impacto Financeiro de Vendas Canceladas (Inadimplência/Loss).

Representatividade dos Meios de Pagamento na Receita Líquida.

Objetivo da Solução: Desenvolver uma arquitetura de dados e um painel executivo e interativo para suportar a tomada de decisão da diretoria comercial e gerência de operações.

🏗️ Arquitetura da Solução \& Modelagem

\[Bases de Vendas 2017, 2018, 2019]
│
▼
\[Power Query / ETL (M)] ──► Clean, Append \& Normalização
│
▼
\[Modelo Dimensional Schema] ──► Star Schema (dCalendario + fVendas)
│
▼
\[Camada DAX] ──► Métricas de Negócio \& Time Intelligence
│
▼
\[Visualização UX/UI Executive]



1. ETL e Engenharia de Dados (Power Query / M)

Consolidação de Bases: Unificação via Append de 3 fontes distintas mantendo a consistência dos tipos de dados e minimizando carga de processamento.

Tabela Calendário Automática (dCalendario): Construída via script em M para garantir granularidade diária, suporte à navegação temporal e inteligência de datas.

Limpeza e Otimização: Tratamento de valores omissos/nulos, remoção de colunas redundantes e conversão otimizada de tipos para alta performance no engine VertiPaq.

2. Modelagem de Dados (Star Schema)

O modelo foi estruturado no padrão Star Schema (Esquema Estrela), garantindo auto-desempenho nas consultas e facilidade de manutenção:

Fato: fVendas (Transacional)

Dimensões: dCalendario, dVendedor, dFormaPagamento

Relacionamentos: Direcionamento único (1:N), garantindo integridade referencial sem ambiguidades.

📐 Camada Analítica: Principais Métricas em DAX

Para fornecer métricas acionáveis, foram desenvolvidas medidas utilizando DAX otimizado:

1. Faturamento Líquido (Excluindo Cancelamentos)

Faturamento Liquido =
CALCULATE(
SUM(fVendas\[ValorVenda]),
fVendas\[Status] <> "Cancelado"
)



2. Taxa de Cancelamento (%)

Taxa Cancelamento % =
DIVIDE(
CALCULATE(SUM(fVendas\[ValorVenda]), fVendas\[Status] = "Cancelado"),
SUM(fVendas\[ValorVenda]),
0
)



3. Faturamento do Ano Anterior (Time Intelligence)

Faturamento SPLY =
CALCULATE(
\[Faturamento Liquido],
SAMEPERIODLASTYEAR('dCalendario'\[Data])
)



4. Variacao YoY (%)

Crescimento YoY % =
VAR \_Atual = \[Faturamento Liquido]
VAR \_Anterior = \[Faturamento SPLY]
RETURN
DIVIDE(\_Atual - \_Anterior, \_Anterior, 0)



🖼️ Painel Executivo \& User Experience (UX/UI)

1. Visão Geral do Dashboard

Design Dark Mode High-Contrast: Desenvolvido para reduzir a fadiga visual e priorizar os KPIs críticos via hierarquia de cores (Neon Accent).

Navegação Dinâmica: Acompanhamento de metas anuais, evolução temporal e distribuição de faturamento por método de pagamento.

2. Painel Pop-up de Filtros (Navegação Avançada via Bookmarks)

Menu retrátil que preserva o real estate do relatório sem poluir a visão do usuário executivo, permitindo cruzamento rápido por Vendedor, Período e Forma de Pagamento.

💡 Key Business Insights (Resultados e Análise de Negócio)

Atingimento de Metas: Identificou-se que o ano de 2018 apresentou o maior desvio positivo frente à meta planejada, puxado pelo aumento das transações via Cartão de Crédito.

Representatividade de Meios de Pagamento: Cartão de Crédito e Débito representam mais de 75% da receita, sinalizando a necessidade de renegociação de taxas operacionais junto aos adquirentes.

Análise de Inadimplência/Cancelamento: O percentual de vendas canceladas manteve-se sob controle, porém concentrado em períodos específicos do mês, sugerindo falha na validação do antifraude em dias de pico.

🛠️ Competências Demonstradas Neste Projeto

Analítica \& Negócio: Tradução de necessidades comerciais em KPIs estratégicos.

Modelagem Dimensional: Star Schema, tratamento de relacionamentos N:1 e otimização de modelo.

Linguagem DAX Avançada: Contexto de Avaliação (Filter/Row Context), Transição de Contexto e Inteligência Temporal.

Linguagem M / ETL: Transformação e tratamento de dados brutos para consumo estruturado.

Design focado no Usuário (DataViz \& UI/UX): Layout intuitivo, storytelling visual e navegação via botões/bookmarks.

👤 Sobre o Autor

Elivã M. Macedo - Instrutor SENAI/SENAC - Logistica e Negócios - Analise e Engenharia de Dados.

elivann.macedo@gmail.com



