# Prática de Versionamento de Dados e Modelos com DVC

Este repositório contém a implementação da prática de versionamento de dados e modelos de Machine Learning utilizando Git e DVC para o problema de classificação de chamados de suporte.

## 1. Pré-requisitos

Para executar o projeto, é necessário ter instalado:
- Git
- Python (>= 3.10)
- DVC (>= 3.0)

Instalação do DVC via pip:
```bash
pip install dvc
```

## 2. Configuração do Armazenamento DVC

Os dados e artefatos de modelo estão versionados no DVC com armazenamento local na pasta irmã `../dvc-storage`.

Para configurar o armazenamento remoto localmente:
```bash
dvc remote add -d storage ../dvc-storage
```

Para listar o remoto configurado:
```bash
dvc remote list
```

## 3. Comandos de Restauração e Treinamento

Como os arquivos de dados e o modelo em si não sobem para o GitHub, para restaurar qualquer versão basta alternar a tag no Git e puxar os arquivos pelo DVC:

### Restaurar versão inicial (r1):
```bash
git checkout modelo-r1
dvc pull
```

### Restaurar versão corrigida (r2):
```bash
git checkout modelo-r2
dvc pull
```

### Executar o treinamento:
```bash
python3 src/train.py <nome-do-modelo>
```
Exemplo:
```bash
python3 src/train.py chamados-r2
```
