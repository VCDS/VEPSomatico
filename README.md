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
  ```python
  from google.colab import drive
  drive.mount('/content/drive')
  ```
2. Agora montamos um diretório específico para os documentos gerados. Utilizamos o `%%bash` para indicar ao Colab
   que este código está em bash e utilizamos o `%cd` para fixar esse diretório como o diretório principal:
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
 4. Vamos instalar algumas módulos que serão utilizados mais adiante:
 ```
 import pandas as pd
 import csv
 ```
 
 Com isso o ambiente de trabalho está preparado para receber o VEP.
 
# Utilizando o VEP

Agora que temos nosso ambiente de trabalho preparado, podemos utilizar o próximo comando para instalar o VEP. Em ordem, cada
linha do comando pode ser interpretada da seguinte forma:
1. Instalação dos pacotes necessários para utilizar o VEP
2. Fazer download do VEP na versão esembl-vep 105.0
3. Descompactar o documento baixado
4. As duas últimas linhas indicam ao colab para entrar no diretório do VEP onde foi descompactado e fazer a instalação

```
%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 
```
Pronto, instalação comcluida. Podemos utilizar o código abaixo para testar se tudo ocorreu como o planejado:
  ```
  %%bash
  cd ensembl-vep-105.0
  ./vep 
  ```

Por fim, com o ambiente de trabalho preparado e o VEP instalado, podemos fazer a anotação das variantes com o seguinte comando:
```
%%bash
/ensembl-vep-105.0/vep  \
  --fork 4 \
  -i /caminho_documento_vcf/nome_documento_vcf.vcf.gz \
  -o nome_desejado.filtered.vcf.tsv \
  --dir_cache /caminho_dir_cashe/ \
  --fasta /caminho_documento_fasta/nome_documento_fasta.fasta \
  --cache --offline --assembly GRCh37 --refseq  \
	--pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \
  --fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
  --individual all
```
Vamos interpretar os caminhos e nomes necessários para preencher corretamente o comando acima:
- em `-i` o `caminho_documento_vcf` se refere ao caminho do diretório onde o arquivo vcf a ser analisado está localoizado e `nome_documento_vcf`
  se refere ao nome do documento VCF que será analisado
- em `-o` o `nome_desejado` se refere ao nome que você deseja utilizar no output do arquivo filtrado gerado pelo VEP
- em `--dir_cache` o 'caminho_dir_cashe' se refere ao caminho do diretório cashe
- em `--fasta` o `caminho_documento_fasta` é o caminho para o diretório onde está localizado o documento fasta
As opções `--cache`, `--fields` e `--individual` você deve preencher de acordo com o que você deseja gerar no seu output e a documentação para
fazer a melhor escolha possível para cada caso pode ser encontrada neste [link](https://www.ensembl.org/info/docs/tools/vep/script/vep_filter.html)

Agora podemos visualizar o arquivo gerado pelo VEP, que faremos através de uma tabela gerada pelo pandas onde:
1. em `caminho_para_vcf_tsv` indica o caminho do diretório para o arquivo `vcf.tsv`
2. em `nome_arquivo_vcf_tsv` se refere ao nome do arquivo `vcf.tsv` que foi gerado
```
tabela = pd.read_csv('/caminho_para_vcf_tsv/nome_arquivo_vcf_tsv.filtered.vcf.tsv', sep='\t', skiprows=38)
df = pd.DataFrame(tabela)
df
```
Exemplo de uma tabela gerada no final do processo:

```
```
|index|\#Uploaded\_variation|Location|Allele|Existing\_variation|HGVSc|HGVSp|SYMBOL|Consequence|IND|ZYG|Amino\_acids|CLIN\_SIG|PolyPhen|SIFT|VARIANT\_CLASS|FREQS|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|1\_874643\_C/A|1:874643|A|-|NM\_001385640\.1:c\.1058-9C\>A|-|SAMD11|splice\_polypyrimidine\_tract\_variant,intron\_variant|WP312|HET|-|-|-|-|SNV|-|
|1|1\_874647\_C/A|1:874647|A|rs1336064632|NM\_001385640\.1:c\.1058-5C\>A|-|SAMD11|splice\_polypyrimidine\_tract\_variant,splice\_region\_variant,intron\_variant|WP312|HET|-|-|-|-|SNV|-|
|2|1\_880620\_A/C|1:880620|C|rs1569931554|NM\_015658\.4:c\.2054-94T\>G|-|NOC2L|intron\_variant|WP312|HET|-|-|-|-|SNV|-|
|3|1\_894491\_C/A|1:894491|A|-|NM\_015658\.4:c\.27-30G\>T|-|NOC2L|intron\_variant|WP312|HET|-|-|-|-|SNV|-|
|4|1\_907758\_A/G|1:907758|G|rs757863610,COSV58020841|NM\_001367552\.1:c\.992A\>G|NP\_001354481\.1:p\.Glu331Gly|PLEKHN1|missense\_variant|WP312|HET|E/G|-|-|-|SNV|1KG\_ALL:G:NA|
```
Chega!

```bash
< Se eu ganhar minha cerveja vai para o professor! 😜>
 -----------------------------
     \   ^__^
      \  (oo)\_______
         (__)\       )\/\
            ||----w |
            ||     ||

```
