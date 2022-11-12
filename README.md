# 🐮 Mini trabalho de somático 🐮 <!-- omit in toc -->

# VEPSomatico
Como utilizar VEP -ensembl 105.0 em um VCF somático no Colab


- [Introdução](#introdução)
- [Preparação do ambiente de trabalho](#preparação-do-ambiente-de-trabalho)
- [Utilizando o VEP](#Utilizando-o-vep)

# Introdução

Ensembl Variant Effect Predictor ou VEP te ajuda a determinar os efeitos das variantes encontradas nos dados a serem analisados; sendo eles genes, transcritos ou sequências proteicas. Precisando apenas das coordenadas das variantes e a mudança nucleica que foi observada.

O processo para utilização do VEP segue por:
1. Montar o drive no Colab
2. Instalar o VEP
3. Fazer a anotação das variantes

# Preparação do ambiente de trabalho

Começar criando um novo notebook no seu [Google Colab](https://colab.research.google.com/), uma vez no colab:
1. Montar o drive no ambiente de trabalho, que permite criar e gerenciar os dados:
  ```
  from google.colab import drive
  drive.mount('/content/drive')
  ```
2. Agora montamos um diretório específico para os documentos gerados. Utilizamos o "%%bash" para indicar ao Colab
   que este código está em bash e utilizamos o "%cd" para fixar esse diretório como o diretório principal:
  ```
  %%bash
  mkdir vepsomatico
  %cd somatico
  ```
3. Para confirmar que o diretório que será utilizado é o que queremos, podemos usar:
  ```
  %%bash
  pwd
  ```
 
 Com isso o ambiente de trabalho está preparado para receber os dados e o VEP.
 
# Utilizando o VEP



```bash
< Imagina fazer o próximo trabalho todo no github>
 -----------------------------
     \   ^__^
      \  (oo)\_______
         (__)\       )\/\
            ||----w |
            ||     ||

```
