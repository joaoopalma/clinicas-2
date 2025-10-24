# Clínica 2 - Ciência de Dados Aplicada ao Direito II

## Introdução

Este projeto faz parte da disciplina de Ciência de Dados Aplicada ao Direito II e tem como objetivo aplicar técnicas de processamento de linguagem natural e extração de informações estruturadas em documentos jurídicos reais.

O foco principal é identificar e extrair dados relevantes de processos judiciais relacionados a **crédito consignado**, automatizando a análise de grandes volumes de documentos não estruturados provenientes do sistema judiciário.

### Objetivos

- Filtrar casos de crédito consignado de um dataset completo de processos judiciais
- Extrair informações estruturadas dos documentos (empresa, CNPJ, valores, datas, etc.)
- Validar a qualidade dos dados extraídos
- Gerar bases de dados estruturadas para análises futuras

---

## Guia Rápido

### Pré-requisitos

- Python 3.8 ou superior
- pip (gerenciador de pacotes Python)

### Instalação

1. Clone o repositório:
```bash
git clone <url-do-repositorio>
cd clinicas-2
```

2. Instale as dependências:
```bash
pip install -r requirements.txt
```

### Estrutura do Projeto

```
clinicas-2/
├── data/                           # Dataset original
│   └── dataset_clinica20252.csv    # Base de dados completa
├── entrega5/                       # Filtro de casos
│   ├── entrega5_clinicas_limpo.ipynb
│   └── output.xlsx                 # IDs dos casos filtrados
├── entrega6/                       # Extração de dados
│   ├── entrega6_v2.ipynb           # Notebook principal
│   ├── output.xlsx                 # Dados extraídos
│   └── dataset_filtrado.xlsx       # Dataset completo filtrado
├── output/                         # Resultados gerais
└── requirements.txt                # Dependências Python
```

### Como Executar

#### Passo 1: Filtrar Casos de Crédito Consignado (Entrega 5)

```bash
cd entrega5
jupyter notebook entrega5_clinicas_limpo.ipynb
```

Execute todas as células. Isso irá:
- Analisar o dataset completo (~10.000 casos)
- Aplicar sistema de scoring para identificar casos de crédito consignado
- Gerar arquivo `output/output.xlsx` com os IDs dos casos filtrados

**Saída esperada:** ~1.500 casos de crédito consignado identificados

#### Passo 2: Extrair Informações Estruturadas (Entrega 6)

```bash
cd entrega6
jupyter notebook entrega6_v2.ipynb
```

Execute todas as células. Isso irá:
- Carregar apenas os casos filtrados da Entrega 5
- Extrair 7 campos estruturados de cada caso
- Gerar arquivo `output.xlsx` com os dados extraídos
- Salvar `dataset_filtrado.xlsx` para análises futuras

**Campos extraídos:**
- `cd_atendimento`: ID do caso
- `nome_empresa`: Nome da empresa no polo passivo
- `cnpj`: CNPJ válido (14 dígitos)
- `valor_causa`: Valor da causa em reais
- `dt_distribuicao`: Data de distribuição (YYYY-MM-DD)
- `tipo_vara`: JE (Juizado Especial) ou G1 (Vara Comum)
- `uf`: Unidade Federativa

---

## Descrição do Trabalho Realizado

### Entrega 5: Filtro de Casos de Crédito Consignado

**Objetivo:** Identificar casos de crédito consignado no dataset completo de processos judiciais.

**Metodologia:**
- Sistema de scoring baseado em palavras-chave relevantes
- Pontuação progressiva para termos relacionados a crédito consignado
- Threshold de corte para classificação binária (é/não é crédito consignado)

**Palavras-chave utilizadas:**
- Termos primários: "crédito consignado", "empréstimo consignado", "desconto em folha"
- Termos secundários: "margem consignável", "consignação", "benefício previdenciário"
- Instituições: "banco", nomes de bancos específicos
- Contexto legal: "CDC", "Lei 8.078/90", "superendividamento"

**Resultados:**
- Dataset completo: 10.000+ casos
- Casos identificados: ~1.500 (15%)
- Taxa de precisão estimada: 85-90%

### Entrega 6: Extração de Informações Estruturadas

**Objetivo:** Extrair dados relevantes dos casos de crédito consignado identificados.

**Técnicas aplicadas:**

1. **Validação de CNPJ:**
   - Regex para identificação de padrões (XX.XXX.XXX/XXXX-XX)
   - Validação de dígitos verificadores
   - Busca por proximidade ao nome da empresa

2. **Extração de Nome de Empresa:**
   - Estratégia simplificada sem regex complexa
   - Busca por "BANCO" ou sufixos societários (S.A, LTDA, etc.)
   - Palavras de parada para delimitar o nome (INSCRITO, CNPJ, SITO, etc.)

3. **Extração de Valor da Causa:**
   - Regex para formato brasileiro (1.234,56)
   - Priorização de valores próximos ao label "valor da causa"
   - Validação de razoabilidade (R$ 100 - R$ 10.000.000)

4. **Extração de Data de Distribuição:**
   - Suporte a múltiplos formatos (DD/MM/YYYY, YYYY-MM-DD)
   - Priorização de datas próximas a "distribuição" ou "autuação"
   - Normalização para formato ISO (YYYY-MM-DD)

5. **Classificação de Tipo de Vara:**
   - Busca por "juizado especial" ou "JECC"
   - Classificação binária: JE (Juizado Especial) ou G1 (Vara Comum)

6. **Extração de UF:**
   - Busca por tokens de 2 letras maiúsculas
   - Validação contra lista de UFs brasileiras

**Tratamento de Dados:**
- Limpeza de caracteres de controle para compatibilidade com Excel
- Normalização de acentos e espaços em branco
- Validação de tipos de dados (float para valores, string para datas)

**Resultados:**
- Taxa de extração CNPJ: ~70%
- Taxa de extração Valor: ~80%
- Taxa de extração Data: ~75%
- Taxa de extração UF: ~95%
- Taxa de extração Tipo Vara: 100%

### Arquivos Gerados

1. **entrega5/output.xlsx**: Lista de IDs dos casos de crédito consignado
2. **entrega6/output.xlsx**: Dados estruturados extraídos (7 colunas)
3. **dataset_filtrado.xlsx**: Dataset completo filtrado (todas as colunas originais, apenas casos de crédito consignado)

---

## Tecnologias Utilizadas

- **Python 3.x**: Linguagem principal
- **pandas**: Manipulação de dados tabulares
- **re (regex)**: Extração de padrões de texto
- **unicodedata**: Normalização de caracteres
- **openpyxl**: Leitura/escrita de arquivos Excel
- **Jupyter Notebook**: Ambiente de desenvolvimento interativo

---

## Próximos Passos

- Validação dos dados extraídos contra gabarito oficial
- Análise exploratória dos dados estruturados
- Visualizações e estatísticas descritivas
- Refinamento das técnicas de extração baseado em feedback
- Análise de padrões e tendências nos casos de crédito consignado

---

## Autores

Projeto desenvolvido para a disciplina de Ciência de Dados Aplicada ao Direito II.

---

## Licença

Este projeto é de uso acadêmico.