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
4. Agora podemos adicionar
 
 Com isso o ambiente de trabalho está preparado para receber os dados e o VEP.
 
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
  -i /content/drive/Shareddrives/T3-2021/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz \
  -o WP312.filtered.vcf.tsv \
  --dir_cache /content/drive/Shareddrives/T3-2021/ \
  --fasta /content/drive/Shareddrives/T3-2021/homo_sapiens_refseq/Homo_sapiens_assembly19.fasta \
  --cache --offline --assembly GRCh37 --refseq  \
	--pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \
  --fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
  --individual all
```



```bash
< Imagina fazer o próximo trabalho todo no github>
 -----------------------------
     \   ^__^
      \  (oo)\_______
         (__)\       )\/\
            ||----w |
            ||     ||

```
