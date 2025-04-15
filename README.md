# Importando as bibliotecas necessárias
import pandas as pd

#Upload do arquivo manualmente (escolha do arquivo no Colab)
from google.colab import files
uploaded = files.upload()

#Carregar o arquivo CSV
df = pd.read_csv("salario_profissionais_dados.csv")
     
Upload widget is only available when the cell has been executed in the current browser session. Please rerun this cell to enable.
Saving salario_profissionais_dados.csv to salario_profissionais_dados (1).csv

# EXPLORAÇÃO INICIAL DA BASE DE DADOS

# Visualizar as 5 primeiras linhas
df.head()
     
work_year	country	region	experience_level	job_title	salary_in_usd	employee_residence	company_location	company_size	years_of_experience
0	2023	Spain	Europe	SE	Principal Data Scientist	85847	ES	ES	L	8
1	2023	United States of America	Americas	MI	ML Engineer	30000	US	US	S	5
2	2023	United States of America	Americas	MI	ML Engineer	25500	US	US	S	3
3	2023	Canada	Americas	SE	Data Scientist	175000	CA	CA	M	8
4	2023	Canada	Americas	SE	Data Scientist	120000	CA	CA	M	8

# Verificar número de linhas e colunas
df.shape

     
(3755, 10)

# Verificar tipos de dados e se há valores ausentes
df.info()

# Verificar valores ausentes por coluna
df.isnull().sum()
     
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3755 entries, 0 to 3754
Data columns (total 10 columns):
 #   Column               Non-Null Count  Dtype 
---  ------               --------------  ----- 
 0   work_year            3755 non-null   int64 
 1   country              3755 non-null   object
 2   region               3755 non-null   object
 3   experience_level     3755 non-null   object
 4   job_title            3755 non-null   object
 5   salary_in_usd        3755 non-null   int64 
 6   employee_residence   3755 non-null   object
 7   company_location     3755 non-null   object
 8   company_size         3755 non-null   object
 9   years_of_experience  3755 non-null   int64 
dtypes: int64(3), object(7)
memory usage: 293.5+ KB
0
work_year	0
country	0
region	0
experience_level	0
job_title	0
salary_in_usd	0
employee_residence	0
company_location	0
company_size	0
years_of_experience	0

dtype: int64

# Estatísticas descritivas iniciais (apenas colunas numéricas)
df.describe()
     
work_year	salary_in_usd	years_of_experience
count	3755.000000	3755.000000	3755.000000
mean	2022.373635	137570.389880	5.970972
std	0.691448	63055.625278	2.062673
min	2020.000000	5132.000000	1.000000
25%	2022.000000	95000.000000	5.000000
50%	2022.000000	135000.000000	6.000000
75%	2023.000000	175000.000000	8.000000
max	2023.000000	450000.000000	10.000000

# Verificar os 10 cargos mais comuns
print("Top 10 cargos mais comuns:")
print(df['job_title'].value_counts().head(10))
     
Top 10 cargos mais comuns:
job_title
Data Engineer                1040
Data Scientist                840
Data Analyst                  612
Machine Learning Engineer     289
Analytics Engineer            103
Data Architect                101
Research Scientist             82
Applied Scientist              58
Data Science Manager           58
Research Engineer              37
Name: count, dtype: int64

# Verificar a distribuição dos níveis de experiência
print("\nDistribuição dos níveis de experiência:")
print(df['experience_level'].value_counts())
     
Distribuição dos níveis de experiência:
experience_level
SE    2516
MI     805
EN     320
EX     114
Name: count, dtype: int64

 # Frequência relativa dos níveis de experiência (%)
print("\nDistribuição percentual dos níveis de experiência:")
print(df['experience_level'].value_counts(normalize=True) * 100)
     
Distribuição percentual dos níveis de experiência:
experience_level
SE    67.003995
MI    21.438083
EN     8.521971
EX     3.035952
Name: proportion, dtype: float64

# Verificar a distribuição do tamanho das empresas
print("\nDistribuição do tamanho das empresas:")
print(df['company_size'].value_counts())
     
Distribuição do tamanho das empresas:
company_size
M    3153
L     454
S     148
Name: count, dtype: int64

# Frequência relativa do tamanho das empresas (%)
print("\nDistribuição percentual do tamanho das empresas:")
print(df['company_size'].value_counts(normalize=True) * 100)
     
Distribuição percentual do tamanho das empresas:
company_size
M    83.968043
L    12.090546
S     3.941411
Name: proportion, dtype: float64

# ESTATÍSTICAS DESCRITIVAS
# Importando bibliotecas necessárias para visualizar gráficos
import matplotlib.pyplot as plt
import seaborn as sns

