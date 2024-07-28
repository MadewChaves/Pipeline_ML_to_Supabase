### ENGLISH

## Pipeline_ML_to_Supabase
This project aims to create a data pipeline that extracts offers from the Mercado Livre website, processes this data and stores it in a database in the Supabase cloud. / Este projeto tem como objetivo criar um pipeline de dados que extrai ofertas do site Mercado Livre, processa esses dados e os armazena em um banco de dados na nuvem Supabase.

# Data Extraction
Data is extracted from multiple pages on the Mercado Livre website using the requests and BeautifulSoup libraries. The code performs web scraping to gather the following information:

Offer Titles: Extracted from HTML elements with the class promotion-item__title.
Offer Prices: Extracted from HTML elements with the class andes-money-amount-combo__main-container. Prices are formatted to remove the currency symbol and concatenate the cents.
The extraction is implemented in the following functions:

get_doc(url): Fetches the HTML content from a URL and returns a BeautifulSoup object.
get_items_title(doc): Extracts and returns offer titles from the HTML document.
get_items_low_prices(doc): Extracts and returns offer prices from the HTML document.
scrape_multiple_pages(n): Scrapes multiple pages from the website and returns a DataFrame with offer titles and prices.

# Data Transformation
The extracted data is processed and transformed to ensure it is in the correct format for analysis and storage. Key transformation steps include:

Price Conversion: Removes the currency symbol, replaces dots with nothing, and commas with decimal points, converting prices to float.
Title Normalization: Strips whitespace and converts titles to lowercase.
Duplicate Removal: Removes duplicate entries from the DataFrame.

# Data Loading
The transformed data is loaded into a Supabase database table using the psycopg2 library. The loading process includes:

Table Creation: Creates a table in the Supabase database to store the offers, with columns for ID, offer title, price, and scraping date.

Data Insertion: Inserts the transformed data into the created table. Uses the execute_values function for bulk insertion.

The data loading is handled by the following functions:

criar_tabela(): Creates the OFERTAS_DO_DIA_ML table in the Supabase database.
insercao_dados(dataframe): Inserts the DataFrame data into the database table.

### PORTUGUÊS

## Pipeline_ML_to_Supabase
Este projeto tem como objetivo criar um pipeline de dados que extrai ofertas do site Mercado Livre, processa esses dados e os armazena em um banco de dados na nuvem Supabase.

# Extração
Na fase de extração, o código utiliza a biblioteca requests para fazer solicitações HTTP e obter o conteúdo HTML das páginas do Mercado Livre. A biblioteca BeautifulSoup é então usada para analisar o HTML e extrair as informações desejadas. Especificamente:

get_doc(url): Faz uma solicitação para a URL fornecida e retorna o conteúdo HTML como um objeto BeautifulSoup. Se a resposta HTTP não for bem-sucedida (status code diferente de 200), uma exceção é levantada.

get_items_title(doc): Analisa o documento HTML para encontrar e extrair os títulos das ofertas, que estão contidos em elementos <p> com a classe promotion-item__title.

get_items_low_prices(doc): Analisa o documento HTML para extrair os preços das ofertas. Os preços são extraídos a partir de elementos <div> com a classe andes-money-amount-combo__main-container, onde são formatados para remover o símbolo de moeda e são concatenados com centavos.

scrape_multiple_pages(n): Raspagem de várias páginas do Mercado Livre. Utiliza as funções anteriores para obter títulos e preços de cada página e retorna um DataFrame do Pandas contendo essas informações.

# Transformação
Na fase de transformação, os dados extraídos são processados para garantir que estejam em um formato adequado para análise e armazenamento:

df_mercado_livre['LOW_PRICE']: Remove o símbolo da moeda ("R$"), substitui pontos por nada e vírgulas por pontos decimais, e converte os preços para o tipo de dado float.

df_mercado_livre['TITLE']: Remove espaços em branco dos títulos das ofertas, normaliza o texto para minúsculas e remove duplicatas para evitar entradas repetidas no DataFrame.

# Carregamento
Na fase de carregamento, os dados processados são inseridos em uma tabela no banco de dados Supabase:

Criação da Tabela: Utiliza a biblioteca psycopg2 para criar uma nova tabela chamada OFERTAS_DO_DIA_ML no banco de dados Supabase. A tabela possui colunas para o ID da oferta, título da oferta, preço da oferta e data de raspagem.

Inserção de Dados: Conecta-se ao banco de dados Supabase e insere os dados do DataFrame na tabela criada. Utiliza a função execute_values para inserir os dados de forma eficiente em massa. Em caso de erro durante a inserção, a transação é revertida.

