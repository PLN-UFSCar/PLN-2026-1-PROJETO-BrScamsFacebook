# Detecção e Classificação de Golpes Digitais e Publicações Suspeitas em Redes Sociais

**Projeto Final - Processamento de Linguagem Natural (PLN) - UFSCar (2026/1)**

**Equipe:**
* Brenda Raquel Maia (757891)
* Bruna Matias de Lima (820582)
* João Manoel Ribeiro Machado (822447)
* Julia Pedro Silva (820869)
* Larissa Dias da Silva (800204)

---

## 1. Recapitulação do Projeto (Seminário 1)
* **Tarefa Escolhida:** Classificação binária de mensagens do Facebook nas categorias **Ham** (mensagens legítimas) e **Spam** (golpes/scams).
* **Dataset:** BrScamsFacebook, composto por 450 mensagens em língua portuguesa. O conjunto é fortemente desbalanceado: 375 mensagens Spam (83,3%) e 75 mensagens Ham (16,7%).
* **Medidas de Avaliação:** Acurácia, Precisão, Recall, F1-Score (com foco especial na classe minoritária) e Matriz de Confusão. O projeto original foi mantido integralmente em relação ao Seminário 1.

## 2. Estratégias Adotadas e Pré-processamento
Para o processamento da língua portuguesa, utilizamos as bibliotecas **NLTK** e **spaCy** (modelo `pt_core_news_sm`). 

### Etapas de Pré-processamento e Representação
1. **Limpeza textual:** Remoção de URLs, menções (@), emojis, pontuação e conversão para caixa baixa.
2. **Tokenização e remoção de stopwords:** Feito com suporte do NLTK.
3. **Redução morfológica:** Testamos *stemming* (RSLP) e *lematização* (spaCy). Optamos pela **lematização** por preservar palavras do léxico português, favorecendo a interpretabilidade, resultando em um vocabulário de 4.336 termos únicos.
4. **Representação Textual:** Bag-of-Words (CountVectorizer) e TF-IDF (TfidfVectorizer), limitadas a 3.000 features usando scikit-learn.

### Dificuldades Encontradas
* Alta esparsidade das matrizes de representação devido aos textos curtos típicos de redes sociais.
* Uso de gírias, abreviações e erros ortográficos que dificultam a atuação das ferramentas padrão.
* Erros na lematização de nomes próprios em contextos informais (ex: "Amanda" lematizado como o verbo "amando").
* Desbalanceamento severo das classes, exigindo técnicas compensatórias na modelagem.

## 3. Resultados dos Experimentos

Desenvolvemos e comparamos três estratégias principais:
* **Estratégia 1 (Abordagem Heurística):** Uso de regras baseadas em URLs, valores monetários, letras maiúsculas e um léxico de palavras-gatilho (ex: *pix, prêmio, grátis*).
* **Estratégia 2 (Modelos Lineares/Probabilísticos):** Naive Bayes Multinomial e Regressão Logística.
* **Estratégia 3 (Não lineares/Ensembles):** SVM, Decision Tree e Random Forest.

### Avaliação Quantitativa
Devido ao desbalanceamento, a métrica principal de comparação foi o **F1-Score Macro**.
* O **melhor modelo** foi a **Regressão Logística com representação TF-IDF e pesos de classes balanceados**, alcançando **F1 macro de 0,661** (Recall Spam: 0,936 / Recall Ham: 0,347).
* A *baseline heurística* obteve um baixo recall (0,285) para Spam, provando que regras fixas sofrem com variações linguísticas. Árvores de decisão apresentaram forte tendência ao *overfitting* devido à alta dimensionalidade do vocabulário frente à baixa quantidade de amostras.

### Avaliação Qualitativa
Realizamos uma avaliação humana em uma amostra balanceada e cega de 40 mensagens (20 Ham / 20 Spam):
* **Fleiss' Kappa:** 0,4687 (Concordância moderada).
* **Concordância média entre pares:** 74,50%.
* O consenso humano divergiu do rótulo original do dataset em alguns casos. A análise revelou que certas mensagens fraudulentas (ex: lojas virtuais falsas) são extremamente curtas e genéricas, sendo impossíveis de classificar com precisão apenas pelo texto, sem o contexto externo (anúncio/vendedor).

## 4. Considerações Finais e Aprendizados
O projeto demonstrou na prática a influência direta do pré-processamento no desempenho dos classificadores, especialmente em textos curtos e informais. Aprendemos que:
* Modelos lineares regularizados (como Regressão Logística) lidam melhor com a alta dimensionalidade textual do que abordagens baseadas em regras fixas, que não generalizam golpes implícitos.
* A acurácia é uma métrica ilusória em datasets desbalanceados.
* A detecção real de golpes necessita ir além do texto, devendo incorporar metadados, características de URLs e informações do perfil do usuário para superar ambiguidades linguísticas.

---
*Os códigos finais e o dataset (se de livre disponibilização) encontram-se nos diretórios deste repositório.*