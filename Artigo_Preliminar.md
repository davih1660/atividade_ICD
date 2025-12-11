# Modelagem Preditiva de Internação em UTI em Casos de SRAG no Brasil (2016-2018)

**Autores:**  
Davi Henrique Menezes da Cruz  
Sabrina de Oliveira Souza

**Instituição:**  
Instituto Federal de Brasília (IFB)

---

## 1. Introdução

A Síndrome Respiratória Aguda Grave (SRAG) representa um importante problema de saúde pública no Brasil, com impactos significativos na morbidade e mortalidade da população. A identificação precoce de pacientes com maior risco de necessitar de internação em Unidade de Terapia Intensiva (UTI) é fundamental para o planejamento de recursos hospitalares, a otimização do atendimento e a melhoria dos desfechos clínicos.

Este estudo tem como objetivo analisar os fatores associados à internação em Unidade de Terapia Intensiva (UTI) em casos de SRAG no Brasil, considerando variáveis demográficas, clínicas e de acesso à saúde. Especificamente, busca-se desenvolver um modelo preditivo capaz de identificar quais pacientes com SRAG têm maior probabilidade de necessitar de internação em UTI, utilizando dados dos anos de 2016, 2017 e 2018.

A escolha do período de análise (2016-2018) permite uma visão abrangente dos padrões de ocorrência da SRAG antes da pandemia de COVID-19, oferecendo uma linha de base importante para estudos comparativos futuros. A identificação precoce de pacientes com maior risco de necessidade de cuidados intensivos é fundamental para o planejamento de recursos hospitalares e a otimização do atendimento em saúde.

---

## 2. Materiais e Métodos

### 2.1. Dados Utilizados

O estudo utilizou dados do Sistema de Informação de Agravos de Notificação (SINAN), disponibilizados pelo Ministério da Saúde do Brasil, referentes aos casos de SRAG notificados nos anos de 2016, 2017 e 2018. Os arquivos utilizados foram:

- `INFLUD16.csv` - Base de dados de 2016
- `INFLUD17.csv` - Base de dados de 2017
- `INFLUD18.csv` - Base de dados de 2018

### 2.2. Variáveis Selecionadas

As seguintes variáveis foram selecionadas para análise:

**Variáveis Demográficas:**
- **Município de Registro do caso** (`ID_MUNICIP`)
- **Data dos primeiros sintomas** (`DT_SIN_PRI`)
- **Sexo** (`CS_SEXO`)
- **Raça/Cor** (`CS_RACA`)
- **Idade** (`NU_IDADE_N`)
- **Escolaridade** (`CS_ESCOL_N`)
- **UF** (`SG_UF`)
- **Município de residência** (`ID_MN_RESI`)
- **Distrito** (`ID_DISTRIT`)

**Variáveis Clínicas e de Acesso à Saúde:**
- **Gestante** (`CS_GESTANT`)
- **Hospitalizado** (`HOSPITAL`)
- **Internado em UTI** (`UTI`) - **Variável Alvo**
- **Vacinação** (`VACINA`)
- **Evolução** (`EVOLUCAO`)

**Comorbidades:**
- **Obesidade** (`OBESIDADE`)
- **Cardiopatia** (`CARDIOPATI`)
- **Pneumopatia** (`PNEUMOPATI`)
- **Doença Metabólica** (`METABOLICA`)

**Sintomas:**
- **Febre** (`FEBRE`)
- **Tosse** (`TOSSE`)
- **Dispneia** (`DISPNEIA`)

### 2.3. Preparação dos Dados

#### 2.3.1. Limpeza e Tratamento de Dados

1. **Unificação das bases**: As três bases de dados foram unificadas em um único conjunto, totalizando 131.687 registros iniciais.

2. **Tratamento de valores ausentes**:
   - Valores ausentes em `raca_cor` foram tratados (aproximadamente 2-4% dos casos)
   - Registros com data de primeiros sintomas inválida foram removidos
   - Valores inconsistentes em `sexo` (como "I" para ignorado) foram tratados

