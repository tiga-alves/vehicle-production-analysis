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



![image](https://github.com/user-attachments/assets/e3536451-c374-4ff7-93a5-31a9c7dee403)



### Análise Exploratória dos Dados


### Data Analysis



### Resultados/Descobertas



### Recomendações


### Limitações


### Referencias

1. SQL for Businesses by Werty.
2. [Stack Overflow](https://stack.com)
