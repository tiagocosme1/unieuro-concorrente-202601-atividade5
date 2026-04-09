# Relatório da NOME DA ATIVIDADE

**Disciplina:** Programação Concorrente e Distribuída
**Aluno(s):** Tiago Geraldo de Lima Cosme
**Turma:** Sistemas de Informação
**Professor:** Rafael
**Data:** 08/04/2026

---

# 1. Descrição do Problema

Neste trabalho, implementamos uma solução para a avaliação massiva de pares de dados, um problema comum em tarefas que exigem comparações exaustivas entre grandes conjuntos numéricos ou de strings. O objetivo principal foi transformar um processamento que levaria muito tempo de forma sequencial em algo ágil e distribuído.

Algoritmo: Utilizamos o padrão de programação distribuída com a interface MPI, onde um processo mestre coordena a distribuição de carga para diversos processos escravos.

Tamanho da entrada: O volume processado foi de exatos 12.497.500 de pares.

Objetivo da paralelização: Reduzir o tempo total de resposta aproveitando a arquitetura multi-core da CPU. Com a paralelização, o programa não fica limitado ao desempenho de apenas um núcleo, distribuindo o cálculo de forma simultânea.

Complexidade: O algoritmo possui complexidade $O(n)$, sendo $n$ o número total de pares, uma vez que a avaliação de cada par é uma operação de tempo constante.

---

# 2. Ambiente Experimental

Descreva o ambiente em que os experimentos foram realizados.

## Orientações

Informar as características do hardware e software utilizados na execução dos testes.

| Item                        | Descrição |
| --------------------------- | --------- |
| Processador                 |Intel Core i7 12 Gen|
| Número de núcleos           |12 Núcleos |
| Memória RAM                 |16 GB      |
| Sistema Operacional         |Windows 11 Pro|
| Linguagem utilizada         |Python 3.10+|
| Biblioteca de paralelização |mpi4py (MPI)|
| Compilador / Versão         |MPICH / OpenMPI|

---

# 3. Metodologia de Testes

A metodologia consistiu em isolar o tempo de processamento central para entender o ganho real de performance.

Medição: O tempo foi cronometrado via terminal e logs internos do script, focando no "Tempo Total MPI" retornado ao final da execução.

Execuções: Foram realizados testes com 1, 2, 4, 8 e 12 processos para observar o comportamento da curva de desempenho.

Condições: O ambiente de teste foi mantido estável, sem outras aplicações pesadas rodando em paralelo, para garantir que as oscilações de tempo fossem mínimas.

---

# 4. Resultados Experimentais

Preencha a tabela com os **tempos médios de execução** obtidos.

## Orientações

* O tempo deve ser informado em **segundos**
* Utilizar a **média das execuções**

| Nº Threads/Processos | Tempo de Execução (s) |
| -------------------- | --------------------- |
| 1                    |31.25                  |
| 2                    |23.98                  |
| 4                    |15.97                  |
| 8                    |11.29                  |
| 12                   |9.99                   |

---

# 5. Cálculo de Speedup e Eficiência

## Fórmulas Utilizadas

### Speedup

```
Speedup(p) = T(1) / T(p)
```

Onde:

* **T(1)** = tempo da execução serial
* **T(p)** = tempo com p threads/processos

### Eficiência

```
Eficiência(p) = Speedup(p) / p
```

Onde:

* **p** = número de threads ou processos

---

# 6. Tabela de Resultados

Preencha a tabela abaixo utilizando os tempos medidos.

| Threads/Processos | Tempo (s) | Speedup | Eficiência |
| ----------------- | --------- | ------- | ---------- |
| 1                 |31.25      | 1.0     | 1.0        |
| 2                 |23.98      |1.30     | 0.65       |
| 4                 |15.97      |1.95     | 0.49       |
| 8                 |11.29      |2.76     | 0.34       |
| 12                |9.99       |3.12     | 0.26       |

---

# 7. Gráfico de Tempo de Execução

![Gráfico Tempo Execução](graficos/tempo_execucao.png)

---

# 8. Gráfico de Speedup

![Gráfico Speedup](graficos/speedup.png)

---

# 9. Gráfico de Eficiência

![Gráfico Eficiência](graficos/eficiencia.png)

---

# 10. Análise dos Resultados

Ao analisar os dados, notamos que a aplicação apresenta uma escalabilidade clara, saindo de 31,25 segundos (serial) para menos de 10 segundos com 12 processos. No entanto, o speedup de 3.12x para 12 núcleos mostra que o ganho não é linear.

Principais pontos observados:

Eficiência: A eficiência caiu conforme aumentamos o número de processos (chegando a 0.26 com 12 núcleos). Isso indica que, embora o resultado saia mais rápido, o custo de "gerenciar" tantos processos e a comunicação entre eles começa a pesar.

Overhead: Houve um overhead considerável de comunicação. Como o MPI precisa "conversar" entre processos para dividir os 12 milhões de pares, esse tempo de troca de mensagens acaba limitando o speedup.

Recursos: O ponto onde a eficiência começou a cair de forma mais acentuada sugere que o número de processos ultrapassou a quantidade de núcleos físicos reais da máquina, gerando disputa por recursos do sistema.

---

# 11. Conclusão

A paralelização via MPI trouxe um ganho de desempenho significativo, provando ser indispensável para lidar com volumes de dados superiores a 12 milhões de registros. O tempo total de execução foi reduzido em aproximadamente 68% na melhor configuração (12 processos).

Como conclusão técnica, o melhor equilíbrio entre velocidade e uso eficiente de hardware seria manter a execução próxima de 4 processos, onde a eficiência ainda está próxima de 50%. Para melhorar esses números no futuro, poderíamos otimizar a distribuição dos dados em "lotes" maiores (chunks), diminuindo a frequência de comunicação e aumentando o tempo que cada núcleo passa calculando de fato.
---
