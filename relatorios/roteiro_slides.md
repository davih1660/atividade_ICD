# Roteiro de Slides - Apresentação do Projeto Final ICD

## Slide 1 – Título e Autores

**Título do slide:** Modelagem Preditiva de Internações em UTI em Casos de SRAG (2016–2018)

**Conteúdo:**
- Título do projeto
- Autores:
  - Davi Henrique Menezes da Cruz
  - Sabrina de Oliveira Souza
- Instituição: IFB – Campus Brasília
- Disciplina: Introdução à Ciência de Dados (ICD)

**Sugestão de fala:**
"Boa tarde/tarde. Apresentamos hoje o projeto final da disciplina de Introdução à Ciência de Dados, desenvolvido em dupla. Nosso trabalho tem como objetivo desenvolver um modelo preditivo para estimar a probabilidade de internação em UTI em pacientes com Síndrome Respiratória Aguda Grave, utilizando dados do OpenDataSUS dos anos de 2016 a 2018."

---

## Slide 2 – Problema e Objetivos

**Título do slide:** Problema e Objetivos

**Conteúdo:**
- **Problema:**
  - SRAG representa importante problema de saúde pública no Brasil
  - Identificação precoce de pacientes com maior risco de UTI é fundamental
  - Necessidade de otimizar recursos hospitalares e melhorar desfechos clínicos
- **Objetivo principal:**
  - Desenvolver modelo de classificação para prever internação em UTI
  - Avaliar impacto de fatores demográficos, clínicos e regionais
  - Priorizar sensibilidade (minimizar falsos negativos)

**Sugestão de fala:**
"A Síndrome Respiratória Aguda Grave é um problema significativo de saúde pública. Nosso objetivo é desenvolver um modelo que identifique precocemente quais pacientes com SRAG têm maior probabilidade de necessitar de internação em UTI. Isso é crucial para o planejamento de recursos hospitalares. Em contexto de saúde pública, priorizamos a sensibilidade do modelo, ou seja, minimizar falsos negativos, pois não identificar um paciente que precisa de UTI pode ter consequências graves."

---

## Slide 3 – Dados e Preparação

**Título do slide:** Dados e Preparação

**Conteúdo:**
- **Fonte:** OpenDataSUS – SRAG 2016, 2017 e 2018
- **Volume inicial:** 131.687 registros e 112 colunas
- **Após limpeza:** 117.403 registros
- **Variáveis selecionadas:**
  - Demográficas: sexo, raça/cor, idade, escolaridade
  - Clínicas: comorbidades (obesidade, cardiopatia, pneumopatia, metabólica), sintomas (febre, tosse, dispneia)
  - Regionais: UF
  - Temporais: ano, mês dos primeiros sintomas
- **Variável-alvo:** `UTI_BINARIO` (1 = internado em UTI, 0 = não internado)

**Sugestão de fala:**
"Utilizamos dados do OpenDataSUS referentes aos casos de SRAG notificados entre 2016 e 2018. Inicialmente tínhamos mais de 131 mil registros com 112 colunas. Após a seleção de variáveis relevantes e limpeza de dados, trabalhamos com aproximadamente 117 mil registros. Selecionamos variáveis demográficas, como sexo, raça e idade; clínicas, incluindo comorbidades e sintomas; e regionais, representadas pela UF. A variável-alvo é binária, indicando se o paciente foi internado em UTI ou não."

---

## Slide 4 – Análise Exploratória (EDA)

**Título do slide:** Análise Exploratória dos Dados

**Conteúdo:**
- **Distribuição da variável-alvo:**
  - Classe majoritária (não UTI): ~64,6%
  - Classe minoritária (UTI): ~35,4%
  - Dados desbalanceados
- **Principais insights:**
  - Distribuição por UF mostra variações regionais
  - Idade média dos pacientes internados em UTI
  - Prevalência de comorbidades entre casos de UTI
  - Padrões temporais (sazonalidade)
- **Tratamento realizado:**
  - Remoção de valores ausentes e códigos "Ignorado"
  - Codificação one-hot de variáveis categóricas
  - Normalização de variáveis numéricas

**Sugestão de fala:**
"A análise exploratória revelou que temos um problema de desbalanceamento de classes: aproximadamente 65% dos casos não foram internados em UTI, enquanto 35% foram. Identificamos variações regionais importantes, padrões relacionados à idade e comorbidades, e também sazonalidade nos casos. Realizamos tratamento de valores ausentes, codificação one-hot para variáveis categóricas e normalização das numéricas."

---

## Slide 5 – Modelagem

**Título do slide:** Modelagem: Regressão Logística

**Conteúdo:**
- **Modelo escolhido:** Regressão Logística
- **Justificativa:**
  - Interpretabilidade dos coeficientes
  - Eficiência computacional
  - Boa performance para classificação binária
- **Configuração:**
  - Divisão treino/teste: 80/20 com estratificação
  - `max_iter=1000`
  - `random_state=42` para reprodutibilidade
