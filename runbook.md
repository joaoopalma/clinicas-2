# Projeto Clínicas 2 - Runbook

## 0) Pré-requisitos

- **Python 3.10+**

    - **Windows:** baixe em python.org e marque “Add Python to PATH”.

    - **macOS:** use o instalador do site ou brew install python.

- **Git** instalado

- A base `dataset_clinica2025.csv`

## 1) Estrutura do repositório

```bash
clinica-2/
├─ data/
│  └─ dataset_clinica2025.csv   # eles colocam aqui o CSV
├─ output/                      # a função salva aqui (opcional)
├─ analise.ipynb                # seu notebook
├─ requirements.txt
├─ .gitignore
└─ README.md
```

## 2) Passo a passo - Windows 

```bash
# 0) Clonar
git clone https://github.com/<sua-org>/<seu-repo>.git
cd <seu-repo>

# 1) Criar ambiente virtual
python -m venv venv

# 2) Ativar
.\venv\Scripts\Activate.ps1

# 3) Instalar dependências
pip install -r requirements.txt

# 4) Abrir o Jupyter (ou VS Code)
jupyter notebook
# ou
code .
```

Para **sair do ambiente** depois:

```bash
deactive
```

## 3) Passo a passo — macOS / Linux (Terminal)

```bash
# 0) Clonar o repositório
git clone https://github.com/<sua-org>/<seu-repo>.git
cd <seu-repo>

# 1) Criar ambiente virtual
python3 -m venv venv

# 2) Ativar o ambiente
source venv/bin/activate

# 3) Instalar dependências
pip install -r requirements.txt

# 4) Colocar o CSV na pasta data/

# 5) Abrir o Jupyter (ou VS Code)
jupyter notebook
# ou
code .

```

Para **sair do ambiente** depois:

```bash
deactive
```

## 4) Como rodar o notebook

1 - Abra `analise.ipynb`

2 - Confirme/ajuste a variável `INPUT_CSV` (caminho do arquivo na pasta data/).

3 - **Run all**

4 - As saídas serão criadas em:
    
    - `output/output.xlsx` (Entrega 5 — filtro “crédito consignado”, apenas `cd_atendimento`);

    - `output/output_colunas.xlsx` (Entrega 6 — `cd_atendimento`, `nome_empresa`, `cnpj`, `valor_causa`, `dt_distribuicao`, `tipo_vara`, `uf`).

## 5) requirements.txt

```bash
pandas>=2.0
numpy>=1.24
openpyxl>=3.1       # necessário para salvar .xlsx
jupyter>=1.0
```

## 6) .gitignore (essencial)

```bash
venv/
__pycache__/
.ipynb_checkpoints/
output/*.xlsx
*.xlsx
.env
```
