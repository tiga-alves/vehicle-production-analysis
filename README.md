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

Para as fases de preparação inicial, foram performadas as seguintes tarefas:
1. Importação e inspeção dos dados.
2. Limpeza e transformação dos dados, removendo duplicatas, ajustando tipos de dados e combinando as diferentes fontes de dados.


Criei uma planilha nova no Excel e realizei a importação dos 3 fontes de dados:
Dados > Obter Dados > De Arquivo > Do Excel Pasta de Trabalho 

![image](https://github.com/user-attachments/assets/e5d81953-e43a-4adf-921a-8468b04faa08)


Selecionei o arquivo no computador e cliquei em Importar:
![image](https://github.com/user-attachments/assets/e123117b-0251-4bec-a8ee-d86fae213036)

Ao abrir a janela abaixo, nos temos um preview dos dados que estamos importando. Selecionei a tabela desejada e cliquei em Tranformar Dados para que o editor do Power Query fosse aberto:

![image](https://github.com/user-attachments/assets/8a72a55c-b202-40e2-8e98-c26e51f6b3dc)

Primeira coisa que fiz foi renomear o nome da tabela no canto direito do editor do Power Query, verificar as etapas já realizadas no Power Query de forma automática, sendo elas:
- Source e Navegação: que corresponde a conexão com a fontes de dados
- Cabeçalhos Promovidos: são os cabeçalhos que foram promovidos para cada uma das colunas
- Tipo Alterado: o Power Query automaticamente realiza a tipagem dos dados que estão sendo importados, cabe a nós vefiricarmos se a tipagem foi realizada da forma correta.

![image](https://github.com/user-attachments/assets/96718a81-937f-4888-b4f7-b17560b93951)


Selecionei a opção para que fosse criado o perfil de coluna com base em todo o dataset para ter visão geral de todos o dados, não das 1000 primeiras linhas apenas:

![image](https://github.com/user-attachments/assets/14899d57-ec4b-4748-adac-5c9e12dfc533)

Na aba Exibição, habilitei as opções:
- Ditribuição de Colunas: para vermos a distribuição dos dados
- Perfil da Coluna
- Qualidade da Coluna

![image](https://github.com/user-attachments/assets/1779bda0-6717-4e2d-aaf3-fe42abd24bc3)

Verifiquei a tipagem realizada pelo Power Query, e está correta:

```M
= Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{{"Source_File", type text}, {"Source_File_Date", type date}, {"Core Nameplate Region Mnemonic", Int64.Type}, {"Core Nameplate Plant Mnemonic", Int64.Type}, {"Mnemonic-Vehicle", Int64.Type}, {"Mnemonic-Vehicle/Plant", Int64.Type}, {"Mnemonic-Platform", Int64.Type}, {"Region", type text}, {"Market", type text}, {"Country/Territory", type text}, {"Production Plant", type text}, {"City", type text}, {"Plant State/Province", type text}, {"Source Plant", type text}, {"Source Plant Country/Territory", type text}, {"Source Plant Region", type text}, {"Design Parent", type text}, {"Engineering Group", type text}, {"Manufacturer Group", type text}, {"Manufacturer", type text}, {"Sales Parent", type text}, {"Production Brand", type text}, {"Platform Design Owner", type text}, {"Architecture", type text}, {"Platform", type text}, {"Program", type text}, {"Production Nameplate", type text}, {"SOP (Start of Production)", type date}, {"EOP (End of Production)", type date}, {"Lifecycle (Time)", Int64.Type}, {"Vehicle", type text}, {"Assembly Type", type text}, {"Strategic Group", type text}, {"Sales Group", type text}, {"Global Nameplate", type text}, {"Primary Design Center", type text}, {"Primary Design Country/Territory", type text}, {"Primary Design Region", type text}, {"Secondary Design Center", type text}, {"Secondary Design Country/Territory", type text}, {"Secondary Design Region", type text}, {"GVW Rating", type text}, {"GVW Class", type text}, {"Car/Truck", type text}, {"Production Type", type text}, {"Global Production Segment", type text}, {"Regional Sales Segment", type text}, {"Global Production Price Class", type text}, {"Global Sales Segment", type text}, {"Global Sales Sub-Segment", type text}, {"Global Sales Price Class", Int64.Type}, {"Short-Term Risk Rating", Int64.Type}, {"Long-Term Risk Rating", Int64.Type}, {"Region-2", type text}, {"Market-2", type text}, {"Country-2", type text}, {"Date_Ref", type date}, {"Monthly_Qty", Int64.Type}})
```

Removi as colunas Source_File e Source_File_Date, pois representam o nome do report e data de extração, o que não tem valor algum para a análise:
```m
= Table.RemoveColumns(#"Tipo Alterado",{"Source_File", "Source_File_Date"})
```

Adicionei uma coluna customizada chamada Vehicle Name, que representa a concatenação de outras duas colunas, Production Brand e Global Nameplate:
```m
= Table.AddColumn(#"Colunas Removidas", "Vehicle Name", each [Production Brand] & " " & [Global Nameplate])
```

![image](https://github.com/user-attachments/assets/fcc9eeec-be3f-4330-b396-fb78466d4ecd)


Adicionei uma coluna customizada chamada Status, que irá conter o valor "Produced" em todas a linhas:
```m
= Table.AddColumn(#"Personalização Adicionada", "Status", each "Produced", type text)
```

Para a tabela de veículos emplacados foi necessário realizar o tratamento nas colunas MODELO e QTDE:

![image](https://github.com/user-attachments/assets/07b9fe87-0ffc-4a6f-a564-87661e81aee3)

Para a coluna MODELO removi os valores vazios:

![image](https://github.com/user-attachments/assets/8e1c2abc-3dda-4ca3-bc5c-573eb625438b)


Para a coluna QTDE teve um problema na hora designar a tipagem para número inteiro, pois nessa coluna tem algumas linhas que quando não há quantidade está como "-", para isso fiz o replace de erros por 0, indicando que há um total de zero veículos emplacadados para a linha em específico:

![image](https://github.com/user-attachments/assets/acecbd63-7085-4ac8-84e6-c4491a56350a)

![image](https://github.com/user-attachments/assets/59524b4e-1458-4c81-a4ba-3949a9b442f7)


A coluna MÊS estava tipada como texto, mas precisei converter para o tipo data. Para isso, eu não pude simplemente mudar para data tendo em vista que o Power Query teve dificuldade em interpretar esse formato de dado. Foi necessário criar um código na linguagem M do Power Query e adicionar uma coluna personalizada, o código funcionaria da seguinte forma:
```M
= try
    (if [MÊS] = "janeiro/2020" then "01/2020" else
    if [MÊS] = "fevereiro/2020" then "02/2020" else
    if [MÊS] = "março/2020" then "03/2020" else null) otherwise null
```

Baseado na data contida na coluna MÊS, gerar um valor correspondente mas com o mês em formato numérico. Considerando que as datas nessa tabela vão desde janeiro/2020 até dezembro/2024, pedi para o llama3, que é uma LLM da Meta que roda localmente na minha máquina gerar o script completo:

![image](https://github.com/user-attachments/assets/24b2b77c-8206-4c60-b71d-f2d92dc18f95)

Tendo o script, fiz o seguintes passos:

Adicionar Coluna > Coluna Personalizada

![image](https://github.com/user-attachments/assets/2857c696-6f6f-4375-a85b-ab1fd7381045)

Dei um nome para a nova coluna, colei o código M, e por último cliquei em OK:

![image](https://github.com/user-attachments/assets/c3569014-0dfc-4d5d-a0e5-7e58fabf6eb2)

Depois disso, alterei de formato alfanumérico para data:
![image](https://github.com/user-attachments/assets/7c09e18d-d07e-4c05-b85d-f67ebbf4dcc9)


![image](https://github.com/user-attachments/assets/8fcd818b-a721-4154-a755-b90884f0360d)







![image](https://github.com/user-attachments/assets/e3536451-c374-4ff7-93a5-31a9c7dee403)



### Análise Exploratória dos Dados


### Data Analysis



### Resultados/Descobertas



### Recomendações


### Limitações


### Referencias

1. SQL for Businesses by Werty.
2. [Stack Overflow](https://stack.com)
