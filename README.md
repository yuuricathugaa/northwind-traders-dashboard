# 📊 Northwind Traders — Dashboard Estratégico Comercial

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Advanced-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Concluído-green?style=for-the-badge)

## 📌 Sobre o Projeto

Dashboard estratégico desenvolvido em Power BI Desktop com o dataset 
**Northwind Traders** (Kaggle), simulando uma análise comercial completa 
para o dono de uma distribuidora de alimentos.

O dashboard está __[aqui](https://github.com/yuuricathugaa/northwind-traders-dashboard/blob/main/dashboard_NorthwindTraders.pbix)__.

O projeto cobre toda a cadeia analítica: da modelagem de dados até o 
storytelling visual, com foco em decisões de negócio reais.

---

## 🖥️ Páginas do Dashboard

### 1. Visão Geral
![Visão Geral](assets/visao-geral.png)

### 2. Clientes & Mercados
![Clientes](assets/clientes.png)

### 3. Produtos & Categorias
![Produtos](assets/produtos.png)

### 4. Equipe de Vendas
![Equipe](assets/equipe-vendas.png)

---

## ❓ Perguntas de Negócio Respondidas

O dashboard foi construído para responder 4 perguntas estratégicas:

| # | Pergunta | Página |
|---|---|---|
| 1 | Como está a saúde financeira do negócio comparado ao ano anterior? | Visão Geral |
| 2 | Quais clientes concentram a maior parte da receita e onde estão geograficamente? | Clientes & Mercados |
| 3 | Quais categorias e produtos lideram as vendas e qual é a tendência de cada um? | Produtos & Categorias |
| 4 | Qual vendedor está performando melhor e qual está abaixo da média? | Equipe de Vendas |

---

## 🎨 Fundamentação de Design e UX (User Experience)

O design deste dashboard foi projetado sob os princípios de **Visual Business Intelligence**, priorizando a redução da carga cognitiva e a agilidade na tomada de decisão estratégica.

### 🏛️ Estilo Visual: Minimalismo Corporativo e Acessibilidade
Diferente de dashboards puramente artísticos, a interface segue o padrão **Clean/Light Mode**, inspirado em diretrizes globais de governança de dados e usabilidade:

*   **Paleta de Cores:** Utilização de uma escala monocromática em **Azul Profundo (#1B3A6B)** para transmitir autoridade e confiança. O contraste em fundo claro (**#F8F9FA**) garante legibilidade máxima em diferentes dispositivos e condições de iluminação (como projeções em salas de reunião).
*   **Tipografia:** Padronização com a família **Segoe UI**, garantindo nitidez e familiaridade visual com o ecossistema corporativo Microsoft 365.

### 📐 Arquitetura da Informação: Metodologia "Top-Down"
O layout respeita o padrão de leitura em "F" e "Z", guiando o olhar do gestor do macro para o micro de forma intuitiva:

1.  **Camada de Atenção (Summary):** KPIs consolidados posicionados para uma validação da saúde do negócio em menos de 5 segundos.
2.  **Camada de Contexto (Trends):** Gráficos de tendência e análise de Pareto (Top 5) que contextualizam as variações dos indicadores principais.
3.  **Camada de Detalhe (Granularity):** Matrizes com hierarquias expansíveis na base, permitindo o *drill-down* técnico sem poluir a visão executiva inicial.

### 🕹️ Navegação e Usabilidade (UI)
*   **Componentes Nativos:** Implementação do **Navegador de Páginas** nativo para emular a experiência de um software ERP/SaaS profissional, reduzindo a curva de aprendizado do usuário.
*   **Affordance & Consistência:** Elementos interativos seguem um padrão visual rígido, indicando claramente onde o usuário pode filtrar ou aprofundar a análise.
*   **Dicas de Ferramenta (Tooltips):** Uso de tooltips de contexto para fornecer "detalhamento sob demanda", mantendo o visual limpo enquanto oferece dados granulares extras.

---

## 📐 Modelagem de Dados

### Star Schema
O modelo segue o padrão Star Schema com separação clara entre 
tabelas fato e dimensão:

**Tabelas Fato:**
- `Fato_Vendas` — granularidade de pedido
- `Fato_Vendas_Detalhes` — granularidade de item de pedido

**Tabelas Dimensão:**
- `Dim_Clientes`
- `Dim_Produtos`
- `Dim_Categorias`
- `Dim_Funcionarios`
- `Dim_Transportadoras`
- `Dim_Calendario` — tabela de datas dedicada, marcada como 
  tabela de datas oficial do modelo

### Por que manter Fato_Vendas e Fato_Vendas_Detalhes separadas?
As duas tabelas possuem granularidades diferentes — uma linha em 
`Fato_Vendas` representa um pedido completo, enquanto uma linha em 
`Fato_Vendas_Detalhes` representa um produto dentro de um pedido. 
Mesclá-las causaria duplicação de dados e distorção em cálculos de 
frete e contagem de pedidos.

---

## 📊 Indicadores (KPIs) Criados

### KPIs Primários
| Medida | Descrição |
|---|---|
| Receita Total | Soma iterada via SUMX garantindo precisão linha a linha |
| Total de Pedidos | DISTINCTCOUNT de OrderID evitando dupla contagem |
| Ticket Médio | DIVIDE seguro entre Receita Total e Total de Pedidos |
| Total Clientes Ativos | Clientes com ao menos 1 pedido no período |

### KPIs de Inteligência Temporal
| Medida | Descrição |
|---|---|
| Faturamento MTD | Acumulado do mês filtrado via DATESMTD |
| Faturamento MTD LY | Mesmo período do ano anterior para comparação |
| Variação % MTD vs LY | Crescimento relativo com DIVIDE seguro |
| Crescimento Anual % | Variação anual via SAMEPERIODLASTYEAR |

### KPIs Avançados
| Medida | Descrição |
|---|---|
| % Participação Cliente | ALLSELECTED para respeitar filtros externos |
| % Participação Produto | Mesma lógica aplicada à dimensão de produtos |
| Ranking Clientes | RANKX com DENSE evitando saltos no ranking |
| Ranking Vendedores | Mesma lógica para dimensão de funcionários |

### KPIs Adicionais
| Medida | Descrição |
|---|---|
| Clientes Novos | Clientes que aparecem pela primeira vez no período selecionado |
| Top Categoria | Categoria com maior receita no período filtrado |
| Mês Pico | Mês com maior faturamento no período analisado |
| Label Crescimento | Rótulo dinâmico de texto para exibição visual do crescimento |
| Última Data | Âncora temporal baseada na última data do dataset |
| Total Produtos Ativos | Produtos com status ativo no catálogo |
| Total Produtos Descontinuados | Produtos retirados do catálogo |
| Variação MTD Formatada | Variação % formatada com ícone direcional ▲▼ |

---

## 🛠️ Tecnologias e Técnicas

- **Power BI Desktop** — desenvolvimento do dashboard
- **Power Query (M)** — ETL e limpeza dos dados
- **DAX Avançado** — 28 medidas incluindo Time Intelligence e KPIs dinâmicos
- **Star Schema** — modelagem dimensional
- **Formatação Condicional** — cor dinâmica por performance
- **Navegador de Páginas Nativo** — UX corporativa
