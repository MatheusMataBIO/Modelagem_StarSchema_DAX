## Modelagem Dimensional Star Schema 🎯 


### Objetivo 🎯 

Esse projeto tem como objetivo praticar minhas habilidades em modelagem de dados utilizando o Power query e o DAX do Power BI.

### Descrição

Utilizaremos a tabela única de Financial Sample para criar as tabelas dimensão e fato do nosso modelo baseado em star schema.
O processo consiste na criação das tabelas com base na tabela original. A partir da cópia serão selecionadas as colunas que irão compor a visão da nova tabela. Sendo assim, a partir da tabela principal serão criadas as tabelas: 

Financials_origem (modo oculto – backup)
D_Produtos (ID_produto, Produto, Média de Unidades Vendidas, Médias do valor de vendas, Mediana do valor de vendas, Valor máximo de Venda, Valor mínimo de Venda)
D_Produtos_Detalhes(ID_produtos, Discount Band, Sale Price,  Units Sold, Manufactoring Price)
D_Descontos (ID_produto, Discount, Discount Band)
D_Detalhes 
D_Calendário – Criada por DAX com calendar()
F_Vendas (SK_ID , ID_Produto, Produto, Units Sold, Sales Price, Discount  Band, Segment, Country, Salers, Profit, Date (campos))

### Etapas

### 1. Criando as tabelas

Abra o Power BI, vá até no menu superior e clique em transformar dados (vai abrir o Power Query) e no canto esquerdo clique com o botão direito em Financial e escolha a opção duplicar ele criará uma duplicação da tabela financial, toda vez que criar uma nova tabela use esse recurso para simplificar a criação. Fiz 5x pois foram 5 tabelas criadas e uma criado utilizando DAX. 


### 2. Modelando a tabela D_Produtos

1 - Renomear a tabela para D_Produtos
2 - Na tabela remover todas a colunas e deixar a coluna product, clicar nela e no menu superior clicar em agregar, selecionar a opção avançada.
3 - Nas opções Nome da nova coluna criar contagem, valor mínimo de venda, valor máximo de venda, média do valor de vendas, mediana do valor de vendas, média da manufatura
4 - Em operação colocar respectivamente Soma, Min, Máx, média, Mediana, Média.
5 - Em coluna colocar respectivamente Units Sold, Sale price, Sale price, Sale price, Sale price, Manufactoring Price.
6 - Clicar em OK
7 - Criar um índice indo em criar coluna índice e reposicionar como a primeira coluna


### 3.  Modelando a tabela D_Descontos

1 - Renomear a tabela para D_Descontos
2 - Remover todas as colunas exceto (Product, Discount, Discount Band)
3 - Adicionar ID para todos os produtos. Ir em coluna condicional, Nomear nova coluna com ID_Produto > Nome da coluna selecionar Product, Operador selecionar igual a valor colocar o nome do produto e na saída colocar 0. vai repetir esse processo para cada produto modificando apenas o valor que é o nome do produto e a saída que cada produto terá seu número.
4 - Clicar em OK
5 - Colocar ID_Produto como primeira coluna e depois pode remover Product pois já temos o ID_Produto para linkar com a tabela fato
6 - Selecione as 3 colunas e botão direito ir em remover duplicatas



### 4. Modelando a tabela Fato_Vendas

 1 - Renomear a tabela para Fato_Vendas
 2 - Rearrumar as colunas na ordem Produto, Units Sold, Sales Price, Discount  Band Segment, Country, Salers, Profit, Date (campos)) remover as colunas que não aparecem nessa rearrumação
 3 - Criar um índice (surrogate Key) que vai representar todas as instâncias com nome SK_ID
 4 - criar um ID utilizando as mesma informação da etapa 3 de "modelando a tabela D_Descontos"
 

### 5. Modelando a tabela D_Produtos_Detalhes

1 - Renomear a tabela para D_Produtos_Detalhes
2 - remover as colunas deixando somente Product, Discount Band, Units Solds, Manufactoring Price, Sale Price
3 - criar um ID utilizando as mesma informação da etapa 3 de "modelando a tabela D_Descontos"

### 6. Criando a tabela D_Datas com DAX e afunção CALENDAR

1 - No menu superior clicar em Nova medida
2 - Na barra de código colocar D_Datas = 
    ADDCOLUMNS(
        CALENDAR("2013-09-01", "2014-12-31"), // Ajuste as datas de início e fim conforme necessário
        "Ano", YEAR([Date]),
        "Mês", MONTH([Date]),
        "Nome do Mês", FORMAT([Date], "MMMM"),
        "Trimestre", QUARTER([Date])
    )

### 7. Modelando a tabela D_Detalhes

1 - Renomear a tabela para D_Detalhes
2 - Criar ID_Produto utilizando as mesma informação da etapa 3 de "modelando a tabela D_Descontos"

### 8. Aplicando as Alterações

1 - Fechar o Power Query e esperar aplicar as modificações

### 9. Criando os relacionamentos entre as Tabelas

1 - Na página principal do Power BI no canto esquerdo ir no ícone modelagem das tabelas
2 - Retirar todos os relacionamentos criados automaticamente e fazer manual 
3 - Para montar os relacionamentos cada tabela deve ter um campo em comum com a tabela fato
4 - Na tabela Financial_Origem ocultar ela clicando nos 3 pontos e "ocultar da visualização do relatório"
5 - Para criar o relacionamento clicar em gerenciar relações vai abrir o quadro com opções.
6 - Nessa modelagem a tabela D_Descontos e D_Detalhes possuem relacionamento muito para muitos as demais tem relacionamento 1 para muitos com a tabela Fato
