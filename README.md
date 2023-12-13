# Guia de Visualização de Dados com Metabase

## Introdução
Este guia fornece um passo a passo sobre a utilização do Metabase para transformar dados em insights valiosos. Abrange a motivação por trás da escolha desta ferramenta, o processo de configuração, criação de dashboards eficientes e utilização de consultas SQL para análise de dados.

## Por que escolher o Metabase?
O Metabase é reconhecido pela sua natureza de código aberto e custo zero, alinhando-se com a busca por soluções econômicas e flexíveis. O amplo suporte da comunidade e a facilidade de integração com diversas fontes de dados fazem dele uma escolha estratégica. Avaliações positivas de usuários e especialistas destacam o Metabase como adequado para projetos ágeis e com recursos limitados.

## Configuração do Metabase
A configuração do Metabase é facilitada por uma documentação detalhada e uma comunidade ativa. A integração com o Docker torna o processo ainda mais acessível, permitindo uma implantação rápida e adaptável à infraestrutura de dados existente.

<img width="938" alt="image" src="https://github.com/camilaanacleto/Grafico-Databricks/assets/68927480/49a1e363-5bdb-480c-ae68-08f081378df5">

## Criação de Dashboard
A interface intuitiva do Metabase simplifica a criação de dashboards. Com base em experiências compartilhadas por usuários e dicas de especialistas, é possível compor um painel visualmente atrativo e funcional, que facilita a navegação e a interpretação dos dados.

## Estratégias de Visualização de Dados
A escolha da técnica de visualização correta é crucial para a apresentação eficaz dos dados. O Metabase oferece uma variedade de opções de visualização, permitindo a seleção da mais adequada para cada tipo de dado, de acordo com as práticas recomendadas na área de visualização de dados.

<img width="695" alt="image" src="https://github.com/camilaanacleto/Grafico-Databricks/assets/68927480/05764706-e655-48fa-bf86-718ee4695a56">

<img width="947" alt="image" src="https://github.com/camilaanacleto/Grafico-Databricks/assets/68927480/9f928ce3-56ff-4cef-b5d9-c4205229b5e7">


## Utilização de SQL para Análises
O Metabase também se destaca para análises mais detalhadas através de SQL. A criação de 'views' como "Categorias mais recorrente nos 5 arquivos de Cnpj" ou "FILTRO - Chocolate - Categoria X Consumo X Canal" ilustra como a ferramenta pode ser utilizada para análises detalhadas e intuitivas de dados. Abaixo deixo o códigos de SQL da view "FILTRO - Chocolate - Categoria X Consumo X Canal":