3. **Padronização de variáveis**:
   - `sexo`: Padronizado para "M" (masculino) ou "F" (feminino)
   - `raca_cor`: Convertida para numérica, com códigos: 1=Branca, 2=Preta, 3=Amarela, 4=Parda, 5=Indígena, 9=Ignorado
   - `data_primeiros_sintomas`: Convertida para formato datetime
   - `uf`: Mapeada para regiões do Brasil (Norte, Nordeste, Sudeste, Sul, Centro-Oeste)

4. **Criação de variáveis derivadas**:
   - **Região**: Criada a partir do mapeamento das UFs para as cinco regiões do Brasil
   - **Mês**: Extraído da data dos primeiros sintomas para análise temporal
   - **Ano**: Mantido para controle temporal

#### 2.3.2. Criação da Variável Alvo

A variável alvo binária `internado_uti` foi criada a partir da variável `UTI` para classificar os casos em:
- **0 (Não internado em UTI)**: Casos com UTI = 2 (Não)
- **1 (Internado em UTI)**: Casos com UTI = 1 (Sim)

Registros com UTI = 9 (Ignorado) foram excluídos da análise. Após a filtragem (removendo registros com sexo indefinido e UTI ignorada), o conjunto final para modelagem contém **127.281 registros**, com a seguinte distribuição:
- Não UTI (0): 64.6%
- UTI (1): 35.4%

### 2.4. Modelo de Aprendizado de Máquina

#### 2.4.1. Algoritmo Escolhido: Regressão Logística

A **Regressão Logística** foi escolhida como algoritmo de classificação por apresentar as seguintes vantagens:

1. **Adequação ao problema**: É apropriada para classificação binária, que é o caso deste estudo
2. **Interpretabilidade**: Os coeficientes do modelo permitem interpretação direta do efeito de cada variável
3. **Eficiência computacional**: Treina rapidamente mesmo com grandes volumes de dados
4. **Capacidade de lidar com variáveis categóricas e numéricas**: Permite combinar diferentes tipos de features

#### 2.4.2. Features Utilizadas

As seguintes variáveis foram utilizadas como features preditoras:

**Variáveis categóricas** (transformadas em dummy variables):
- `sexo`: Masculino (M) ou Feminino (F)
- `regiao`: Norte (N), Nordeste (NE), Sudeste (SE), Sul (S), Centro-Oeste (CO)
- `raca_cor_cat`: Branca, Preta, Amarela, Parda, Indígena
- `escolaridade`: Nível de escolaridade
- `gestante`: Indica se o paciente é gestante
- `hospital`: Indica se o paciente foi hospitalizado
- `vacina`: Indica se o paciente foi vacinado
- `evolucao`: Evolução do caso
- `obesidade`: Presença de obesidade
- `cardiopati`: Presença de cardiopatia
- `pneumopati`: Presença de pneumopatia
- `metabolica`: Presença de doença metabólica
- `febre`: Presença de febre
- `tosse`: Presença de tosse
- `dispneia`: Presença de dispneia

**Variáveis numéricas**:
- `ano`: Ano do registro (2016, 2017, 2018)
- `mes`: Mês dos primeiros sintomas (1-12)
- `idade`: Idade do paciente em anos

Após a transformação (one-hot encoding com drop_first para evitar multicolinearidade), o modelo utiliza múltiplas features derivadas das variáveis categóricas acima, além das variáveis numéricas.

#### 2.4.3. Divisão dos Dados

Os dados foram divididos em conjuntos de treino e teste:
- **Treino**: 70% dos dados (aproximadamente 89.096 registros)
- **Teste**: 30% dos dados (aproximadamente 38.185 registros)

A divisão foi realizada com `stratify=y` para manter a proporção da variável alvo (internado_uti) em ambos os conjuntos, garantindo representatividade.

#### 2.4.4. Configuração do Modelo

O modelo de Regressão Logística foi configurado com os seguintes parâmetros:
- `max_iter=1000`: Número máximo de iterações para convergência
- `class_weight='balanced'`: Ajuste automático dos pesos das classes para lidar com o desbalanceamento
- `random_state=42`: Semente aleatória para reprodutibilidade

---

