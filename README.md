# 🚀 Pipeline ETL de Análise de Vendas - Desafio Desafio-Data-Wrangling-e-Pipelines-em-Python - SQUAD ENIAC
# Membros: Alessandra, Aline, Caroline, Clara, Dayane, Elaine, Maria Elacide, Tandara

[![Status](https://img.shields.io/badge/Status-Completo-green.svg)]()
[![Tecnologias](https://img.shields.io/badge/Linguagem-Python-blue.svg)]()

## Descrição desse projeto

Este projeto implementa um **Pipeline de ETL (Extração, Transformação e Carga)** em Python para processar e estruturar dados brutos de vendas (Arquivo `vendas.csv`). Almejamos limpar, padronizar e enriquecer os dados para que possam ser carregados em um banco de dados (SQLite), prontos para análise e relatórios da empresa em questão.

O script executa as seguintes etapas principais:
1.  **Extração:** Leitura do arquivo CSV hospedado no Google Drive, pasta do projeto (via `gdown`).
2.  **Transformação:** Limpeza e padronização de datas, correção de tipos de dados, tratamento de valores nulos/negativos e cálculo de `valor_total`.
3.  **Carga:** Criação e preenchimento de duas tabelas em SQLite (`tb_vendas` e `tb_clientes_unicos`), estabelecendo um relacionamento de chave estrangeira.

Em resumo, o fluxo do pipeline é: 
**CSV (Google Drive) $\rightarrow$ Extração (`gdown`/`pandas`) $\rightarrow$ Transformação $\rightarrow$ Carga (SQLite)**.

## Tecnologias que foram usadas

* **Linguagem de programação principal:** Python
* **Bibliotecas:**
    * `pandas`: Para manipulação e *data wrangling* dos dados.
    * `sqlite3`: Para a fase de carga (terceira etapa) para criação e manipulação do banco de dados.
    * `numpy`: Em conjunto com o pandas.
    * `gdown`: Para fazer o download do CSV diretamente do Google Drive no Colab.
* **Ambiente:** Google Colab

## Estrutura desse Pipeline (Etapa de transformações)

As seguintes transformações de *Data Wrangling* foram aplicadas nos dados:

| Etapa | Descrição da Transformação |
| :--- | :--- |
| **Extração** | Leitura e filtros iniciais: `preco_unitario > 100`, ordenação decrescente pelo preço. |
| **Data** | Padronização da coluna `data_venda` para o formato `YYYY-MM-DD`, além de padronizar o type. |
| **Tratamento dos nulo** | Substituição de valores nulos na coluna `categoria` por "Não informado". |
| **Tipo de dados** | Correção da coluna `quantidade`, substituindo o texto 'três' por '3' e convertendo-a para o tipo inteiro. |
| **Valores negativos** | Remoção de linhas onde `preco_unitario` era negativo, para limpeza. |
| **Cálculo** | Criação da coluna `valor_total` (calculada por `quantidade * preco_unitario`). |

## Instruções de como executar o projeto

Este código foi projetado para rodar no **Google Colab**, utilizando-se de upload e acesso ao Google Drive.

**Requisitos:**

* Acesso ao Google Colab (ou qualquer ambiente Python com as bibliotecas sinalizadas instaladas).
* O ou um arquivo CSV de vendas com a estrutura esperada pelo script.

**Passos para executar no Colab:**

Este código foi otimizado para rodar no Google Colab, fazendo uso das bibliotecas gdown, pandas e sqlite3.

1. **Abra o Notebook:** Cole o conteúdo do arquivo `eniac_desafio_data_wrangling_e_pipelines_em_python.py` em uma célula de código do Google Colab.
2. **Suba o arquivo CSV:** O script tenta ler um arquivo chamado `vendas.csv`. Você deve garantir que o CSV está no mesmo diretório de execução ou alterar o `arquivo_id` e `url` no trecho:
    ```python
    arquivo_id = "13IPaHOFEnvDkApg4E6jwyhvHvaOHFOLX"  # Não esqueça de ajustar o caminho para novos arquivos, ok?
    url = f"https://drive.google.com/uc?id={arquivo_id}"
    gdown.download(url, output, quiet=False)
    ```
       Dica: Você pode simplesmente subir o arquivo `vendas.csv` na sessão do Colab ou usar o `gdown` como configurado.
       O código também disponível aqui https://drive.google.com/file/d/1PbVcJZ1MOek4_OWEP0lDb6ZZEckqZfAv/view?usp=sharing ou no notebook https://colab.research.google.com/drive/1n45URFAAXMoZqlSYHwV5z-hhZUP4kNUR?usp=sharing
   
4.  **Execute as células:** Execute as células de código sequencialmente.
5.  **Download do banco:** Após executar, o script faz o download automático do banco de dados gerado:
    ```python
    from google.colab import files
    files.download('vendas.db')
    ```

## Consultas SQL de validação

Após a Carga (Load), o script valida o resultado com consultas SQL. A consulta principal gerada é:

```sql
SELECT categoria,
       ROUND(SUM(valor_total), 2) AS total_vendas
FROM tb_vendas
GROUP BY categoria
ORDER BY total_vendas DESC;

---

📄 Licença
Este projeto é de código aberto e está sob a licença MIT.

📧 Contato
Alessandra Machado - @alessandramdsz

Link do projeto: [https://github.com/alessandramdsz/Desafio-Data-Wrangling-e-Pipelines-em-Python]