- **Técnicas aplicadas:**
  - SMOTE para balanceamento das classes
  - Ajuste de limiar de classificação (threshold tuning)

**Sugestão de fala:**
"Escolhemos a Regressão Logística como modelo principal devido à sua interpretabilidade, eficiência computacional e adequação para classificação binária. Dividimos os dados em 80% para treino e 20% para teste, com estratificação para manter a proporção das classes. Testamos o uso de SMOTE para balanceamento e realizamos ajuste do limiar de classificação para otimizar as métricas de interesse, especialmente o recall da classe UTI."

---

## Slide 6 – Resultados e Métricas

**Título do slide:** Resultados e Métricas

**Conteúdo:**
- **Modelo inicial (limiar 0,5):**
  - Acurácia: 67,91%
  - AUC-ROC: 0,6925
  - Recall (UTI=1): 27,00% → muito conservador
  - Precisão: [valor]
- **Após ajuste de limiar (0,3):**
  - Recall: 76,57% ⬆️ (aumento significativo)
  - Precisão: 45,28%
  - Acurácia: 59,35%
- **Interpretação:**
  - Trade-off entre precisão e recall
  - Priorização de sensibilidade (minimizar falsos negativos)
  - Adequado para contexto de saúde pública

**Sugestão de fala:**
"O modelo inicial, com limiar padrão de 0,5, apresentou acurácia de 67,91% e AUC-ROC de 0,69, mas o recall da classe UTI foi de apenas 27%, indicando que o modelo era muito conservador e perdia muitos casos verdadeiros. Após ajustar o limiar para 0,3, conseguimos aumentar o recall para 76,57%, o que significa que agora identificamos corretamente mais de três quartos dos pacientes que realmente precisam de UTI. Isso veio com uma redução na precisão para 45% e na acurácia para 59%, mas em saúde pública, é preferível ter mais falsos positivos do que falsos negativos, pois não identificar um paciente grave pode ter consequências fatais."

---

## Slide 7 – Conclusões

**Título do slide:** Conclusões

**Conteúdo:**
- **Principais contribuições:**
  - Modelo preditivo funcional para internação em UTI em SRAG
  - Identificação de fatores de risco relevantes
  - Metodologia reprodutível e documentada
- **Resultados alcançados:**
  - Recall de 76,57% após ajuste de limiar
  - Modelo adequado para contexto de saúde pública
  - Trade-off entre precisão e sensibilidade bem justificado
- **Aplicabilidade:**
  - Apoio à decisão clínica
  - Planejamento de recursos hospitalares
  - Identificação precoce de casos graves

**Sugestão de fala:**
"Concluímos que desenvolvemos um modelo preditivo funcional que pode auxiliar na identificação de pacientes com SRAG com maior risco de necessitar de internação em UTI. O modelo alcançou um recall de 76,57% após o ajuste de limiar, o que é adequado para o contexto de saúde pública. O trade-off entre precisão e sensibilidade foi justificado pela necessidade de minimizar falsos negativos. O modelo pode ser útil como ferramenta de apoio à decisão clínica e para planejamento de recursos hospitalares, especialmente em situações de alta demanda."

---

## Slide 8 – Limitações e Trabalhos Futuros

**Título do slide:** Limitações e Trabalhos Futuros

**Conteúdo:**
- **Limitações:**
  - Dados históricos (2016-2018), antes da pandemia de COVID-19
  - Desbalanceamento de classes mesmo após técnicas de balanceamento
  - Variáveis disponíveis limitadas pelo dataset original
  - Modelo não considera interações complexas entre variáveis
- **Trabalhos futuros:**
  - Testar outros algoritmos (Random Forest, XGBoost, Redes Neurais)
  - Engenharia de features mais avançada (interações, variáveis temporais)
  - Validação cruzada e tuning de hiperparâmetros
  - Incorporação de dados mais recentes (incluindo período pandêmico)
  - Desenvolvimento de interface para uso clínico

**Sugestão de fala:**
"Reconhecemos algumas limitações do nosso trabalho: utilizamos dados históricos anteriores à pandemia de COVID-19, o que pode limitar a generalização para contextos mais recentes. O desbalanceamento de classes ainda é um desafio, e temos limitações relacionadas às variáveis disponíveis no dataset. Para trabalhos futuros, sugerimos testar outros algoritmos de machine learning, realizar engenharia de features mais avançada, implementar validação cruzada robusta e incorporar dados mais recentes. Também seria interessante desenvolver uma interface amigável para uso clínico. Obrigado pela atenção!"

---

## Observações para a Apresentação

- **Tempo estimado:** 10-15 minutos
- **Transições:** Use transições suaves entre slides, conectando cada seção com a anterior
- **Visualizações:** Inclua gráficos da EDA, matriz de confusão e curva ROC nos slides apropriados
- **Tom:** Mantenha tom acadêmico mas acessível, explicando termos técnicos quando necessário
- **Encerramento:** Reserve tempo para perguntas ao final

