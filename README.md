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
- Analisar o dataset completo (~19.000 casos)
- Aplicar sistema de scoring para identificar casos de crédito consignado
- Gerar arquivo `output/output.xlsx` com os IDs dos casos filtrados

**Saída esperada:** ~8.000 casos de crédito consignado identificados

#### Passo 2: Extrair Informações Estruturadas (Entrega 6)

```bash
cd entrega6
jupyter notebook entrega6_v2.ipynb
```

Execute as células sequencialmente:

**Seções principais:**
1. **Seções 1-8**: Carregamento de dados e funções de extração
2. **Seção 9**: Extração básica (7 campos obrigatórios) - gera `output.xlsx`
3. **Seção 10**: Execução da extração básica
4. **Seção 10.1** (Opcional): Extração expandida com valores morais/materiais - gera `output_expandido.xlsx`
5. **Seção 11**: Análise estatística descritiva e visualizações
6. **Seção 12**: Geração de relatório consolidado - gera `relatorio.xlsx`
7. **Seção 13**: Verificação de sanidade e resumo final

**Campos extraídos (básicos - 7 colunas):**
- `cd_atendimento`: ID do caso
- `nome_empresa`: Nome da empresa no polo passivo
- `cnpj`: CNPJ válido (14 dígitos)
- `valor_causa`: Valor da causa em reais
- `dt_distribuicao`: Data de distribuição (YYYY-MM-DD)
- `tipo_vara`: JE (Juizado Especial) ou G1 (Vara Comum)
- `uf`: Unidade Federativa

**Campos adicionais (extração expandida - 13 colunas):**
- Todos os campos básicos acima
- `valor_moral`: Valor de danos morais extraído
- `valor_material`: Valor de danos materiais extraído
- `has_minimo`: Flag "não inferior a"
- `has_em_dobro`: Flag "em dobro"
- `has_ate`: Flag "até"
- `evidencia`: Trecho de texto evidenciando a extração

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

**Objetivo:** Extrair dados relevantes dos casos de crédito consignado identificados e gerar análises estatísticas.

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

4. **Extração de Valores de Danos Morais e Materiais:**
   - Busca contextual em janelas de ±100 caracteres ao redor de "danos morais"/"danos materiais"
   - Extração de valores em formato brasileiro (R$ 1.234,56)
   - Detecção de flags qualificadores: "não inferior a", "em dobro", "até"
   - Armazenamento de evidência textual para validação

5. **Extração de Data de Distribuição:**
   - Suporte a múltiplos formatos (DD/MM/YYYY, YYYY-MM-DD)
   - Priorização de datas próximas a "distribuição" ou "autuação"
   - Normalização para formato ISO (YYYY-MM-DD)

6. **Classificação de Tipo de Vara:**
   - Busca por "juizado especial" ou "JECC"
   - Classificação binária: JE (Juizado Especial) ou G1 (Vara Comum)

7. **Extração de UF:**
   - Busca por tokens de 2 letras maiúsculas
   - Validação contra lista de UFs brasileiras

**Análises Estatísticas:**
- Estatísticas descritivas (count, mean, median, std, min, p25, p75, max)
- Histogramas de distribuição de valores morais e materiais
- Boxplots de valores por tipo de vara
- Análise de flags e qualificadores textuais

**Tratamento de Dados:**
- Limpeza de caracteres de controle para compatibilidade com Excel
- Normalização de acentos e espaços em branco
- Validação de tipos de dados (float para valores, string para datas)
- Aplicação automática de limpar_para_excel() em todos os exports

**Resultados:**
- Taxa de extração CNPJ: ~70%
- Taxa de extração Valor Causa: ~80%
- Taxa de extração Data: ~75%
- Taxa de extração UF: ~95%
- Taxa de extração Tipo Vara: 100%
- Taxa de extração Valores Morais: ~60%
- Taxa de extração Valores Materiais: ~40%

### Arquivos Gerados

1. **entrega5/output.xlsx**: Lista de IDs dos casos de crédito consignado
2. **entrega6/output.xlsx**: Dados estruturados extraídos (7 colunas obrigatórias)
3. **entrega6/output_expandido.xlsx**: Dados expandidos com valores morais/materiais (13 colunas)
4. **entrega6/relatorio.xlsx**: Relatório consolidado com 3 abas:
   - **Resumo**: métricas gerais, percentuais, médias, contagens por vara/UF
   - **Descritivo**: estatísticas completas de danos morais e materiais
   - **Logs_Sanidade**: verificações de qualidade (CNPJs vazios, datas faltantes, etc.)
5. **entrega6/histogramas_danos.png**: Visualização da distribuição de valores
6. **entrega6/boxplots_danos_vara.png**: Comparação de valores por tipo de vara
7. **dataset_filtrado.xlsx**: Dataset completo filtrado (todas as colunas originais, apenas casos de crédito consignado)

---

## Tecnologias Utilizadas

- **Python 3.x**: Linguagem principal
- **pandas**: Manipulação de dados tabulares
- **numpy**: Operações numéricas e transformações
- **re (regex)**: Extração de padrões de texto
- **unicodedata**: Normalização de caracteres
- **unidecode**: Remoção de acentos
- **openpyxl**: Leitura/escrita de arquivos Excel
- **matplotlib**: Geração de gráficos e visualizações
- **seaborn**: Visualizações estatísticas avançadas
- **Jupyter Notebook**: Ambiente de desenvolvimento interativo

---

## Próximos Passos

- Validação dos dados extraídos contra gabarito oficial
- Modelos inferenciais (regressão OLS) para prever valores de danos
- Análise de correlação entre flags textuais e valores deferidos
- Refinamento das técnicas de extração baseado em feedback
- Análise de padrões e tendências nos casos de crédito consignado por região e tipo de vara
- Estudo de jurisprudência com base nos valores morais/materiais extraídos

---

## Autores

Projeto desenvolvido para a disciplina de Ciência de Dados Aplicada ao Direito II.

---

## Licença

Este projeto é de uso acadêmico.