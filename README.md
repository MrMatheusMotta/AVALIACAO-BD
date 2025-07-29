# Sistema de Banco de Dados para Cafeteria

## Visão Geral do Projeto

Este projeto consiste no desenvolvimento completo de um sistema de banco de dados relacional para uma cafeteria, desde a concepção do modelo de dados até a implementação física no MySQL e a manipulação de dados através de consultas SQL avançadas. O objetivo principal foi aplicar os princípios de modelagem de banco de dados e dominar uma ampla gama de comandos SQL para simular funcionalidades reais de um sistema de informação.

## Modelagem de Dados

O banco de dados foi projetado seguindo as melhores práticas de modelagem, passando por duas fases cruciais:

1.  **Modelo Conceitual:** Representação de alto nível das entidades e seus relacionamentos, focado na lógica de negócio e independente da tecnologia. Este modelo definiu as principais entidades: `CLIENTE`, `PEDIDOS`, `PRODUTO` e `ENTREGAS`, e seus relacionamentos.

2.  **Modelo Lógico:** Tradução do modelo conceitual para uma estrutura que pode ser implementada em um SGBDR relacional, definindo tabelas, colunas, chaves primárias (PKs) e chaves estrangeiras (FKs), incluindo a resolução de relacionamentos Muitos-para-Muitos (N:M) através de tabelas associativas.

### Diagrama do Modelo Lógico

O modelo lógico final consiste nas seguintes tabelas e seus principais relacionamentos:

* **CLIENTE:**
    * `ID_CLIENTE` (PK)
    * `NOME`
    * `ENDERECO`
* **PRODUTO:**
    * `ID_PRODUTO` (PK)
    * `NOME`
    * `DESCRICAO`
    * `VALOR`
    * `CATEGORIA`
* **PEDIDOS:**
    * `ID_PEDIDO` (PK)
    * `HORA`
    * `VALOR_TOTAL`
    * `STATUS`
    * `FK_ID_CLIENTE` (FK para CLIENTE)
* **ENTREGAS:**
    * `ID_ENTREGA` (PK)
    * `DATA`
    * `HORA`
    * `ENDERECO_ENTREGA`
    * `FK_ID_PEDIDO` (FK para PEDIDOS)
* **CONTER (Tabela Associativa PEDIDO_PRODUTO):**
    * `FK_PEDIDOS_ID_PEDIDO` (PK, FK para PEDIDOS)
    * `FK_PRODUTO_ID_PRODUTO` (PK, FK para PRODUTO)
    * `QUANTIDADE`

TODA A PARTE VISUAL ESTÁ CONTIDA NO ARQUIVO PDF NESTA BRANCH!

## Tecnologias Utilizadas

* **Banco de Dados:** MySQL
* **Linguagem de Consulta:** SQL (Structured Query Language)

## Como Configurar e Executar

Siga os passos abaixo para configurar o banco de dados e executar as consultas:

1.  **Instale o MySQL:** Certifique-se de ter o MySQL Server instalado em sua máquina.
2.  **Acesse o MySQL:** Utilize um cliente MySQL (como MySQL Workbench, DBeaver, ou o cliente de linha de comando `mysql`).

3.  **Crie e Popule o Banco de Dados:**
    * O arquivo `cafeteria_script.sql` contém os comandos `CREATE TABLE` para construir todas as tabelas e `INSERT INTO` para popular cada tabela com mais de 10 registros, respeitando todas as chaves primárias e estrangeiras.
    * Execute o conteúdo do arquivo `cafeteria_script.sql` no seu cliente MySQL.

    ```sql
    -- Exemplo de execução via linha de comando:
    mysql -u seu_usuario -p < caminho/para/cafeteria_script.sql
    ```

4.  **Execute as Consultas SQL:**
    * Após a criação e população do banco de dados, você pode executar as consultas SQL fornecidas no arquivo (ou diretamente no seu cliente MySQL) para explorar os dados e validar as funcionalidades. As consultas estão organizadas por categoria de acordo com os objetivos do projeto.

## Funcionalidades e Consultas SQL