```sql
SELECT
    categoria_api_2.id AS idcategory,
    categoria_api_2.name,
    CASE
        WHEN cnpj1.cnae_fiscal_principal = '58296' THEN 'Supermercados'
        WHEN cnpj1.cnae_fiscal_principal = '9600' THEN 'Minimercados, Mercearias e Armazéns'
        WHEN cnpj1.cnae_fiscal_principal = '92612' THEN 'Lanchonetes, Casas de Chá, de Sucos e Similares'
        WHEN cnpj1.cnae_fiscal_principal = '515874' THEN 'Restaurantes e Similares'
        WHEN cnpj1.cnae_fiscal_principal = '184274' THEN 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas'
        WHEN cnpj1.cnae_fiscal_principal = '574234' THEN 'Hipermercados'
        WHEN cnpj1.cnae_fiscal_principal = '577735' THEN 'Serviços de Alimentação para Eventos e Recepções - Bufê'
        WHEN cnpj1.cnae_fiscal_principal = '9182' THEN 'Comércio Atacadista de Mercadorias em Geral, com Predominância de Produtos Alimentícios'
        WHEN cnpj1.cnae_fiscal_principal = '2006' THEN 'Comércio Atacadista de Produtos Alimentícios em Geral'
        WHEN cnpj1.cnae_fiscal_principal = '561204' THEN 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas sem entreterimento'
        WHEN cnpj1.cnae_fiscal_principal = '5611203' THEN 'Lanchonete'
        WHEN cnpj1.cnae_fiscal_principal = '5611201' THEN 'Restaurantes Similares'
        ELSE 'Outro'  
    END AS cnae_fiscal_principal,
    cnpj1.sigla_uf,
    EXTRACT(MONTH FROM api_sale_2.saledate) AS mes,
    SUM(api_sale_2.amount) AS total_amount
FROM
    group3.public.api_sale_2
INNER JOIN
    group3.public.categoria_api_2 ON api_sale_2.idcategory = categoria_api_2.id
INNER JOIN
    group3.public.cnpj1 ON api_sale_2.cnpj = cnpj1.cnpj
WHERE
    categoria_api_2.name = 'Chocolate' 
    
    AND (
        {{agrupamento_canal}} = '#' OR
        (
            ({{agrupamento_canal}} = 'Supermercados' AND cnpj1.cnae_fiscal_principal = '58296') OR
            ({{agrupamento_canal}} = 'Minimercados, Mercearias e Armazéns' AND cnpj1.cnae_fiscal_principal = '9600') OR
            ({{agrupamento_canal}} = 'Lanchonetes, Casas de Chá, de Sucos e Similares' AND cnpj1.cnae_fiscal_principal = '92612') OR
            ({{agrupamento_canal}} = 'Restaurantes e Similares' AND cnpj1.cnae_fiscal_principal = '515874') OR
            ({{agrupamento_canal}} = 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas' AND cnpj1.cnae_fiscal_principal = '184274') OR
            ({{agrupamento_canal}} = 'Hipermercados' AND cnpj1.cnae_fiscal_principal = '574234') OR
            ({{agrupamento_canal}} = 'Serviços de Alimentação para Eventos e Recepções - Bufê' AND cnpj1.cnae_fiscal_principal = '577735') OR
            ({{agrupamento_canal}} = 'Comércio Atacadista de Mercadorias em Geral, com Predominância de Produtos Alimentícios' AND cnpj1.cnae_fiscal_principal = '9182') OR
            ({{agrupamento_canal}} = 'Comércio Atacadista de Produtos Alimentícios em Geral' AND cnpj1.cnae_fiscal_principal = '2006') OR
            ({{agrupamento_canal}} = 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas sem entretenimento' AND cnpj1.cnae_fiscal_principal = '561204') OR
            ({{agrupamento_canal}} = 'Lanchonete' AND cnpj1.cnae_fiscal_principal = '5611203') OR
            ({{agrupamento_canal}} = 'Restaurantes Similares' AND cnpj1.cnae_fiscal_principal = '5611201')
        )
        OR
        ({{agrupamento_canal}} = 'Outro' AND cnpj1.cnae_fiscal_principal NOT IN ('58296', '9600', '92612', '515874', '184274', '574234', '577735', '9182', '2006', '561204', '5611203', '5611201'))
    )

    AND ({{agrupamento_regiao}} <> 'São Paulo (SP)' OR cnpj1.sigla_uf = 'SP')
    AND ({{agrupamento_regiao}} <> 'Rondônia (RO)' OR cnpj1.sigla_uf = 'RO')
    AND ({{agrupamento_regiao}} <> 'Acre (AC)' OR cnpj1.sigla_uf = 'AC')
    AND ({{agrupamento_regiao}} <> 'Amazonas (AM)' OR cnpj1.sigla_uf = 'AM')
    AND ({{agrupamento_regiao}} <> 'Roraima (RR)' OR cnpj1.sigla_uf = 'RR')
    AND ({{agrupamento_regiao}} <> 'Pará (PA)' OR cnpj1.sigla_uf = 'PA')
    AND ({{agrupamento_regiao}} <> 'Amapá (AP)' OR cnpj1.sigla_uf = 'AP')
    AND ({{agrupamento_regiao}} <> 'Tocantins (TO)' OR cnpj1.sigla_uf = 'TO')
    AND ({{agrupamento_regiao}} <> 'Maranhão (MA)' OR cnpj1.sigla_uf = 'MA')
    AND ({{agrupamento_regiao}} <> 'Piauí (PI)' OR cnpj1.sigla_uf = 'PI')
    AND ({{agrupamento_regiao}} <> 'Ceará (CE)' OR cnpj1.sigla_uf = 'CE')
    AND ({{agrupamento_regiao}} <> 'Rio Grande do Norte (RN)' OR cnpj1.sigla_uf = 'RN')
    AND ({{agrupamento_regiao}} <> 'Paraíba (PB)' OR cnpj1.sigla_uf = 'PB')
    AND ({{agrupamento_regiao}} <> 'Pernambuco (PE)' OR cnpj1.sigla_uf = 'PE')
    AND ({{agrupamento_regiao}} <> 'Alagoas (AL)' OR cnpj1.sigla_uf = 'AL')
    AND ({{agrupamento_regiao}} <> 'Bahia (BA)' OR cnpj1.sigla_uf = 'BA')
    AND ({{agrupamento_regiao}} <> 'Minas Gerais (MG)' OR cnpj1.sigla_uf = 'MG')
    AND ({{agrupamento_regiao}} <> 'Espírito Santo (ES)' OR cnpj1.sigla_uf = 'ES')
    AND ({{agrupamento_regiao}} <> 'Rio de Janeiro (RJ)' OR cnpj1.sigla_uf = 'RJ')
    AND ({{agrupamento_regiao}} <> 'Paraná (PR)' OR cnpj1.sigla_uf = 'PR')
    AND ({{agrupamento_regiao}} <> 'Santa Catarina (SC)' OR cnpj1.sigla_uf = 'SC')
    AND ({{agrupamento_regiao}} <> 'Rio Grande do Sul (RS)' OR cnpj1.sigla_uf = 'RS')
    AND ({{agrupamento_regiao}} <> 'Mato Grosso do Sul (MS)' OR cnpj1.sigla_uf = 'MS')
    AND ({{agrupamento_regiao}} <> 'Mato Grosso (MT)' OR cnpj1.sigla_uf = 'MT')
    AND ({{agrupamento_regiao}} <> 'Goiás (GO)' OR cnpj1.sigla_uf = 'GO')
    AND ({{agrupamento_regiao}} <> 'Distrito Federal (DF)' OR cnpj1.sigla_uf = 'DF')
    AND ({{agrupamento_regiao}} <> 'Sergipe (SE)' OR cnpj1.sigla_uf = 'SE')
         
GROUP BY
    categoria_api_2.id,
    categoria_api_2.name,
    cnpj1.cnae_fiscal_principal,
    cnpj1.sigla_uf,
    mes
ORDER BY
    categoria_api_2.name ASC,
    cnpj1.sigla_uf ASC,
    mes ASC;

SELECT
    categoria_api_2.id AS idcategory,
    categoria_api_2.name,
    CASE
        WHEN cnpj1.cnae_fiscal_principal = '58296' THEN 'Supermercados'
        WHEN cnpj1.cnae_fiscal_principal = '9600' THEN 'Minimercados, Mercearias e Armazéns'
        WHEN cnpj1.cnae_fiscal_principal = '92612' THEN 'Lanchonetes, Casas de Chá, de Sucos e Similares'
        WHEN cnpj1.cnae_fiscal_principal = '515874' THEN 'Restaurantes e Similares'
        WHEN cnpj1.cnae_fiscal_principal = '184274' THEN 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas'
        WHEN cnpj1.cnae_fiscal_principal = '574234' THEN 'Hipermercados'
        WHEN cnpj1.cnae_fiscal_principal = '577735' THEN 'Serviços de Alimentação para Eventos e Recepções - Bufê'
        WHEN cnpj1.cnae_fiscal_principal = '9182' THEN 'Comércio Atacadista de Mercadorias em Geral, com Predominância de Produtos Alimentícios'
        WHEN cnpj1.cnae_fiscal_principal = '2006' THEN 'Comércio Atacadista de Produtos Alimentícios em Geral'
        WHEN cnpj1.cnae_fiscal_principal = '561204' THEN 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas sem entreterimento'
        WHEN cnpj1.cnae_fiscal_principal = '5611203' THEN 'Lanchonete'
        WHEN cnpj1.cnae_fiscal_principal = '5611201' THEN 'Restaurantes Similares'
        ELSE 'Outro'  
    END AS cnae_fiscal_principal,
    cnpj1.sigla_uf,
    EXTRACT(MONTH FROM api_sale_2.saledate) AS mes,
    SUM(api_sale_2.amount) AS total_amount
FROM
    group3.public.api_sale_2
INNER JOIN
    group3.public.categoria_api_2 ON api_sale_2.idcategory = categoria_api_2.id
INNER JOIN
    group3.public.cnpj1 ON api_sale_2.cnpj = cnpj1.cnpj
WHERE
    categoria_api_2.name = 'Chocolate' 
    
    AND (
        {{agrupamento_canal}} = '#' OR
        (
            ({{agrupamento_canal}} = 'Supermercados' AND cnpj1.cnae_fiscal_principal = '58296') OR
            ({{agrupamento_canal}} = 'Minimercados, Mercearias e Armazéns' AND cnpj1.cnae_fiscal_principal = '9600') OR
            ({{agrupamento_canal}} = 'Lanchonetes, Casas de Chá, de Sucos e Similares' AND cnpj1.cnae_fiscal_principal = '92612') OR
            ({{agrupamento_canal}} = 'Restaurantes e Similares' AND cnpj1.cnae_fiscal_principal = '515874') OR
            ({{agrupamento_canal}} = 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas' AND cnpj1.cnae_fiscal_principal = '184274') OR
            ({{agrupamento_canal}} = 'Hipermercados' AND cnpj1.cnae_fiscal_principal = '574234') OR
            ({{agrupamento_canal}} = 'Serviços de Alimentação para Eventos e Recepções - Bufê' AND cnpj1.cnae_fiscal_principal = '577735') OR
            ({{agrupamento_canal}} = 'Comércio Atacadista de Mercadorias em Geral, com Predominância de Produtos Alimentícios' AND cnpj1.cnae_fiscal_principal = '9182') OR
            ({{agrupamento_canal}} = 'Comércio Atacadista de Produtos Alimentícios em Geral' AND cnpj1.cnae_fiscal_principal = '2006') OR
            ({{agrupamento_canal}} = 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas sem entretenimento' AND cnpj1.cnae_fiscal_principal = '561204') OR
            ({{agrupamento_canal}} = 'Lanchonete' AND cnpj1.cnae_fiscal_principal = '5611203') OR
            ({{agrupamento_canal}} = 'Restaurantes Similares' AND cnpj1.cnae_fiscal_principal = '5611201')
        )
        OR
        ({{agrupamento_canal}} = 'Outro' AND cnpj1.cnae_fiscal_principal NOT IN ('58296', '9600', '92612', '515874', '184274', '574234', '577735', '9182', '2006', '561204', '5611203', '5611201'))
    )

    AND ({{agrupamento_regiao}} <> 'São Paulo (SP)' OR cnpj1.sigla_uf = 'SP')
    AND ({{agrupamento_regiao}} <> 'Rondônia (RO)' OR cnpj1.sigla_uf = 'RO')
    AND ({{agrupamento_regiao}} <> 'Acre (AC)' OR cnpj1.sigla_uf = 'AC')
    AND ({{agrupamento_regiao}} <> 'Amazonas (AM)' OR cnpj1.sigla_uf = 'AM')
    AND ({{agrupamento_regiao}} <> 'Roraima (RR)' OR cnpj1.sigla_uf = 'RR')
    AND ({{agrupamento_regiao}} <> 'Pará (PA)' OR cnpj1.sigla_uf = 'PA')
    AND ({{agrupamento_regiao}} <> 'Amapá (AP)' OR cnpj1.sigla_uf = 'AP')
    AND ({{agrupamento_regiao}} <> 'Tocantins (TO)' OR cnpj1.sigla_uf = 'TO')
    AND ({{agrupamento_regiao}} <> 'Maranhão (MA)' OR cnpj1.sigla_uf = 'MA')
    AND ({{agrupamento_regiao}} <> 'Piauí (PI)' OR cnpj1.sigla_uf = 'PI')
    AND ({{agrupamento_regiao}} <> 'Ceará (CE)' OR cnpj1.sigla_uf = 'CE')
    AND ({{agrupamento_regiao}} <> 'Rio Grande do Norte (RN)' OR cnpj1.sigla_uf = 'RN')
    AND ({{agrupamento_regiao}} <> 'Paraíba (PB)' OR cnpj1.sigla_uf = 'PB')
    AND ({{agrupamento_regiao}} <> 'Pernambuco (PE)' OR cnpj1.sigla_uf = 'PE')
    AND ({{agrupamento_regiao}} <> 'Alagoas (AL)' OR cnpj1.sigla_uf = 'AL')
    AND ({{agrupamento_regiao}} <> 'Bahia (BA)' OR cnpj1.sigla_uf = 'BA')
    AND ({{agrupamento_regiao}} <> 'Minas Gerais (MG)' OR cnpj1.sigla_uf = 'MG')
    AND ({{agrupamento_regiao}} <> 'Espírito Santo (ES)' OR cnpj1.sigla_uf = 'ES')
    AND ({{agrupamento_regiao}} <> 'Rio de Janeiro (RJ)' OR cnpj1.sigla_uf = 'RJ')
    AND ({{agrupamento_regiao}} <> 'Paraná (PR)' OR cnpj1.sigla_uf = 'PR')
    AND ({{agrupamento_regiao}} <> 'Santa Catarina (SC)' OR cnpj1.sigla_uf = 'SC')
    AND ({{agrupamento_regiao}} <> 'Rio Grande do Sul (RS)' OR cnpj1.sigla_uf = 'RS')
    AND ({{agrupamento_regiao}} <> 'Mato Grosso do Sul (MS)' OR cnpj1.sigla_uf = 'MS')
    AND ({{agrupamento_regiao}} <> 'Mato Grosso (MT)' OR cnpj1.sigla_uf = 'MT')
    AND ({{agrupamento_regiao}} <> 'Goiás (GO)' OR cnpj1.sigla_uf = 'GO')
    AND ({{agrupamento_regiao}} <> 'Distrito Federal (DF)' OR cnpj1.sigla_uf = 'DF')
    AND ({{agrupamento_regiao}} <> 'Sergipe (SE)' OR cnpj1.sigla_uf = 'SE')
         
GROUP BY
    categoria_api_2.id,
    categoria_api_2.name,
    cnpj1.cnae_fiscal_principal,
    cnpj1.sigla_uf,
    mes
ORDER BY
    categoria_api_2.name ASC,
    cnpj1.sigla_uf ASC,
    mes ASC;

SELECT
    categoria_api_2.id AS idcategory,
    categoria_api_2.name,
    CASE
        WHEN cnpj1.cnae_fiscal_principal = '58296' THEN 'Supermercados'
        WHEN cnpj1.cnae_fiscal_principal = '9600' THEN 'Minimercados, Mercearias e Armazéns'
        WHEN cnpj1.cnae_fiscal_principal = '92612' THEN 'Lanchonetes, Casas de Chá, de Sucos e Similares'
        WHEN cnpj1.cnae_fiscal_principal = '515874' THEN 'Restaurantes e Similares'
        WHEN cnpj1.cnae_fiscal_principal = '184274' THEN 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas'
        WHEN cnpj1.cnae_fiscal_principal = '574234' THEN 'Hipermercados'
        WHEN cnpj1.cnae_fiscal_principal = '577735' THEN 'Serviços de Alimentação para Eventos e Recepções - Bufê'
        WHEN cnpj1.cnae_fiscal_principal = '9182' THEN 'Comércio Atacadista de Mercadorias em Geral, com Predominância de Produtos Alimentícios'
        WHEN cnpj1.cnae_fiscal_principal = '2006' THEN 'Comércio Atacadista de Produtos Alimentícios em Geral'
        WHEN cnpj1.cnae_fiscal_principal = '561204' THEN 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas sem entreterimento'
        WHEN cnpj1.cnae_fiscal_principal = '5611203' THEN 'Lanchonete'
        WHEN cnpj1.cnae_fiscal_principal = '5611201' THEN 'Restaurantes Similares'
        ELSE 'Outro'  
    END AS cnae_fiscal_principal,
    cnpj1.sigla_uf,
    EXTRACT(MONTH FROM api_sale_2.saledate) AS mes,
    SUM(api_sale_2.amount) AS total_amount
FROM
    group3.public.api_sale_2
INNER JOIN
    group3.public.categoria_api_2 ON api_sale_2.idcategory = categoria_api_2.id
INNER JOIN
    group3.public.cnpj1 ON api_sale_2.cnpj = cnpj1.cnpj
WHERE
    categoria_api_2.name = 'Chocolate' 
    
    AND (
        {{agrupamento_canal}} = '#' OR
        (
            ({{agrupamento_canal}} = 'Supermercados' AND cnpj1.cnae_fiscal_principal = '58296') OR
            ({{agrupamento_canal}} = 'Minimercados, Mercearias e Armazéns' AND cnpj1.cnae_fiscal_principal = '9600') OR
            ({{agrupamento_canal}} = 'Lanchonetes, Casas de Chá, de Sucos e Similares' AND cnpj1.cnae_fiscal_principal = '92612') OR
            ({{agrupamento_canal}} = 'Restaurantes e Similares' AND cnpj1.cnae_fiscal_principal = '515874') OR
            ({{agrupamento_canal}} = 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas' AND cnpj1.cnae_fiscal_principal = '184274') OR
            ({{agrupamento_canal}} = 'Hipermercados' AND cnpj1.cnae_fiscal_principal = '574234') OR
            ({{agrupamento_canal}} = 'Serviços de Alimentação para Eventos e Recepções - Bufê' AND cnpj1.cnae_fiscal_principal = '577735') OR
            ({{agrupamento_canal}} = 'Comércio Atacadista de Mercadorias em Geral, com Predominância de Produtos Alimentícios' AND cnpj1.cnae_fiscal_principal = '9182') OR
            ({{agrupamento_canal}} = 'Comércio Atacadista de Produtos Alimentícios em Geral' AND cnpj1.cnae_fiscal_principal = '2006') OR
            ({{agrupamento_canal}} = 'Bares e Outros Estabelecimentos Especializados em Servir Bebidas sem entretenimento' AND cnpj1.cnae_fiscal_principal = '561204') OR
            ({{agrupamento_canal}} = 'Lanchonete' AND cnpj1.cnae_fiscal_principal = '5611203') OR
            ({{agrupamento_canal}} = 'Restaurantes Similares' AND cnpj1.cnae_fiscal_principal = '5611201')
        )
        OR
        ({{agrupamento_canal}} = 'Outro' AND cnpj1.cnae_fiscal_principal NOT IN ('58296', '9600', '92612', '515874', '184274', '574234', '577735', '9182', '2006', '561204', '5611203', '5611201'))
    )

    AND ({{agrupamento_regiao}} <> 'São Paulo (SP)' OR cnpj1.sigla_uf = 'SP')
    AND ({{agrupamento_regiao}} <> 'Rondônia (RO)' OR cnpj1.sigla_uf = 'RO')
    AND ({{agrupamento_regiao}} <> 'Acre (AC)' OR cnpj1.sigla_uf = 'AC')
    AND ({{agrupamento_regiao}} <> 'Amazonas (AM)' OR cnpj1.sigla_uf = 'AM')
    AND ({{agrupamento_regiao}} <> 'Roraima (RR)' OR cnpj1.sigla_uf = 'RR')
    AND ({{agrupamento_regiao}} <> 'Pará (PA)' OR cnpj1.sigla_uf = 'PA')
    AND ({{agrupamento_regiao}} <> 'Amapá (AP)' OR cnpj1.sigla_uf = 'AP')
    AND ({{agrupamento_regiao}} <> 'Tocantins (TO)' OR cnpj1.sigla_uf = 'TO')
    AND ({{agrupamento_regiao}} <> 'Maranhão (MA)' OR cnpj1.sigla_uf = 'MA')
    AND ({{agrupamento_regiao}} <> 'Piauí (PI)' OR cnpj1.sigla_uf = 'PI')
    AND ({{agrupamento_regiao}} <> 'Ceará (CE)' OR cnpj1.sigla_uf = 'CE')
    AND ({{agrupamento_regiao}} <> 'Rio Grande do Norte (RN)' OR cnpj1.sigla_uf = 'RN')
    AND ({{agrupamento_regiao}} <> 'Paraíba (PB)' OR cnpj1.sigla_uf = 'PB')
    AND ({{agrupamento_regiao}} <> 'Pernambuco (PE)' OR cnpj1.sigla_uf = 'PE')
    AND ({{agrupamento_regiao}} <> 'Alagoas (AL)' OR cnpj1.sigla_uf = 'AL')
    AND ({{agrupamento_regiao}} <> 'Bahia (BA)' OR cnpj1.sigla_uf = 'BA')
    AND ({{agrupamento_regiao}} <> 'Minas Gerais (MG)' OR cnpj1.sigla_uf = 'MG')
    AND ({{agrupamento_regiao}} <> 'Espírito Santo (ES)' OR cnpj1.sigla_uf = 'ES')
    AND ({{agrupamento_regiao}} <> 'Rio de Janeiro (RJ)' OR cnpj1.sigla_uf = 'RJ')
    AND ({{agrupamento_regiao}} <> 'Paraná (PR)' OR cnpj1.sigla_uf = 'PR')
    AND ({{agrupamento_regiao}} <> 'Santa Catarina (SC)' OR cnpj1.sigla_uf = 'SC')
    AND ({{agrupamento_regiao}} <> 'Rio Grande do Sul (RS)' OR cnpj1.sigla_uf = 'RS')
    AND ({{agrupamento_regiao}} <> 'Mato Grosso do Sul (MS)' OR cnpj1.sigla_uf = 'MS')
    AND ({{agrupamento_regiao}} <> 'Mato Grosso (MT)' OR cnpj1.sigla_uf = 'MT')
    AND ({{agrupamento_regiao}} <> 'Goiás (GO)' OR cnpj1.sigla_uf = 'GO')
    AND ({{agrupamento_regiao}} <> 'Distrito Federal (DF)' OR cnpj1.sigla_uf = 'DF')
    AND ({{agrupamento_regiao}} <> 'Sergipe (SE)' OR cnpj1.sigla_uf = 'SE')
         
GROUP BY
    categoria_api_2.id,
    categoria_api_2.name,
    cnpj1.cnae_fiscal_principal,
    cnpj1.sigla_uf,
    mes
ORDER BY
    categoria_api_2.name ASC,
    cnpj1.sigla_uf ASC,
    mes ASC;
```

## Conclusão e Explorações Futuras
A escolha do Metabase representa um avanço na capacidade de análise de dados do projeto. Este guia marca o início de uma jornada para tornar análises complexas em processos simplificados e resultados acionáveis. A expectativa é que esta base de conhecimento seja um ponto de partida para futuras investigações e descobertas no campo dos dados.