## 3. Primeiros Resultados

### 3.1. Métricas de Avaliação

O modelo foi avaliado utilizando as seguintes métricas:

#### 3.1.1. Acurácia
A acurácia do modelo foi de **52.0%**, indicando que o modelo classifica corretamente pouco mais da metade dos casos. Considerando que a classe majoritária (não internado em UTI) representa 64.6% dos dados, a acurácia está próxima do baseline, sugerindo que o modelo tem dificuldade em superar a classe majoritária.

#### 3.1.2. F1-Score
O F1-Score foi de **0.408**, indicando uma performance moderada. Esta métrica é particularmente útil para dados desbalanceados, pois balanceia precisão e recall.

#### 3.1.3. AUC-ROC
A área sob a curva ROC (AUC-ROC) foi de **0.521**, indicando que o modelo possui capacidade discriminativa ligeiramente superior a um classificador aleatório (AUC = 0.5). Este valor sugere que há espaço significativo para melhorias no modelo.

#### 3.1.4. Matriz de Confusão

A matriz de confusão no conjunto de teste apresentou os seguintes resultados:

|                | Predito: Não UTI | Predito: UTI |
|----------------|------------------|--------------|
| **Real: Não UTI** | 11.674 (TN)     | 9.332 (FP)   |
| **Real: UTI** | 6.435 (FN)       | 5.431 (TP)   |

**Interpretação:**
- **Verdadeiros Negativos (TN)**: 11.674 casos não internados em UTI foram corretamente classificados
- **Falsos Positivos (FP)**: 9.332 casos não internados em UTI foram incorretamente classificados como internados em UTI
- **Falsos Negativos (FN)**: 6.435 casos internados em UTI foram incorretamente classificados como não internados em UTI
- **Verdadeiros Positivos (TP)**: 5.431 casos internados em UTI foram corretamente classificados

**Métricas derivadas:**
- **Sensibilidade (Recall - Classe UTI)**: 45.8% - Dos casos realmente internados em UTI, menos da metade foi identificada corretamente
- **Especificidade (Classe Não UTI)**: 55.6% - Dos casos realmente não internados em UTI, pouco mais da metade foi identificada corretamente
- **Precisão (Classe UTI)**: 36.8% - Dos casos previstos como internados em UTI, apenas 36.8% são realmente internados em UTI

### 3.2. Análise do Desbalanceamento

O conjunto de dados apresenta um desbalanceamento moderado:
- **Não UTI (0)**: 64.6% dos casos
- **UTI (1)**: 35.4% dos casos
- **Razão de desbalanceamento**: 1.83:1

Este desbalanceamento pode explicar parte da dificuldade do modelo em identificar corretamente a classe minoritária (internado em UTI). O uso de `class_weight='balanced'` ajuda a compensar esse desbalanceamento, mas pode não ser suficiente para alcançar uma performance ideal.

### 3.3. Importância das Variáveis

A análise dos coeficientes da Regressão Logística permite identificar quais variáveis têm maior influência na predição da internação em UTI. Os coeficientes indicam:

- **Coeficientes positivos**: Aumentam a probabilidade de internação em UTI
- **Coeficientes negativos**: Diminuem a probabilidade de internação em UTI
- **Valores absolutos maiores**: Indicam maior influência na predição

As variáveis incluídas no modelo abrangem diferentes dimensões:
- **Demográficas**: Sexo, Raça/Cor, Idade, Escolaridade, Região
- **Clínicas**: Comorbidades (obesidade, cardiopatia, pneumopatia, doença metabólica)
- **Sintomas**: Febre, Tosse, Dispneia
- **Acesso à Saúde**: Hospitalização, Vacinação, Evolução do caso

*(Nota: Os valores específicos dos coeficientes serão apresentados após a execução completa do modelo no notebook)*

### 3.4. Visualizações

O modelo gerou as seguintes visualizações:

