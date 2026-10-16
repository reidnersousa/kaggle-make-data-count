# 🥉 Make Data Count - Finding Data References

Este repositório contém a solução desenvolvida pela equipe **CIIA** para a competição internacional do Kaggle [**"Make Data Count - Finding Data References"**](https://www.kaggle.com/competitions/make-data-count-finding-data-references).

O objetivo do desafio foi identificar referências a bases de dados em artigos científicos e classificar como esses dados foram mencionados.

---

## 📊 Resultado Final

- 🥉 **Medalha de Bronze**
- 📍 **Colocação:** 127º lugar de 1.282 equipes
- 📈 **Métrica:** F1-Score = **0.50185**

---

## 📝 Visão Geral do Desafio

A tarefa consistia em extrair dois tipos principais de identificadores de datasets em textos científicos:

- **DOIs (Digital Object Identifier)**
Exemplo: `10.5281/zenodo.12345`
- **Accession IDs**
Identificadores específicos de bancos de dados:
    - `PRJNA12345`
    - `GSE9876`
    - entre outros

### 🎯 Classificação

Cada dataset identificado deveria ser classificado em:

- **Primary** → Dados gerados pelo próprio artigo
- **Secondary** → Dados reutilizados de fontes externas

---

## 🛠️ Arquitetura da Solução

O pipeline foi estruturado em **5 etapas principais**:

### 1. Parsing e Limpeza de Texto

- Conversão de formatos **PDF/XML → TXT**
- Normalização de Unicode
- Correção de identificadores quebrados (ex: DOIs com espaços)
- Remoção de ruídos:
    - Cabeçalhos
    - Rodapés

---

### 2. Segmentação de Conteúdo

- Identificação da seção de **referências**
- Uso de âncoras como:
    - `"Data availability"`
- Foco em trechos com maior probabilidade de conter datasets

---

### 3. Extração de Identificadores (Regex)

- Uso de **expressões regulares especializadas**
- Suporte a múltiplos padrões:
    - NCBI
    - BioProject
    - GEO
    - ProteomeXchange
- Normalização para formato canônico (principalmente DOIs)

---

### 4. Classificação com LLM

- Modelo: **Qwen2.5 32B AWQ**
- Inferência via **vLLM**

### Estratégias utilizadas:

- **Prompt Engineering**
    - Regras baseadas em repositórios conhecidos:
        - Dryad
        - Zenodo
        - Figshare
- **Few-shot Learning**
    - Exemplos contextuais para diferenciar:
        - autoria vs. citação

---

### 5. Pós-filtragem

- Remoção de falsos positivos:
    - padrões semelhantes a artigos científicos
- **Context Rescue**
    - Preserva identificadores próximos de palavras-chave como:
        - `dataset`
        - `repository`

---

## 🚀 Principais Aprendizados

- 🔀 **Hibridismo é essencial**
Combinar:
    - Regex + heurísticas
    - LLMs
    → Melhor resultado do que usar isoladamente
- 🧹 **Engenharia de Dados importa muito**
Pequenas melhorias em limpeza aumentaram significativamente o **recall**
- ⚖️ **Uso estratégico de LLMs**
Aplicar modelos pesados apenas em casos ambíguos:
    - melhora performance
    - reduz custo computacional

---

## 🧰 Tecnologias Utilizadas

- **Linguagem:** Python
- **LLM:** Qwen2.5 32B AWQ
- **Inferência:** vLLM
- **Processamento de Texto:**
    - Regex
    - Unicodedata
