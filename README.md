# Extracting-Statistics-in-PySpark

Aqui está um exemplo de um README que você pode usar no GitHub para o seu projeto:

---

# Análise Descritiva de Dados Financeiros com PySpark

Este repositório contém um projeto que utiliza PySpark para realizar análises descritivas em um conjunto de dados de ações do S&P 500. O objetivo principal é explorar o dataset e responder a perguntas específicas sobre o comportamento das ações ao longo de cinco anos.

## Objetivos do Projeto

- Extrair e analisar estatísticas descritivas dos preços das ações.
- Identificar padrões e tendências, como volatilidade e frequência de preços de fechamento maiores que os de abertura.
- Visualizar dados de forma eficiente utilizando PySpark e bibliotecas complementares.

## Pré-requisitos

Para executar este projeto, você precisará das seguintes ferramentas:

- **Python 3.x**
- **PySpark**
- **Google Colab** (opcional, mas recomendado)
- **Kaggle API** (para download dos dados)

## Instalação

Siga os passos abaixo para configurar o ambiente:

1. **Instalação das bibliotecas necessárias**:
    ```bash
    pip install pyspark
    pip install kaggle
    ```

2. **Configuração da API do Kaggle**:
    - Baixe o arquivo `kaggle.json` com suas credenciais da API do Kaggle.
    - No Google Colab, faça o upload e configure o ambiente:
    ```python
    from google.colab import files
    files.upload()  # Faça o upload do arquivo kaggle.json

    !mkdir -p ~/.kaggle
    !cp kaggle.json ~/.kaggle/
    !chmod 600 ~/.kaggle/kaggle.json
    ```

3. **Download do Dataset**:
    - Baixe os dados do Kaggle:
    ```bash
    kaggle datasets download -d camnugent/sandp500
    unzip sandp500.zip
    ```

## Utilização

Após configurar o ambiente, siga as instruções no notebook para realizar as análises:

1. **Carregar os dados**:
    ```python
    from pyspark.sql import SparkSession
    spark = SparkSession.builder.master("local[*]").getOrCreate()
    df = spark.read.csv('all_stocks_5yr.csv', header=True, inferSchema=True)
    ```

2. **Executar análises descritivas**:
    - Exemplo de análise: Contar o número de empresas distintas no dataset.
    ```python
    empresas_distintas = df.select("Name").distinct().count()
    print(f"Número de empresas distintas: {empresas_distintas}")
    ```

3. **Visualizar resultados**:
    - Você pode gerar gráficos e tabelas para visualizar os resultados das análises, como a frequência de preços de fechamento superiores aos de abertura.

## Exemplos de Análises Realizadas

- **Frequência de Fechamento Maior que Abertura**:
    ```python
    fechamento_maior_abertura = df.filter(df.close > df.open)
    frequencia_maior_abertura = fechamento_maior_abertura.count() / df.count()
    print(f"Frequência: {frequencia_maior_abertura:.2%}")
    ```

- **Ação com Maior Volatilidade**:
    ```python
    from pyspark.sql.functions import stddev
    volatilidade = df.groupBy('Name').agg(stddev('Close').alias('Volatility')).orderBy('Volatility', ascending=False)
    print(volatilidade.show(1))
    ```

