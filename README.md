📌 Visão Geral

Este projeto tem como objetivo realizar uma análise exploratória e comparativa dos atributos base dos Temtem, utilizando dados públicos do jogo Temtem.

A análise busca identificar:

- Os Temtem com melhor desempenho geral

- Perfis ofensivos, defensivos e de velocidade

- A relação entre tipagem, atributos e Tier

- Destaques individuais, como Temtem com combinações excepcionais de status

O projeto contempla extração de dados, tratamento, modelagem analítica e visualização em dashboard.

🧠 Perguntas de Negócio

- Quais Temtem possuem os maiores atributos base?

- Ter dois tipos aumenta a chance de um Temtem estar em Tier alto?

- Existem “perfis” de atributos distintos por Tier?

- Qual Temtem apresenta a melhor combinação entre ataque e velocidade?

🗂️ Fonte dos Dados

Temtem Wiki (Fandom)
Dados públicos referentes aos atributos base das criaturas.

⚙️ Tecnologias Utilizadas

* Python

* Pandas

* NumPy

* Scikit-learn

* Power BI

* Git & GitHub

🔄 Pipeline do Projeto
1️⃣ Extração de Dados

Os dados foram extraídos diretamente da Temtem Wiki utilizando pandas.read_html, permitindo capturar tabelas estruturadas da página.

tables = pd.read_html(url)
df = tables[0]

2️⃣ Tratamento e Limpeza

Achatamento de colunas MultiIndex

Padronização de nomes de colunas

Conversão de tipos para numérico

Tratamento de valores ausentes

Remoção de linhas inválidas

df[stats_cols] = (
    df[stats_cols]
    .replace('-', np.nan)
    .apply(pd.to_numeric, errors='coerce')
    .fillna(0)
)

3️⃣ Criação de Métricas Analíticas

Foram criadas métricas derivadas para permitir análises mais ricas:

Offensive Power = ATK + SPATK

Defensive Power = HP + DEF + SPDEF

Speed Power = ATK + SPATK + SPD

4️⃣ Score Global e Normalização

Para evitar distorções por escala, foi aplicada normalização Min-Max antes da criação do score final.

scaler = MinMaxScaler()
df[['off_norm', 'def_norm', 'spd_norm']] = scaler.fit_transform(
    df[['offensive_power', 'defensive_power', 'spd']]
)


Score Global Normalizado:

35% Poder Ofensivo

35% Poder Defensivo

30% Velocidade

5️⃣ Tierização

Os Temtem foram classificados em tiers com base no score global normalizado:

S – Elite

A – Forte

B – Mediano

C – Abaixo da média

df['tier'] = pd.qcut(
    df['global_score_norm'],
    q=[0, 0.2, 0.5, 0.8, 1.0],
    labels=['C', 'B', 'A', 'S']
)

📊 Visualização no Power BI

O dashboard foi estruturado em 3 páginas:

🔹 Página 1 – Visão Geral

KPIs principais

Top 10 Temtem mais fortes

Top 10 por Dois Tipos

Distribuição por Tier

🔹 Página 2 – Comparações e Perfis

Percentual de Temtem com dois tipos por Tier

Perfis médios de atributos por Tier

Relação entre velocidade e poder ofensivo

🔹 Página 3 – Conclusões

Principais insights

Implicações estratégicas

Destaque da Análise(Oceara)

Créditos e referências

⭐ Destaque da Análise – Oceara

O Temtem Oceara apresentou:

Maior combinação entre velocidade e poder ofensivo

Tipo primário Water

Presença no Top 10 geral

Classificação Tier S

Este resultado indica um perfil altamente eficiente para estratégias ofensivas rápidas.


📁 Estrutura do Repositório
📦 temtem-analysis
 ┣ 📂 data
 ┃ ┗ temtem_base_stats.csv
 ┣ 📂 notebooks / scripts
 ┃ ┗ temtem-stats.py
 ┣ 📂 powerbi
 ┃ ┗ temtem_dashboard.pbix
 ┣ README.md


📌 Observações

Este projeto foi desenvolvido para fins educacionais e analíticos

Os dados utilizados são públicos

O modelo de score pode ser ajustado conforme diferentes estratégias de balanceamento

👤 Autor

Erik Andrey
Analista de Dados
Projeto desenvolvido para portfólio e prática em análise de dados.
