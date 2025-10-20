# Payroll PDF Parser - Automação da Extração de Dados de Folhas de Pagamento

## 🎯 Visão Geral do Projeto

Este projeto automatiza o processo de leitura e extração de dados de relatórios de folha de pagamento em formato PDF. A solução converte múltiplos arquivos de uma pasta em uma única base de dados consolidada em formato CSV, pronta para análise, geração de relatórios de BI ou integração com outros sistemas.

O script foi projetado para lidar com o layout específico de um sistema de folha de pagamento, mas pode ser adaptado para outros formatos com ajustes nos padrões de extração (Expressões Regulares).

## 💡 Problema Resolvido

O departamento de RH e Finanças frequentemente lida com dezenas ou centenas de relatórios de folha de pagamento em PDF. A extração manual desses dados para análise é um processo:
- **Extremamente Lento e Repetitivo:** O processo manual de preenchimento e conferência dos dados podia levar dias de trabalho. Com a automação, essa tarefa é concluída em segundos
- **Propenso a erros:** A digitação manual pode levar a incosistências nos dados, compromentendo a confiabilidade das análises.
- **Não escalável:** Torna-se inviável com o crescimento do número de funcionários.

Este script resolve esses problemas ao fornecer uma solução automatizada, rápida e precisa.

## ✨ Funcionalidades Principais

- **Processamento em Lote:** Lê todos os arquivos PDF de um diretório especificado.
- **Extração Inteligente:** Utiliza Expressões Regulares (Regex) para extrair informações estruturadas de texto não estruturado, como:
    - Dados do funcionário (Nome, CPF, Cargo, Data de Admissão).
    - Informações do cabeçalho (Competência, Tipo de Cálculo, Departamento).
    - Verbas de pagamento (Proventos e Descontos).
    - Totais e bases de cálculo (INSS, FGTS, IRRF).
- **Padronização e Limpeza:**
    - Converte valores monetários para o formato numérico correto.
    - Padroniza os nomes das verbas (rubricas) usando um dicionário de mapeamento, garantindo a consistência entre diferentes folhas de pagamento.
- **Saída Estruturada:** Consolida todos os dados extraídos em um único DataFrame do Pandas e o exporta como um arquivo CSV.

## 🛠️ Tecnologias Utilizadas

- **Python 3.x**
- **Pandas:** Para manipulação e estruturação dos dados.
- **PDFPlumber:** Para a extração de texto de arquivos PDF.
- **Regex (módulo `re`):** Para o parsing e a extração de dados específicos do texto.

## 🚀 Como Usar

1.  **Pré-requisitos:**
    Certifique-se de ter o Python instalado. Em seguida, instale as bibliotecas necessárias:
    ```bash
    pip install pandas pdfplumber
    ```

2.  **Estrutura de Pastas:**
    Crie uma pasta principal para o seu projeto e, dentro dela, uma subpasta chamada `FOPAG` (ou o nome que preferir, mas lembre-se de ajustar no código).
    ```
    /seu-projeto/
    |-- script_folha.py
    |-- /FOPAG/
        |-- folha_janeiro.pdf
        |-- folha_fevereiro.pdf
        |-- ...
    ```

3.  **Execução:**
    Coloque todos os seus arquivos PDF de folha de pagamento na pasta `FOPAG`. Em seguida, execute o script Python:
    ```bash
    python script_folha.py
    ```

4.  **Saída:**
    Ao final da execução, um arquivo chamado `BASE_FOPAAG_STAGGIN.csv` será gerado na pasta principal do projeto, contendo todos os dados consolidados.

## 🔧 Estrutura do Código e Customização

O código é modularizado em funções para facilitar a manutenção e o entendimento.

-   `limpar_valor(valor_str)`: Converte uma string monetária (ex: "1.234,56") em um número float (1234.56).
-   `limpar_nome_coluna(codigo, descricao)`: **O coração da lógica de negócio.** Esta função padroniza o nome de cada verba (provento ou desconto). Para customizar, basta adicionar novos códigos e descrições ao dicionário `mapeamento_original`. Esta é a principal área a ser modificada para adaptar o script a outros layouts de folha.
-   `extrair_info_base(texto_pagina)`: Extrai dados globais do documento, como a competência (mês/ano).
-   `processar_pdfs_na_pasta(pasta_path)`: A função principal que orquestra todo o processo: localiza os PDFs, itera sobre eles, extrai os blocos de cada funcionário e organiza os dados para a criação do DataFrame final.

## ⚖️ Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.