O projeto demonstra a manipulação e análise de dados através de uma ampla gama de consultas SQL, cobrindo os seguintes aspectos:

* **Seleção e Filtragem Básica (`SELECT`, `WHERE`):** Extração de dados específicos com base em condições simples.
    ```sql
    -- Exemplo: Seleciona todos os pedidos que estão com o status 'Em Processamento'.
    SELECT * FROM PEDIDOS WHERE STATUS = 'Em Processamento';
    ```
* **Agrupamento e Ordenação (`GROUP BY`, `ORDER BY` com Agregação):** Sumarização de dados e organização dos resultados.
    ```sql
    -- Exemplo: Conta quantos pedidos cada cliente fez, ordenando pelo cliente que fez mais pedidos.
    SELECT C.NOME, COUNT(P.ID_PEDIDO) AS TotalPedidos
    FROM CLIENTE AS C JOIN PEDIDOS AS P ON C.ID_CLIENTE = P.FK_ID_CLIENTE
    GROUP BY C.NOME ORDER BY TotalPedidos DESC;
    ```
* **Operadores Aritméticos:** Realização de cálculos diretos nos resultados da consulta.
* **Operadores de Comparação:** Filtragem avançada utilizando `=`, `!=`, `>`, `<`.
* **Operadores Lógicos (`AND`, `OR`, `NOT`):** Combinação e negação de múltiplas condições de filtragem.
* **Operadores Auxiliares (`BETWEEN`, `LIKE`, `IN`):** Filtragem por intervalos, padrões de texto e listas de valores.
* **Funções de Agregação (`SUM()`, `AVG()`, `MAX()`, `MIN()`, `COUNT()`):** Cálculos sumarizados sobre conjuntos de dados.
* **Funções de Datas (`CURDATE()`, `YEAR()`):** Manipulação e filtragem de dados temporais.
* **Subconsultas:** Consultas aninhadas para resolver problemas mais complexos, envolvendo agrupamento e correlação de dados.
    ```sql
    -- Exemplo: Clientes que fizeram pedidos com valor total acima da média de todos os pedidos.
    SELECT NOME, ENDERECO FROM CLIENTE WHERE ID_CLIENTE IN (
        SELECT FK_ID_CLIENTE FROM PEDIDOS WHERE VALOR_TOTAL > (SELECT AVG(VALOR_TOTAL) FROM PEDIDOS)
        GROUP BY FK_ID_CLIENTE
    );
    ```
* **JOINs (`INNER`, `LEFT`, `RIGHT`):** Combinação de dados de múltiplas tabelas para visualização integrada e análise de relacionamentos.
    ```sql
    -- Exemplo (INNER JOIN): Pedidos que TÊM entregas.
    SELECT P.ID_PEDIDO, P.VALOR_TOTAL, E.DATA AS DataEntrega, E.ENDERECO_ENTREGA
    FROM PEDIDOS AS P INNER JOIN ENTREGAS AS E ON P.ID_PEDIDO = E.FK_ID_PEDIDO;

    -- Exemplo (LEFT JOIN): Todos os pedidos e suas respectivas entregas, se houver.
    SELECT P.ID_PEDIDO, P.STATUS, E.ID_ENTREGA, E.DATA AS DataEntrega
    FROM PEDIDOS AS P LEFT JOIN ENTREGAS AS E ON P.ID_PEDIDO = E.FK_ID_PEDIDO;
    ```

## Aprendizados e Conclusão

Este projeto solidificou a compreensão sobre:

* O ciclo de vida completo da modelagem de banco de dados (conceitual, lógico, físico).
* A importância da integridade referencial e o uso correto de chaves primárias e estrangeiras.
* A versatilidade da linguagem SQL para consultar, filtrar, agregar e unir dados de formas complexas.
* A aplicação prática de comandos SQL para resolver problemas de negócio e extrair insights valiosos.

Este trabalho serve como uma base robusta para futuros projetos de banco de dados e demonstra proficiência na manipulação e análise de dados relacionais.

## Autor

* SGT MOTTA - I CADSPM / 2025 *
