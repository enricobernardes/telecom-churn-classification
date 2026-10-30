# Previsão de churn: quando 80% de acerto quase não supera o chute

Case de classificação sobre cancelamento de clientes (*churn*) em telecomunicações, com foco em avaliar o modelo do jeito certo quando as classes são desbalanceadas.

> **Dataset sintético:** 1.200 clientes gerados no próprio notebook (semente fixa, resultados reproduzíveis). O objetivo é praticar o fluxo de classificação e a avaliação; os números não representam uma empresa real.

## Resumo
Testei 5 modelos para prever quais clientes de uma operadora vão cancelar. O melhor acertou 80%, mas um "chute" que diz sempre "vai cancelar" já acertava 78,75%: o modelo quase não agregava valor. Ao dar mais peso aos clientes que ficam, o modelo passou a acertar 93% das vezes em que aponta um cancelamento, ao custo de deixar passar mais cancelamentos.

**Qual o impacto?** Antes de investir em um modelo, a empresa precisa decidir o que custa mais: perder um cliente sem agir ou gastar desconto e contato com quem ia ficar de qualquer jeito. O número de acerto, sozinho, engana.

## Problema
A operadora perde clientes e só descobre depois do cancelamento. Uma ação de retenção (desconto, contato do suporte, revisão de plano) custa dinheiro, então ela não pode ser oferecida a todos. É preciso saber em quem agir antes que o cliente saia.

**Como o notebook resolve:** compara modelos de classificação contra um chute simples e avalia cada um pelas métricas certas para classes desbalanceadas, não só pelo acerto geral.

## Dados
- 1.200 clientes, 5 variáveis: idade, tempo de contrato, gasto mensal, chamados ao suporte, atraso de pagamento
- Alvo: `churn` (1 = cancela). **76% cancelam** (classes desbalanceadas)

## Abordagem
1. Qualidade dos dados e análise exploratória (correlação, distribuições, outliers)
2. Split treino/teste 80/20
3. Baseline (`DummyClassifier`, sempre "cancela") antes de qualquer modelo
4. Árvore de Decisão, Random Forest, Regressão Logística e KNN (escala só para Logística e KNN)
5. Comparação por Accuracy, Recall, Precision e F1, além do recall de quem fica
6. Teste com `class_weight='balanced'`

## Resultados

| Modelo | Accuracy | Recall (churn) | Precision (churn) | F1 (churn) | Recall (quem fica) |
|---|---|---|---|---|---|
| Baseline (sempre "cancela") | 0,788 | 1,000 | 0,788 | 0,881 | 0,000 |
| Árvore de Decisão | 0,738 | 0,820 | 0,842 | 0,831 | 0,431 |
| Random Forest | 0,767 | 0,915 | 0,812 | 0,861 | 0,216 |
| **Regressão Logística** | **0,800** | 0,958 | 0,819 | **0,883** | 0,216 |
| KNN | 0,771 | 0,894 | 0,828 | 0,860 | 0,314 |
| Reg. Logística + `class_weight` | 0,713 | 0,690 | 0,930 | 0,790 | 0,800 |

No F1, o melhor modelo (0,883) empata com o chute (0,881). É o **Paradoxo da Acurácia**: com classes desbalanceadas, acerto alto não significa modelo útil.

## Próximos passos
- Validação cruzada e ajuste do limiar de decisão
- Aplicar o fluxo a um dataset real de churn

## Tecnologias
Python · Pandas · NumPy · Matplotlib · Seaborn · Scikit-learn

## Como rodar
Abra `telecom_churn_classification.ipynb` no Jupyter, Colab ou Kaggle e rode tudo: o dataset é gerado no próprio notebook.

---
*Sintaxe com apoio de guias pessoais e IA; análise e conclusões são minhas.*
