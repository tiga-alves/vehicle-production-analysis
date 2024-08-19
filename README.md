# Análise de Produção de Automóveis

## Sumário

- [Objetivo](#objetivo)
- [Dados Fornecidos](#dados-fornecidos)
- [Ferramentas](#ferramentas)
- [Preparação Inicial](#preparacao-inicial)
- 

### Objetivo

Criar um dashboard utilizando Excel e Power Query, além de manipular e transformar dados.

### Dados Fornecidos

Conjuntos de dados sobre produção de veículos, incluindo modelos e características de veículos e quantidades. O conjunto de dados está disposto de acordo com as etapas do ciclo de vida de um veículo, desde o planejamento até o registro para a utilização final.

- producted_vehicles.xlsx: Arquivo contendo informações detalhadas sobre veículos que já foram fabricados e estão prontos para serem vendidos.
- registered_vehicles.xlsx: Arquivo contendo informações detalhadas sobre veículos que já foram vendidos e registrados para a circulação nas vias públicas.
- vehicles_planned_production.xlsx: Arquivo contendo informações detalhadas sobre veículos que estão na fase de planejamento e preparação.

### Ferramentas

- Excel - Construção do Dashboard, tratamento e manipulação dos dados.
  - [Download](https://microsoft.com)

### Preparação Inicial

Para as fases de preparação inicial, foram realizadas as seguintes tarefas:

1. Importação e inspeção dos dados: Adquirir e revisar os dados para garantir sua integridade e relevância.
2. Limpeza e transformação dos dados: Preparar os dados para análise, eliminando inconsistências e aplicando as transformações necessárias.

### Processo de importação dos dados
Criei uma nova planilha no Excel e realizei a importação das três fontes de dados da seguinte maneira:
Naveguei até Dados > Obter Dados > De Arquivo > Do Excel Pasta de Trabalho.

![image](https://github.com/user-attachments/assets/e5d81953-e43a-4adf-921a-8468b04faa08)


Selecionei o arquivo no computador e cliquei em Importar:
![image](https://github.com/user-attachments/assets/e123117b-0251-4bec-a8ee-d86fae213036)

Ao abrir a janela abaixo, nos temos um preview dos dados que estamos importando. Selecionei a tabela desejada e cliquei em Tranformar Dados para que o editor do Power Query fosse aberto:

![image](https://github.com/user-attachments/assets/8a72a55c-b202-40e2-8e98-c26e51f6b3dc)

A primeira coisa que fiz foi renomear a tabela no canto direito do editor do Power Query e verificar as etapas realizadas automaticamente pelo Power Query:

- Source e Navegação: corresponde à conexão com as fontes de dados, garantindo que os dados sejam importados corretamente.
- Cabeçalhos Promovidos: os cabeçalhos foram promovidos para cada uma das colunas, definindo quais linhas representam os títulos das colunas.
- Tipo Alterado: o Power Query realizou automaticamente a tipagem dos dados importados. Cabe a nós verificar se a tipagem foi realizada corretamente, assegurando que cada coluna esteja no tipo de dado adequado (texto, número, data, etc.).

![image](https://github.com/user-attachments/assets/96718a81-937f-4888-b4f7-b17560b93951)


Selecionei a opção para que fosse criado o perfil de coluna com base em todo o dataset para ter visão geral de todos o dados, não das 1000 primeiras linhas apenas:

![image](https://github.com/user-attachments/assets/5d6e1edc-4c61-4ab9-9508-dc857941e924)


Na aba Exibição, habilitei as seguintes opções:

- Distribuição de Colunas: para visualizar a distribuição dos dados em cada coluna, identificando a frequência de valores distintos. Isso ajuda a entender a variabilidade dos dados e detectar possíveis anomalias.
- Perfil da Coluna: para obter um resumo estatístico de cada coluna, incluindo contagem de valores distintos, valor mínimo, valor máximo, média, etc. Isso é essencial para ter uma visão geral da estrutura dos dados e identificar tendências ou outliers.
- Qualidade da Coluna: para monitorar a qualidade dos dados, indicando a porcentagem de valores válidos, valores vazios e erros. Isso é crucial para garantir a integridade dos dados e identificar possíveis problemas de qualidade que precisam ser corrigidos.

![image](https://github.com/user-attachments/assets/1779bda0-6717-4e2d-aaf3-fe42abd24bc3)


### Tabela de veículos produzidos
Verifiquei a tipagem realizada pelo Power Query, e está correta:

```M
= Table.TransformColumnTypes(#"Etapa Anterior",{{"Source_File", type text}, {"Source_File_Date", type date}, {"Core Nameplate Region Mnemonic", Int64.Type}, {"Core Nameplate Plant Mnemonic", Int64.Type}, {"Mnemonic-Vehicle", Int64.Type}, {"Mnemonic-Vehicle/Plant", Int64.Type}, {"Mnemonic-Platform", Int64.Type}, {"Region", type text}, {"Market", type text}, {"Country/Territory", type text}, {"Production Plant", type text}, {"City", type text}, {"Plant State/Province", type text}, {"Source Plant", type text}, {"Source Plant Country/Territory", type text}, {"Source Plant Region", type text}, {"Design Parent", type text}, {"Engineering Group", type text}, {"Manufacturer Group", type text}, {"Manufacturer", type text}, {"Sales Parent", type text}, {"Production Brand", type text}, {"Platform Design Owner", type text}, {"Architecture", type text}, {"Platform", type text}, {"Program", type text}, {"Production Nameplate", type text}, {"SOP (Start of Production)", type date}, {"EOP (End of Production)", type date}, {"Lifecycle (Time)", Int64.Type}, {"Vehicle", type text}, {"Assembly Type", type text}, {"Strategic Group", type text}, {"Sales Group", type text}, {"Global Nameplate", type text}, {"Primary Design Center", type text}, {"Primary Design Country/Territory", type text}, {"Primary Design Region", type text}, {"Secondary Design Center", type text}, {"Secondary Design Country/Territory", type text}, {"Secondary Design Region", type text}, {"GVW Rating", type text}, {"GVW Class", type text}, {"Car/Truck", type text}, {"Production Type", type text}, {"Global Production Segment", type text}, {"Regional Sales Segment", type text}, {"Global Production Price Class", type text}, {"Global Sales Segment", type text}, {"Global Sales Sub-Segment", type text}, {"Global Sales Price Class", Int64.Type}, {"Short-Term Risk Rating", Int64.Type}, {"Long-Term Risk Rating", Int64.Type}, {"Region-2", type text}, {"Market-2", type text}, {"Country-2", type text}, {"Date_Ref", type date}, {"Monthly_Qty", Int64.Type}})
```

Removi as colunas Source_File e Source_File_Date, pois representam o nome do report e data de extração, o que não tem valor algum para a análise:
```m
= Table.RemoveColumns(#"Etapa Anterior",{"Source_File", "Source_File_Date"})
```

Adicionei uma coluna customizada chamada Vehicle Name, que representa a concatenação de outras duas colunas, Production Brand e Global Nameplate:
```m
= Table.AddColumn(#"Etapa Anterior", "Vehicle Name", each [Production Brand] & " " & [Global Nameplate])
```

![image](https://github.com/user-attachments/assets/fcc9eeec-be3f-4330-b396-fb78466d4ecd)


Adicionei uma coluna customizada chamada Status, que irá conter o valor "Produced" em todas a linhas:
```m
= Table.AddColumn(#"Etapa Anterior", "Status", each "Produced", type text)
```

Resumo das etapas aplicadas:

![image](https://github.com/user-attachments/assets/650dc7b7-1179-4f93-9d8b-e094b4cb9775)

### Tabela de veículos emplacados

Substituição de erros na coluna QTDE: A presença de "-" na coluna QTDE impede a conversão direta para número inteiro. Substituir "-" por 0 resolve o problema, permitindo a tipagem correta e garantindo que a análise subsequente seja precisa.

![image](https://github.com/user-attachments/assets/acecbd63-7085-4ac8-84e6-c4491a56350a)

![image](https://github.com/user-attachments/assets/59524b4e-1458-4c81-a4ba-3949a9b442f7)


Conversão da coluna MÊS para tipo data: A coluna MÊS estava em formato de texto, dificultando a conversão direta para data. Criei um código na linguagem M e adicionar uma coluna personalizada permitiu transformar o formato textual dos meses para um formato numérico, facilitando a conversão para tipo data. Essa abordagem garante que os dados sejam interpretados corretamente nas análises temporais.
```M
= try
    (if [MÊS] = "janeiro/2020" then "01/2020" else
    if [MÊS] = "fevereiro/2020" then "02/2020" else
    ... 
    if [MÊS] = "março/2020" then "03/2020" else null) otherwise null
```

Esse código gera um valor correspondente ao mês em formato numérico com base na data contida na coluna MÊS. Com o script pronto, segui os seguintes passos:

Adicionar Coluna > Coluna Personalizada

![image](https://github.com/user-attachments/assets/2857c696-6f6f-4375-a85b-ab1fd7381045)

Dei um nome para a nova coluna, colei o código M, e cliquei em OK:

![image](https://github.com/user-attachments/assets/c3569014-0dfc-4d5d-a0e5-7e58fabf6eb2)

Depois disso, alterei o formato alfanumérico para data:


![image](https://github.com/user-attachments/assets/7c09e18d-d07e-4c05-b85d-f67ebbf4dcc9)


Colunas foram renomeadas:
```m
= Table.RenameColumns(#"Etapa Anterior",{{"FABRICA", "Manufacturer"}, {"MODELO", "Global Nameplate"}, {"QTDE", "Quantity"}, {"MÊS", "Date"}})
```

Motivo: Renomear as colunas para nomes mais descritivos e padronizados facilita a compreensão e análise dos dados. Nomes claros e consistentes são essenciais para uma análise eficiente e precisa.

Para a coluna Global Nameplate, substituí os valores vazios por "-", pois quero manter essas linhas para analisar a quantidade de veículos emplacados por fabricante:
```m
= Table.ReplaceValue(#"Etapa Anterior",null,"-",Replacer.ReplaceValue,{"Global Nameplate"})
```

Motivo: Substituir valores vazios por "-" na coluna Global Nameplate garante que todas as linhas sejam mantidas na análise, mesmo aquelas que não possuem um nome de modelo específico. Isso é importante para analisar a quantidade de veículos emplacados por fabricante de maneira abrangente, sem perder informações devido a valores ausentes.

Resumo das etapas aplicadas:

![image](https://github.com/user-attachments/assets/a280f7c9-a311-41f1-a0bc-9dc888e2643c)


### Tabela de veículos a serem produzidos

Na coluna customizada Status eu preenchi as linhas com a identificação de "Planned":

```m
= Table.AddColumn(#"Etapa Anterior", "Status", each "Planned", type text)
```

Substituição na coluna Monthly_: A presença de "-" na coluna Monthly_Qty impede a conversão direta para número inteiro. Substituir "-" por 0 resolve o problema, permitindo a tipagem correta e garantindo que a análise subsequente seja precisa.

```m
= Table.ReplaceValue(#"Etapa Anterior","-",0,Replacer.ReplaceValue,{"Monthly_Qty"})
```

Reordenei as colunas para que estejam na mesma ordem da tabela de veículos já produzidos. Isso é crucial para a criação de uma tabela apendada, permitindo uma análise conjunta dos dados de veículos planejados e já produzidos.

Resumo das etapas aplicadas:

![image](https://github.com/user-attachments/assets/9d387ea9-a969-4b1f-90ca-280cb31f38d4)


### Tabela calendário


### Análise Exploratória dos Dados


### Data Analysis



### Resultados/Descobertas



### Recomendações


### Limitações


### Referencias

1. SQL for Businesses by Werty.
2. [Stack Overflow](https://stack.com)
