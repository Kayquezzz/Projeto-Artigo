# Projeto-Artigo
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Projeto Artigo Taxa de Mortalidade Infantil no Brasil**

 ***Resumo***

Este projeto investiga como fatores socioeconômicos — especialmente a renda média domiciliar — se relacionam com as taxas de mortalidade infantil no Brasil entre 2019 e 2024. A análise parte da hipótese de que regiões com menor renda tendem a registrar maior mortalidade devido à maior vulnerabilidade social.
Os resultados indicam uma correlação negativa, ou seja, locais com renda mais alta geralmente apresentam menores índices de mortalidade infantil.

 ***Introdução***

O Brasil é marcado por fortes desigualdades sociais que afetam diretamente condições essenciais como moradia, saneamento e acesso à saúde. Áreas com maior renda tendem a possuir melhores indicadores de saúde e menores taxas de mortalidade infantil.
Entre 2019 e 2024, fatores como a pandemia de COVID-19 agravaram essas disparidades, impactando principalmente regiões já vulneráveis.
Este estudo utiliza dados oficiais de renda e mortalidade para compreender como essas variáveis se relacionaram ao longo do período analisado e como isso pode orientar políticas públicas.

***Metodologia***
Bases de dados utilizadas

Mortalidade infantil (2019–2024): registros anuais por estado.
Renda média domiciliar (2019–2024): dados por unidade federativa.
Processos aplicados
Padronização dos nomes de estados e remoção de caracteres especiais.
Limpeza e transformação de valores numéricos (renda e mortalidade).
Cálculo da correlação de Pearson para medir a relação entre renda e mortalidade.
Criação de gráficos de dispersão e comparações visuais entre os estados.
Observação: a análise mostra relações, mas não determina causa e efeito.

***Resultados***

A união dos dados permitiu gerar um painel consistente de renda média e mortalidade infantil por estado. A análise mostrou que:
Há uma tendência geral de relação negativa: quanto maior a renda, menor a mortalidade infantil.
A intensidade dessa relação variou entre os anos.
Durante a pandemia (2020–2021), houve aumento significativo nas taxas de mortalidade, principalmente nas regiões de menor renda.

***Gráficos e observações***

Estados como Paraná, Maranhão e Ceará apresentaram maiores números de mortes infantis, enquanto Acre, Amapá e Rondônia registraram os menores valores.

A comparação direta com a renda per capita mostrou que a mortalidade infantil não depende apenas da renda — fatores como acesso a serviços de saúde, saneamento e infraestrutura também influenciam fortemente.

***Interpretação geral***

Embora exista uma tendência de que regiões com maior renda tenham menor mortalidade infantil, os resultados indicam que a renda não explica totalmente as diferenças entre os estados. Em alguns casos, a renda influenciou, mas em outros, a relação foi fraca ou inexistente — reforçando que a mortalidade infantil é um fenômeno multifatorial.


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Codigo do Projeto com as explicações de cada passo a passo**


*Esse código baixa uma planilha oficial do IBGE que contém as projeções da população brasileira e, a partir dela, seleciona apenas os dados de crianças e adolescentes (idades de 0 a 14 anos) dos 27 estados do Brasil.*

*Depois disso, ele soma a população dessa faixa etária em cada estado, organiza os estados do maior para o menor número de crianças e mostra o resultado final.*
Por fim, o código também calcula a soma total dessa população e a média entre os estados.

*import pandas as pd
from io import BytesIO
import requests*

*url = "https://ftp.ibge.gov.br/Projecao_da_Populacao/Projecao_da_Populacao_2024/projecoes_2024_tab1_idade_simples.xlsx"*

*# Baixar e carregar planilha com cabeçalho correto
r = requests.get(url)
xls = pd.ExcelFile(BytesIO(r.content))
df = pd.read_excel(xls, sheet_name="1) POP_IDADE SIMPLES", header=5)*

*# Filtrar apenas "Ambos os sexos"
df = df[df["SEXO"] == "Ambos"]*

*# Filtrar idades 0 a 14
df = df[df["IDADE"].between(0, 14)]*

*# Lista das 27 siglas oficiais dos estados
ufs = [
    "AC","AL","AP","AM","BA","CE","DF","ES","GO","MA","MT","MS","MG",
    "PA","PB","PR","PE","PI","RJ","RN","RS","RO","RR","SC","SP","SE","TO"
]*

*# Selecionar apenas linhas dos estados
df = df[df["SIGLA"].isin(ufs)]*

*# Selecionar apenas ano 2019
df_2019 = df[["SIGLA", "LOCAL", "IDADE", 2019]]*

*# Somar população 0 a 14 por estado
pop_0_14_2019 = df_2019.groupby(["SIGLA", "LOCAL"])[2019].sum().reset_index()*

*# Ordenar do maior para o menor
pop_0_14_2019 = pop_0_14_2019.sort_values(by=2019, ascending=False)*

*# Exibir resultado final
print("População de 0 a 14 anos por estado (2019):")
display(pop_0_14_2019)*

*# Soma total e média apenas dos 27 estados
soma_total_2019 = pop_0_14_2019[2019].sum()
media_estados_2019 = pop_0_14_2019[2019].mean()*

*print("\nSoma total (27 estados, 0-14 anos, 2019):", f"{soma_total_2019:,}".replace(",", "."))
print("Média por estado:", f"{media_estados_2019:,.0f}".replace(",", "."))*




