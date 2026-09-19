# Computação Distribuída - Resolução dos Exercícios I (1.1 e 1.2)

**Instituição:** Universidade de Fortaleza (UNIFOR)  
**Aluna:** Amanda Lira Andrade Botelho  

Este repositório contém a resolução das atividades 1.1 e 1.2 da lista "Exercícios I" da disciplina de Computação Distribuída. A solução computacional, incluindo o simulador estocástico, geração de tabelas e visualização de gráficos, foi desenvolvida inteiramente em Python utilizando um Jupyter Notebook.

---

## 📁 Estrutura do Projeto

| Arquivo | Descrição |
| :--- | :--- |
| `resolucao_exercicios.ipynb` | Arquivo principal contendo as implementações em Python, o cálculo analítico, o simulador estocástico e a geração dos gráficos 2D. |
| `requirements.txt` | Lista de dependências do Python (bibliotecas) necessárias para executar o projeto. |
| `README.md` | Documentação principal com as instruções de execução e a dedução teórica do Exercício 1.1. |

---

## 🚀 Como Rodar o Projeto

Para executar o notebook e reproduzir as simulações e gráficos localmente, certifique-se de ter o Python instalado na sua máquina e siga os passos abaixo:

1. **Crie um ambiente virtual (recomendado):**
   ```bash
   python -m venv .venv
   ```

2. **Ative o ambiente virtual:**
   * No Windows:
     ```bash
     .venv\Scripts\activate
     ```
   * No Linux/Mac:
     ```bash
     source .venv/bin/activate
     ```

3. **Instale as dependências:**
   Crie um arquivo `requirements.txt` com o conteúdo `numpy pandas matplotlib jupyter` e rode:
   ```bash
   pip install -r requirements.txt
   ```

4. **Inicie o Jupyter Notebook:**
   ```bash
   jupyter notebook resolucao_exercicios.ipynb
   ```
   *Você também pode abrir e rodar o arquivo `.ipynb` diretamente através de editores como VS Code.*

---

## 📝 Exercício 1.1: Dedução Analítica da Disponibilidade

O objetivo da função é calcular matematicamente a disponibilidade de um serviço com base na falha independente das máquinas que compõe o sistema sendo:
- $n$ = número total de servidores no sistema
- $k$ = a quantidade mínima de servidores que deve estar ativa simultaneamente para o sistema funcionar
- $p$ = probabilidade individual do funcionamento de cada servidor

### 1. Obtendo a fórmula

#### Disponibilidade independente de cada servidor

Cada um dos servidores no sistema pode ter apenas um de dois estados, disponível e indisponível, sendo a probabilidade dele estar disponível $p$, logo temos para cada um deles a probabilidade de estar indisponível como $(1 - p)$.

#### Probabilidade de cenário específico

Se quisermos analisar a probabilidade de um caso específico onde temos $i$ servidores online, enquanto o restante $(n - i)$ está offline, obtemos essa probabilidade com $p^i (1 - p)^{n - i}$. É aplicada a probabilidade de se estar disponível para todos os servidores que queremos disponíveis no caso ($p^i$) e multiplicada pela probabilidade de indisponibilidade para todos aqueles que estariam indisponíveis no caso ($(1 - p)^{n - i}$).

### Combinações

Como os $i$ servidores podem ser quaisquer uns dentro de $n$, para considerar todas as combinações de quem está online, multiplicamos a probabilidade pelo coeficiente binomial $\binom{n}{i}$.

### Pelo menos $k$ servidores

Como estamos trabalhando com pelo menos $k$ servidores disponíveis, não podemos analisar somente o caso exato onde $i = k$, mas também todos os cenários subsequentes até $n$. Assim, é necessário fazer o somatório das probabilidades de todos esses cenários, obtendo a equação geral:

$$A(n, k, p) = \sum_{i=k}^{n} \binom{n}{i} p^i (1-p)^{n-i}$$

### 2. Dedução dos Casos Extremos

Para simplificar a análise do comportamento do sistema, derivamos as fórmulas para os dois cenários extremos solicitados pelo problema:

* **Operação de Atualização ($k = n$): Maioria Máxima**
  Neste cenário, o serviço exige que **todos** os $n$ servidores estejam operantes simultaneamente. Aplicando $k = n$ na fórmula geral, temos apenas um termo na somatória onde $i = n$:
  $$A_{k=n} = \binom{n}{n} p^n (1-p)^0$$
  Como $\binom{n}{n} = 1$ e $(1-p)^0 = 1$, a fórmula se reduz à probabilidade conjunta de todos os eventos independentes:
  $$A_{k=n} = p^n$$

* **Operação de Consulta ($k = 1$): Maioria Mínima**
  Aqui, o serviço funciona se **pelo menos um** servidor estiver online. Em vez de somar as probabilidades de $i=1$ até $i=n$, é matematicamente mais eficiente utilizar a regra do evento complementar: calcular a probabilidade de o serviço falhar completamente (zero servidores disponíveis, $i=0$) e subtrair de $1$.
  A probabilidade de todos os $n$ servidores falharem simultaneamente é:
  $$P(X = 0) = (1-p)^n$$
  Portanto, a disponibilidade garantida por pelo menos um servidor é o complemento dessa falha total:
  $$A_{k=1} = 1 - (1-p)^n$$

---

## 📊 Exercício 1.2: Cálculo Analítico vs. Simulação Estocástica

A implementação do Exercício 1.2 encontra-se detalhada no arquivo `.ipynb`. O desenvolvimento foi segmentado em duas abordagens:

1. **Cálculo Analítico:** Implementação computacional direta da fórmula binomial deduzida no Exercício 1.1, calculando o valor exato da disponibilidade teórica para os cenários onde $k=1$, $k=\lceil n/2 \rceil$ (maioria absoluta) e $k=n$.
2. **Simulador Estocástico:** Um modelo iterativo vetorizado que executa 10.000 "rodadas" para cada combinação de $n$, $k$ e $p$. A disponibilidade de cada servidor é sorteada independentemente. A proporção de rodadas em que o sistema atinge o quórum $k$ estabelece a "frequência experimental".

Os resultados de ambas as abordagens são compilados em uma tabela comparativa (DataFrame) e visualizados através de gráficos 2D, comprovando a convergência entre a teoria (distribuição binomial) e a prática (frequência das simulações estocásticas).