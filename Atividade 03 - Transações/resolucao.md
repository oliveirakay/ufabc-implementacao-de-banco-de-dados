# Resolução - Atividade 03 - Transações e Controle de Concorrência
## Questão 1 (4,5)

Considere o seguinte cenário com itens de dados: A, B, C, todos com valor inicial 100. Duas transações (T1 e T2) executam operações sobre esses itens conforme indicado abaixo.

| T1 | T2 |
| :--- | :--- |
| R1(A); | R2(B); |
| R1(B); | R2(C); |
| S1 = A + B; | S2 = B + C; |
| W1(C, S1); -- escreve em C o valor da soma | W2(A, S2); -- escreve em A o valor da soma |

### Com base nessas informações, responda as questões a seguir.

Q1a. (2,0) Apresente um escalonamento possível em que T1 e T2 são executadas <b>concorrentemente</b>, mas cujo resultado <b>NÃO é seriável</b>. Demonstre que seu escalonamento não é seriável.


Q1b. (2,5) Apresente um escalonamento possível em que T1 e T2 são executadas concorrentemente, mas cujo resultado É seriável.
## Resposta da Questão 1
### Escalonamentos Seriais

### Plano S1 T1 -> T2

| Tempo | T1 | T2 | Efeito |
| :--- | :--- | :--- | :--- |
| 1 | R1(A) | | T1 lê A=100 |
| 2 | R1(B) | | T1 lê B=100 |
| 3 | S1 = A + B | | S1 = 100 + 100 = 200 |
| 4 | W1(C, S1) | | C é atualizado para 200 |
| 5 | | R2(B) | T2 lê B=100 |
| 6 | | R2(C) | T2 lê C=200 |
| 7 | | S2 = B + C | S2 = 100 + 200 = 300 |
| 8 | | W2(A, S2) | A é atualizado para 300 |

### Plano S2 T2 -> T1

| Tempo | T1 | T2 | Efeito |
| :--- | :--- | :--- | :--- |
| 1 | | R2(B) | T2 lê B=100 |
| 2 | | R2(C) | T2 lê C=100 |
| 3 | | S2 = B + C | S2 = 100 + 100 = 200 |
| 4 | | W2(A, S2) | A é atualizado para 200 |
| 5 | R1(A) | | T1 lê A=200 |
| 6 | R1(B) | | T1 lê B=100 |
| 7 | S1 = A + B | | S1 = 200 + 100 = 300 |
| 8 | W1(C, S1) | | C é atualizado para 300 |

| Plano | A | B | C |
|:--- | :--- | :--- | :--- |
| Inicial | 100 | 100 | 100 |
| Plano S1 (T1 -> T2) | 300 | 100 | 200 |
| Plano S2 (T2 -> T1) | 200 | 100 | 300 |
### O que eu quero: 
- Concorrência - No contexto se traduz: Intercalação das operações de transações distintas
- Não seriável (Q1a) - Resultado final deve ser distindo de um escalonamento serial.
- Seriável (Q1b) - Resultado final deve ser igual a um dos resultados do escalonamento serial.



### Plano K - Há concorrência, mas não é seriável devido ao resultado final
| Tempo | T1 | T2 | Efeito |
| :--- | :--- | :--- | :--- |
| 1 | R1(A) | | T1 lê A=100 |
| 2 | R1(B) | | T1 lê B=100 |
| 3 | | R2(B) | T2 lê B=100 |
| 4 | | R2(C) | T2 lê C=200 |
| 5 | S1 = A + B | | S1 = 100 + 100 = 200 |
| 6 | W1(C, S1) | | C é atualizado para 200, mas a operação foi perdida |
| 7 | | S2 = B + C | S2 = 100 + 100 = 200 |
| 8 | | W2(A, S2) | A é atualizado para 200 |

### Plano Ks - Há concorrência e é seriável. 
| Tempo | T1 | T2 | Efeito |
| :--- | :--- | :--- | :--- |
| 1 | R1(A) | | T1 lê A=100 |
| 2 | R1(B) | | T1 lê B=100 |
| 3 | | R2(B) | T2 lê B=100 |
| 4 | S1 = A + B | | S1 = 100 + 100 = 200 |
| 5 | W1(C, S1) | | C é atualizado para 200, mas a operação foi perdida |
| 6 | | R2(C) | T2 lê C=200 |
| 7 | | S2 = B + C | S2 = 100 + 100 = 200 |
| 8 | | W2(A, S2) | A é atualizado para 200 |

| Plano | A | B | C |
|:--- | :--- | :--- | :--- |
| Inicial | 100 | 100 | 100 |
| Plano S1 (T1 -> T2) | 300 | 100 | 200 |
| Plano S2 (T2 -> T1) | 200 | 100 | 300 |
| Plano K (Não Seriável)| 200 | 100 | 200 |

### Valor final das variáveis para cada aplicação de plano.

- Para considerar um plano não seriável o resultado final precisa ser distinto dos possíveis planos seriáveis, ou seja, nesse caso S1 e S1.

| Plano | A | B | C |
|:--- | :--- | :--- | :--- |
| Inicial | 100 | 100 | 100 |
| Plano S1 (T1 -> T2) | 300 | 100 | 200 |
| Plano S2 (T2 -> T1) | 200 | 100 | 300 |
| Plano Ks (Seriável) | 200 | 100 | 300 |

### Anotações e observações

#### OBS:

- Houve concorrência nos exemplos acima, e garantimos consistência com o fato das transações serem seriáveis. Mas o que a concorrência trouxe de positivo?

#### Concorrência vs Paralelismo:


#### Escalonamento: Conceito

Conjunto de operações de transações, no qual as opereações de uma mesma transação se mantém em ordem relativa, mas as oporações de transações distintas podem ser intercaladas a fim de alcançar a concorrência.

Resultado Não Seriável e Seriável: 
Seja A, e B duas transações com {a1,a,2,...,an} e {b1,b2,...,bn} operações dessas respectivamente, e x,y,z variáveis usadas nessas transações.

Um plano X é dito seriável se e somente x, o valor de x,y,z após a aplicação das operações x1,x2...xn resultar em valores de x,y,z iguais a um dos resultados dos Planos: seq(A,B),seq(B,A).

> *Não Serial* é diferente de *Não Seriável*.
> 
> *Serial* é diferente de *Seriável*.


## Script SQL

```sql

```
