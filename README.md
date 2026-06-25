# Organização de Estruturas de Arquivos

## Grupo

- RAFAEL BARRIONUEVO DE SOUZA
- LETICIA MENDONCA DOS SANTOS
- MARIA LUIZA BERTIN DOS SANTOS
- ERICK RICARDO BAIAO BATISTA PEREIRA
- GUSTAVO ANDRADE DE SOUZA
- MATHEUS ALEXANDRE FERREIRA LEITE

Este repositório está dividido em duas partes principais:

- `P1`: exercícios com busca binária, índice ordenado de CEP e ordenação externa.
- `P2`: processamento de CSV da Covid e indexação de CEP com Árvore B.

> Os programas esperam que os arquivos de entrada estejam na mesma pasta do executável. Por isso, antes de executar cada etapa, entre na pasta indicada com `cd`.

## P1

### Busca binária em CEP ordenado

Pasta: `P1/Busca-Binária`

Arquivo principal: `01buscaBinariaCEP.c`

Este programa busca um CEP diretamente no arquivo `cep_ordenado.dat`, usando busca binária.

```powershell
cd P1\Busca-Binária
gcc 01buscaBinariaCEP.c -o buscaBinariaCEP.exe
.\buscaBinariaCEP.exe 69907060
```

Arquivo necessário nesta pasta:

- `cep_ordenado.dat`

### Índice ordenado de CEP

Pasta: `P1/Indice-Ordenado-CEP`

Arquivos:

- `02CriaEOrdenaIndice.c`: cria o arquivo `indice.dat` a partir de `cep.dat`.
- `02IndiceBusca.c`: consulta CEPs usando o índice gerado.

```powershell
cd P1\Indice-Ordenado-CEP

gcc 02CriaEOrdenaIndice.c -o criaIndice.exe
.\criaIndice.exe

gcc 02IndiceBusca.c -o buscaIndice.exe
.\buscaIndice.exe 69907060
```

Arquivos necessários nesta pasta:

- `cep.dat`
- `indice.dat`, gerado pelo programa `02CriaEOrdenaIndice.c`

### Ordenação externa

Pasta: `P1/Ordenação-Externa-Diversos-Blocos`

Arquivo principal: `03ExternalMerge.c`

```powershell
cd P1\Ordenação-Externa-Diversos-Blocos
gcc 03ExternalMerge.c -o externalMerge.exe
.\externalMerge.exe
```

Arquivo necessário nesta pasta:

- `cep.dat`

## P2

### Processamento de CSV da Covid

Pasta: `P2/Processamento-de-CSV-Covid`

Arquivos:

- `01ProcessamentoCSVCovid.c`
- `CSVParser.c`
- `CSVParser.h`

O programa lê `owid-covid-data.csv` e calcula o total de casos e mortes na América do Sul.

```powershell
cd P2\Processamento-de-CSV-Covid
gcc 01ProcessamentoCSVCovid.c CSVParser.c -o 01ProcessamentoCSVCovid.exe
.\01ProcessamentoCSVCovid.exe
```

Arquivo necessário nesta pasta:

- `owid-covid-data.csv`

### CEP indexado com Árvore B

Pasta: `P2/CEP-indexado-com-arvore-B`

Esta parte usa `cep.dat` para criar dois subconjuntos de dados, indexar o primeiro em uma Árvore B e verificar quais CEPs do segundo subconjunto também existem no primeiro.

Arquivos:

- `01AleatorizaCep.c`: cria `cep-1.dat` e `cep-2.dat` a partir de `cep.dat`.
- `02CriaArvoreBcomCep1.c`: cria/atualiza `arvore.dat`, indexando os CEPs de `cep-1.dat`.
- `03JoinArvoreBcomCep.c`: lê `cep-2.dat`, busca cada CEP em `arvore.dat` e grava os encontrados em `cep-join.dat`.
- `ArvoreB.c` e `ArvoreB.h`: implementação da Árvore B usada pelos programas principais.
- `ArvoreB-Busca.c`: consulta manualmente um CEP específico em `arvore.dat`.

#### Fluxo principal

```powershell
cd P2\CEP-indexado-com-arvore-B

# Cria cep-1.dat e cep-2.dat
gcc 01AleatorizaCep.c -o 01AleatorizaCep.exe
.\01AleatorizaCep.exe

# Cria a Árvore B usando cep-1.dat
gcc 02CriaArvoreBcomCep1.c ArvoreB.c -o 02CriaArvoreBcomCep1.exe
.\02CriaArvoreBcomCep1.exe

# Gera cep-join.dat com os CEPs de cep-2.dat encontrados na Árvore B
gcc 03JoinArvoreBcomCep.c ArvoreB.c -o 03JoinArvoreBcomCep.exe
.\03JoinArvoreBcomCep.exe
```

Arquivos necessários nesta pasta:

- `cep.dat`, antes de executar `01AleatorizaCep.c`
- `cep-1.dat` e `cep-2.dat`, gerados por `01AleatorizaCep.c`
- `arvore.dat`, gerado por `02CriaArvoreBcomCep1.c`

#### BuscaCEP: testar um CEP específico

O arquivo `ArvoreB-Busca.c` pode ser compilado para consultar um CEP diretamente em `arvore.dat`.

```powershell
cd P2\CEP-indexado-com-arvore-B
gcc ArvoreB-Busca.c -o buscaCEP.exe
.\buscaCEP.exe 69907060
```

Antes de testar a busca, `arvore.dat` precisa existir. Para isso, execute primeiro:

```powershell
.\02CriaArvoreBcomCep1.exe
```

O retorno do `buscaCEP.exe` indica a posição do CEP dentro de `cep-1.dat`:

- `-1`: CEP não encontrado.
- `0`: CEP encontrado na primeira posição.
- Qualquer número positivo, como `310295`: CEP encontrado nessa posição do arquivo.

Exemplos:

```powershell
.\buscaCEP.exe 69907060
.\buscaCEP.exe 23013480
.\buscaCEP.exe 12280102
.\buscaCEP.exe 00000000
```
