# Modelagem Preditiva de Internações em UTI em Casos de SRAG (2016–2018)

Repositório do projeto final da disciplina **Introdução à Ciência de Dados (ICD)**,
IFB – Campus Brasília.

**Autores:**  
- Davi Henrique Menezes da Cruz  
- Sabrina de Oliveira Souza  

## Objetivo

Desenvolver um modelo de **classificação** para estimar a probabilidade de
internação em **Unidade de Terapia Intensiva (UTI)** em pacientes com
**Síndrome Respiratória Aguda Grave (SRAG)**, utilizando dados do
**OpenDataSUS (2016–2018)**.

O foco é avaliar o impacto de fatores **demográficos, clínicos e regionais**
(sexo, raça/cor, idade, comorbidades e UF) sobre a necessidade de cuidados
intensivos.

## Dados

- Fonte: **OpenDataSUS – SRAG 2016, 2017 e 2018**  
- Total inicial: **131.687 registros e 112 colunas**  
- Após seleção de variáveis relevantes e limpeza: **117.403 registros**  
- Variável-alvo: `UTI_BINARIO`  
  - 1 = paciente internado em UTI  
  - 0 = paciente não internado em UTI   

Os arquivos CSV originais **não são versionados** neste repositório por
restrições de tamanho. O notebook realiza o download automático das bases
via `gdown`.

## Metodologia

1. **Coleta e unificação das bases (2016–2018)**  
2. **Seleção de variáveis** demográficas, clínicas e regionais  
3. **Limpeza de dados**  
   - Tratamento de `NaN` e código 9 = "Ignorado"  
   - Conversão de datas e criação de variáveis temporais  
4. **Criação da variável-alvo `UTI_BINARIO`**  
5. **Codificação one-hot** de variáveis categóricas  
6. **Divisão treino/teste (80/20) com estratificação**  
7. **Modelo principal:** Regressão Logística  
8. **Avaliação inicial (limiar 0,5)**  
9. **Teste com SMOTE** para balanceamento das classes  
10. **Ajuste de limiar para 0,3**, priorizando Recall (sensibilidade)

## Resultados principais

- Acurácia inicial (limiar 0,5): **67,91%**  
- AUC-ROC: **0,6925**  
- Recall inicial (classe UTI=1): **27,00%** → modelo muito conservador  

Após ajuste do limiar para **0,3**:

- Recall: **76,57%**  
- Precisão: **45,28%**  
- Acurácia: **59,35%**

Em contexto de saúde pública, a prioridade é **minimizar falsos negativos**
(pacientes graves não identificados), portanto optou-se pelo modelo com
maior sensibilidade, mesmo com aumento de falsos positivos.

## Como executar

### Opção 1: Executar no Google Colab (Recomendado)

O notebook principal está disponível no Google Colab e pode ser executado diretamente:

🔗 **[Abrir no Google Colab](https://colab.research.google.com/drive/1rrep2ONRaZIT2fE197b7OBXz-phaDD8t?usp=sharing)**

O notebook realiza o download automático dos dados necessários via `gdown`.

### Opção 2: Executar localmente

1. Clone o repositório:

   ```bash
   git clone https://github.com/davih1660/icd-srag-uti-davi-sabrina.git
   cd icd-srag-uti-davi-sabrina
   ```

2. Instale as dependências:

   ```bash
   pip install -r requirements.txt
   ```

3. Abra o notebook:

   ```bash
   jupyter notebook notebooks/Trabalho_ICD_ajustado_sabrinaEdavi.ipynb
   ```

   Ou se preferir usar o JupyterLab:

   ```bash
   jupyter lab notebooks/Trabalho_ICD_ajustado_sabrinaEdavi.ipynb
   ```

   **Nota:** Os arquivos CSV originais não estão incluídos no repositório. O notebook realiza o download automático via `gdown` quando executado.

## Estrutura do Projeto

```
icd-srag-uti-davi-sabrina/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Trabalho_ICD_ajustado_sabrinaEdavi.ipynb
├── documentos/
│   └── ProjetoFinal_Artigo.pdf
├── data/                    # Dados CSV (não versionados)
├── figuras/                 # Visualizações exportadas
└── src/                     # Código fonte adicional (se necessário)
```

**Observação:** Os arquivos CSV em `data/` não são versionados no repositório devido ao tamanho. O notebook realiza o download automático quando necessário.

## Links Importantes

- 📁 **Repositório GitHub**: [https://github.com/davih1660/icd-srag-uti-davi-sabrina.git](https://github.com/davih1660/icd-srag-uti-davi-sabrina.git)
- 📓 **Notebook no Colab**: [https://colab.research.google.com/drive/1rrep2ONRaZIT2fE197b7OBXz-phaDD8t?usp=sharing](https://colab.research.google.com/drive/1rrep2ONRaZIT2fE197b7OBXz-phaDD8t?usp=sharing)
- 🎥 **Vídeo de Apresentação**: [https://drive.google.com/file/d/1fJdmW-6ihQ6T8imjdMsltCDMqeRon37s/view?usp=sharing](https://drive.google.com/file/d/1fJdmW-6ihQ6T8imjdMsltCDMqeRon37s/view?usp=sharing)

## Licença

Este projeto foi desenvolvido para fins acadêmicos no contexto da disciplina de Introdução à Ciência de Dados (ICD) do IFB – Campus Brasília.