# Medidas de tendência central e dispersão
mean_salary = df['salary_in_usd'].mean()  # Média
median_salary = df['salary_in_usd'].median()  # Mediana
std_salary = df['salary_in_usd'].std()  # Desvio padrão
min_salary = df['salary_in_usd'].min()  # Mínimo
max_salary = df['salary_in_usd'].max()  # Máximo

# Exibir as medidas
print(f"Média do salário: {mean_salary}")
print(f"Mediana do salário: {median_salary}")
print(f"Desvio padrão do salário: {std_salary}")
print(f"Salário mínimo: {min_salary}")
print(f"Salário máximo: {max_salary}")
     
Média do salário: 137570.38988015978
Mediana do salário: 135000.0
Desvio padrão do salário: 63055.625278224084
Salário mínimo: 5132
Salário máximo: 450000

# Distribuição geral dos salários - Histograma
plt.figure(figsize=(10,6))
sns.histplot(df['salary_in_usd'], kde=True, bins=30, color='skyblue')
plt.title('Distribuição dos Salários em USD')
plt.xlabel('Salário Anual (USD)')
plt.ylabel('Frequência')
plt.show()

     


# Comparação de salários por nível de experiência (boxplot)
plt.figure(figsize=(10,6))
sns.boxplot(x='experience_level', y='salary_in_usd', data=df)
plt.title('Distribuição dos Salários por Nível de Experiência')
plt.xlabel('Nível de Experiência')
plt.ylabel('Salário Anual (USD)')
plt.show()
     


# Comparação de médias de salário por outros grupos (exemplo: Tamanho da Empresa)
mean_salary_by_company_size = df.groupby('company_size')['salary_in_usd'].mean()
print("\nMédia de Salário por Tamanho da Empresa:")
print(mean_salary_by_company_size)
     
Média de Salário por Tamanho da Empresa:
company_size
L    118300.982379
M    143130.548367
S     78226.682432
Name: salary_in_usd, dtype: float64

# Calcular a média salarial por país de residência
mean_salary_by_country = df.groupby('employee_residence')['salary_in_usd'].mean()

# Ordenar do maior para o menor salário médio
top_10_countries = mean_salary_by_country.sort_values(ascending=False).head(10)

# Exibir os 10 países com maiores salários médios
print("Top 10 países com maiores médias salariais (USD):")
print(top_10_countries)
     
Top 10 países com maiores médias salariais (USD):
employee_residence
IL    423834.000000
MY    200000.000000
PR    166000.000000
US    152822.011651
CA    132222.905882
CN    125404.000000
NZ    125000.000000
BA    120000.000000
IE    114943.428571
DO    110000.000000
Name: salary_in_usd, dtype: float64

# Visualizar com gráfico de barras
plt.figure(figsize=(12,6))
sns.barplot(x=top_10_countries.values, y=top_10_countries.index, palette='viridis')
plt.title('Top 10 Países com Maiores Médias Salariais (USD)')
plt.xlabel('Salário Médio (USD)')
plt.ylabel('País de Residência')
plt.show()
     
<ipython-input-26-802fa25afdcb>:3: FutureWarning: 

Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `y` variable to `hue` and set `legend=False` for the same effect.

  sns.barplot(x=top_10_countries.values, y=top_10_countries.index, palette='viridis')


# Mapear níveis de experiência para valores numéricos
experience_map = {'EN': 0, 'MI': 1, 'SE': 2, 'EX': 3}
df['experience_numeric'] = df['experience_level'].map(experience_map)

# Selecionar colunas numéricas para correlação
corr_matrix = df[['salary_in_usd', 'work_year', 'experience_numeric']].corr()

# Exibir a matriz
print("Matriz de Correlação:")
print(corr_matrix)

     
Matriz de Correlação:
                    salary_in_usd  work_year  experience_numeric
salary_in_usd            1.000000   0.228290            0.441668
work_year                0.228290   1.000000            0.194987
experience_numeric       0.441668   0.194987            1.000000

# Heatmap para visualização
plt.figure(figsize=(8, 5))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', fmt=".2f")
plt.title('Correlação entre Salário, Ano e Experiência')
plt.show()
     


# Regressão: Salário ao longo dos anos
plt.figure(figsize=(10, 5))
sns.regplot(data=df, x='work_year', y='salary_in_usd', scatter_kws={'alpha':0.3}, line_kws={'color':'red'})
plt.title('Tendência Salarial ao Longo dos Anos')
plt.xlabel('Ano')
plt.ylabel('Salário em USD')
plt.show()
     


# Regressão: Salário vs Nível de Experiência
plt.figure(figsize=(10, 5))
sns.regplot(data=df, x='experience_numeric', y='salary_in_usd', scatter_kws={'alpha':0.3}, line_kws={'color':'green'})
plt.title('Salário vs Nível de Experiência')
plt.xlabel('Nível de Experiência (0=EN, 3=EX)')
plt.ylabel('Salário em USD')
plt.show()

     




