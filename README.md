# Dio_financial
Repositorio para desenvolver o Desario financial em Power BI dentro do bootcampo Suzano - Análise de dados
## ARQUIVOS:
* **Financial Sample.xlsx** - arquivo com os dados brutos a serem analisados pelo Power BI. Obtido nos dados de amostra dentro do próprio Power BI.
* **financial.pbix** - arquivo Power BI com a implementação dos relatorios solicitados no desafio.
* **financial.pdf** - relatorio gerado no formato pdf
<br>
* URL da My workspace onde foi publicado  https://app.powerbi.com/groups/me/lineage?experience=power-bi
![image](https://github.com/user-attachments/assets/52ab7043-02e7-4b5a-9a76-fb807ce36c5d)
* URL do link exportado para o powerpoint para a versao dinâmica do relatorio
https://app.powerbi.com/groups/me/reports/5cc502d1-e599-43b4-b2c9-0ad45fd6b020/947f605053af41eb7164?bookmarkGuid=cf8c2e24-56bd-4f4a-8cc0-8a9a6656a86f&bookmarkUsage=1&ctid=eadfe06c-a575-4c81-b28a-e282479faa9c&portalSessionId=c64fc881-04f8-43c1-89b3-8367df48c9c0&fromEntryPoint=export

# Branch = Dio_Relatorio de vendas da empresa Fantasia
Criamos a branch **Desafio-Relatorio-de-vendas-empresa-Fantasia**, para o novo desafios. Foi usado o mesmo arquivo de dados *Financial Sample.xlsx* para gerar o novo relatorio. mas neste desafio temos novidades como formatação do background onde serão incluidos os visuais, inclusão de botões de ação para mudança de páginas, limpeza de segmentação e chaveamento de visuais que ocupam o mesmo espaço, alem da inclusão de novos visuais (de terceiros e não defaults) como o chiclet e o radar que foram adicionados para aprendermos como trazer e trabalhar com esses visuais.
## Arquivos
* **Desafio---Relatorio-de-vendas-empresa-Fantasia.pbix** - arquivo Power BI com a implementação
* **logo_dio.png** - imagem da logo da DIO
* **clean blue.png** - imagem de uma borracha (usada para limpar as segmentações)
* **Desafio - Relatorio de vendas empresa Fantasia.pdf** - relatorio gerado, salvo no formato PDF.
<br>
* URL da My Workspace onde foi publicado https://app.powerbi.com/groups/me/reports/767013a8-4830-4b30-b44f-6b915e501e49/657ef02cf3293e13cbb6?experience=power-bi&bookmarkGuid=38de32dedb86c4a355cf

# Branch - Modelo baseado schema star
Criamos essa nova branch, partindo também de branch main. Usamos o arquivo Financial Sample.xlsx como origem e criamos as tabelas:
* Fato:
  * F_SALES
* Dimensão:
  * D_Calendar
  * D_Discount
  * D_Geographt
  * D_Product
  * D_Segment
 ![image](https://github.com/user-attachments/assets/58252ba8-207b-4cbc-8c58-783f8a587871)

## Premissas 
* separar claramente os atributos descritivos (dimensões) das métricas (fato)
* facilitar análises temporais através da dimensão D_Calendar
* Reduzir redundâncias de dados
* Permitir analises por diferentes perspectivas (geografia, produto, segmento, etc.)
* atenção a performance das consultas
* Isolamento em dimensões diferentes os diversos conjuntos de informação (tempo, geografia, produto, segmento de atuação, etc.)
  
## Processo de criação
No Power Query <br>
a. Dimensão: D_Calendar
1) Duplique a tabela Financials Sample
2) Mantenha apenas as colunas: date, month_number, month_name, year
3) Remova duplicatas para ter datas únicas
4) Adcione a coluna id_date(PK) como indice iniciando em 1
   
b. Dimensão: D_Discount
1) Duplique a tabela Financials Sample
2) Mantenha apenas as colunas: discount_band, discounts
3) Remova duplicatas
4) Adicione a coluna id_discount(PK) como indice iniciando em 1
   
c. Dimensão: D_Geography
1) Duplique a tabela Financials Sample
2) Mantenha apenas as colunas: country
3) Remova duplicatas
4) Adicione a coluna id_geography(PK) como indice iniciando em 1
   
d. Dimensão: D_Product
1) Duplique a tabela Financials Sample
2) Mantenha apenas as colunas: product, manufacturing_price, sale_price
3) Remova duplicatas
4) Adicione a coluna id_product(PK) como indice iniciando em 1
   
e. Dimensão: D_Segment
1) Duplique a tabela Financials Sample
2) Mantenha apenas as colunas: segment
3) Remova duplicatas
4) Adicione a coluna id_segment (PK) como indice iniciando em 1
   
f. Fato: F_SALES
1) Duplique a tabela Financials Sample
2) Adcione ID_Sales (PK) como índice iniciando em 1
3) Adicione as chaves estrangeiras através da mesclagem:
   - Mesclar com D_CALENDAR usando 'date' para obter date_id
   - Mesclar com D_GEOGRAPHY usando 'country' para obter geography_id
   - Mesclar com D_SEGMENT usando 'segment' para obter segment_id
   - Mesclar com D_PRODUCT usando 'product' para obter product_id
   - Mesclar com D_DISCOUNT usando 'discount_band' para obter discount_id
5) Mantenha apenas as colunas: id_sales (PK), sales, units_sold, profit, COGS, gross_sales, date_id (FK), geography_id(FK), segment_id(FK), product_id(FK), discount_id(FK)
6) Criar os relacionamentos no modelo. As dimensões se relacionam com a tabela fato através de suas respectivas chaves primarias (PK) e chaves estrangeiras (FK) 
   - F_SALES[id_date] -> D_CALENDAR[date_id]
   - F_SALES[geography_id] -> D_GEOGRAPHY[geography_id]
   - F_SALES[segment_id] -> D_SEGMENT[segment_id]
   - F_SALES[product_id] -> D_PRODUCT[product_id]
   - F_SALES[discount_id] -> D_DISCOUNT[discount_id]

## Estatisticas 

1) Serão criadas medidas via DAX
   
| medida                 | DAX                                         |
| :--------------------- | :------------------------------------------ |
| Unidades vendidas      | Tot Units sold = sum(F_SALES[Units Sold])   |
| Mínimo preço de venda  | Tot Units sold = sum(F_SALES[Units Sold])   |
| Máximo preço de venda  | Tot Units sold = sum(F_SALES[Units Sold])   |
| Mediana preço de venda | Tot Units sold = sum(F_SALES[Units Sold])   |
| Média preço de venda   | Tot Units sold = sum(F_SALES[Units Sold])   |

2) E adicionar cartões para mostrar essas medidas
![image](https://github.com/user-attachments/assets/8bac5fdd-0764-4098-be00-460cf46c8ee3)

## Arquivo
Desafio DIO e-commerce.pbix - Arquivo Power BI contendo as tabelas: fato e dimensão e a tabela original da qual partimos para a criação do modelo estrela. Além disso, na guia de visualizaçdo do modelo podemos ver as tabelas e seus respectivos relacionamentos.