1. **Matriz de Confusão (Heatmap)**: Representação visual da matriz de confusão, facilitando a interpretação dos erros de classificação
2. **Curva ROC**: Gráfico mostrando a relação entre taxa de verdadeiros positivos e taxa de falsos positivos, com a área sob a curva (AUC) indicada
3. **Gráfico de Métricas**: Comparação visual das três principais métricas (Acurácia, F1-Score, AUC-ROC)
4. **Importância das Variáveis**: Gráfico de barras horizontal mostrando os coeficientes de cada variável no modelo

---

## 4. Discussão Preliminar

### 4.1. Performance do Modelo

A performance inicial do modelo (Acurácia: 52%, F1-Score: 0.410, AUC-ROC: 0.520) indica que há espaço significativo para melhorias. O modelo está performando apenas ligeiramente melhor que um classificador aleatório, o que sugere que:

1. As features atuais podem não ser suficientemente informativas para a tarefa de classificação
2. Pode haver necessidade de engenharia de features adicional (ex: interações entre variáveis)
3. O desbalanceamento, mesmo com tratamento, pode estar impactando a performance

### 4.2. Limitações Identificadas

1. **Dificuldade em identificar a classe minoritária**: O modelo tem baixa sensibilidade (45.8%) para a classe UTI, o que é problemático se o objetivo é identificar precocemente pacientes que necessitarão de cuidados intensivos
2. **Alta taxa de falsos positivos**: 9.332 casos não internados em UTI foram incorretamente classificados como internados em UTI, indicando que o modelo pode estar superestimando a necessidade de cuidados intensivos
3. **Alta taxa de falsos negativos**: 6.435 casos internados em UTI foram incorretamente classificados como não internados, o que pode ser crítico em um contexto clínico, onde a subestimação da necessidade de UTI pode ter consequências graves
4. **Performance próxima ao baseline**: A acurácia de 52% está muito próxima da classe majoritária (64.6%), sugerindo que o modelo ainda não está capturando adequadamente os padrões que levam à internação em UTI

### 4.3. Próximos Passos

Para melhorar a performance do modelo, os seguintes ajustes serão testados:

1. **Teste de outros algoritmos**:
   - Árvores de Decisão (melhor interpretabilidade)
   - Random Forest (pode capturar interações não-lineares)
   - kNN (pode ser útil para padrões locais)
   - Naive Bayes (pode ser eficaz para dados categóricos)

2. **Engenharia de features**:
   - Criar interações entre variáveis (ex: sexo × região)
   - Adicionar variáveis temporais (sazonalidade, tendências)
   - Considerar variáveis geográficas adicionais (densidade populacional, IDH)

3. **Tratamento de desbalanceamento**:
   - Testar SMOTE (Synthetic Minority Oversampling Technique)
   - Ajustar threshold de classificação
   - Usar métricas específicas (F1, AUC-ROC) como critério de otimização

4. **Validação cruzada**:
   - Implementar k-fold cross-validation para avaliação mais robusta
   - Realizar tuning de hiperparâmetros (GridSearchCV ou RandomizedSearchCV)

---

## 5. Conclusões Preliminares

Este estudo apresenta uma primeira abordagem à modelagem preditiva de internação em UTI em casos de SRAG no Brasil. A Regressão Logística foi implementada com sucesso, permitindo uma avaliação inicial do problema. Embora a performance inicial seja moderada, os resultados fornecem insights importantes sobre:

1. A distribuição dos casos de SRAG que necessitam de internação em UTI (35.4% dos casos)
2. O impacto do desbalanceamento de classes na performance do modelo
3. A necessidade de ajustes e melhorias para alcançar uma performance mais robusta, especialmente considerando a importância clínica de identificar corretamente pacientes que necessitarão de cuidados intensivos

Os próximos passos incluem a exploração de outros algoritmos, engenharia de features adicional (incluindo interações entre variáveis), técnicas mais avançadas de tratamento de desbalanceamento e ajuste de threshold de classificação, visando melhorar a capacidade preditiva e a interpretabilidade do modelo, com foco especial em aumentar a sensibilidade para identificar corretamente os casos que necessitam de internação em UTI.

---

## Referências

*(A ser completado na versão final do artigo)*

---

**Data de elaboração:** Dezembro de 2024  
**Versão:** Preliminar - Etapa 4

