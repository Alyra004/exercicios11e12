# Resolução - Exercícios I (Computação Distribuída)

Este repositório contém a resolução das atividades 1.1 e 1.2 da disciplina de Computação Distribuída. A solução computacional, incluindo tabelas e gráficos, foi desenvolvida em um Jupyter Notebook (`.ipynb`).

## Exercício 1.1: Dedução Analítica da Disponibilidade

O objetivo é calcular a disponibilidade ($A$) de um serviço replicado em $n$ servidores, onde cada servidor tem uma probabilidade individual $p$ de estar disponível, sendo necessário um mínimo de $k$ servidores para operação consistente.

A disponibilidade segue uma distribuição binomial, que calcula a probabilidade de obtermos $i$ sucessos (servidores disponíveis) em $n$ tentativas. Como o sistema exige "pelo menos" $k$ servidores, somamos as probabilidades de $k$ até $n$:

$$A(n, k, p) = \sum_{i=k}^{n} \binom{n}{i} p^i (1-p)^{n-i}$$

**Casos Extremos:**
* **Consulta ($k = 1$):** O serviço funciona se pelo menos um servidor estiver online. É o complemento de todos falharem.
  $$A_{k=1} = 1 - (1-p)^n$$
* **Atualização ($k = n$):** O serviço requer que todos os servidores estejam online simultaneamente.
  $$A_{k=n} = p^n$$

## Exercício 1.2: Cálculo Analítico e Simulador Estocástico

A implementação foi dividida em duas partes dentro do Notebook:
1. **Cálculo Analítico:** Implementação direta da fórmula binomial deduzida no Exercício 1.1 para calcular o valor exato da disponibilidade teórica.
2. **Simulador Estocástico:** Um modelo computacional que executa 10.000 rodadas para cada cenário. Em cada rodada, a disponibilidade de cada servidor é sorteada aleatoriamente (número entre 0 e 1 comparado à probabilidade $p$). A proporção de rodadas em que o sistema atinge o quórum $k$ gera a "frequência experimental", permitindo a comparação com o modelo analítico.

**Premissa adotada:** Para o caso específico de $k = n/2$, optou-se por utilizar o teto (arredondamento para cima) para garantir sempre a maioria absoluta em casos de $n$ ímpar.
