---
title: "Unidade 7: Vetores (Arrays)"
date: 2026-06-21T19:47:54-03:00
draft: false
weight: 3
---


Esta unidade cobre a estrutura de dados homogênea mais básica na Linguagem C: os **Vetores (Arrays)**.

As anotações abaixo foram compiladas com base no **Questionário 5 (Unidades 5, 6 e 7)** (que obteve nota máxima de 50,0/50,0 - 100%, arquivo correspondente: [Questionário 5 (PDF)](file:///home/sl01/go/src/github.com/flccodes/youandi/sources/Questionário%205_Un_5-6-7%20-%20IFSULDEMINAS.pdf)).

---

## 📌 Teoria Essencial sobre Vetores em C

1.  **Definição:** Um vetor é uma estrutura de dados **homogênea** e **estática**:
    *   **Homogênea:** Todos os elementos armazenados são obrigatoriamente do mesmo tipo (por exemplo, todos são `int` ou todos são `float`). Diferente de uma `struct`, um vetor não mistura tipos de dados.
    *   **Estática:** O tamanho do vetor é definido no momento da declaração e não pode ser alterado durante a execução do programa.
2.  **Acesso Direto (Acesso Aleatório):** O tempo necessário para acessar qualquer posição do vetor é constante ($O(1)$). O computador calcula o endereço diretamente a partir do índice:
    $$\text{Endereço} = \text{Endereço Base} + (\text{Índice} \times \text{Tamanho do Tipo})$$
3.  **Índices (A Regra de Ouro):** Em C, um vetor de tamanho $N$ possui índices válidos estritamente de **$0$ a $N-1$**.
    *   Exemplo: `int vetor[10]` tem as posições válidas de `0` a `9`. Acessar `vetor[10]` é um estouro de limite de memória (*buffer overflow*).

---

## 📝 Questões Analisadas & Lições Práticas

### Questão 2: Impressão em Ordem Inversa
*   **Código:**
    ```c
    int vetor[10], i;
    // ... [Leitura do vetor] ...
    for (i = 9; i >= 0; i--) // Comando correto para inverter
        printf(" %d ", vetor[i]);
    ```
*   **Análise de Alternativas e Erros Comuns:**
    *   *❌ O erro de início e limite:* Usar `for (i=10; i>0; i--)` causa dois bugs. Primeiro: tenta ler `vetor[10]`, que está fora do vetor (invasão de memória). Segundo: o loop para antes de chegar no índice `0` (porque a condição é `i > 0`), deixando de imprimir o primeiro valor do vetor.
    *   *✅ A solução correta:* Iniciar no último índice válido (`9`) e incluir a igualdade a zero na condição (`i >= 0`).

### Questão 4: Procurando o Maior Valor
*   **Código correto:**
    ```c
    int vetor[10], i, maior;
    // ... [Leitura] ...
    maior = vetor[0]; // Inicia com o primeiro elemento
    for (i = 1; i < 10; i++) {
        if (maior < vetor[i])
            maior = vetor[i];
    }
    ```
*   **💡 Aprendizado:** Para encontrar o maior ou menor valor, a inicialização da variável `maior` deve ser sempre com um valor do próprio vetor (ex: `vetor[0]`), e não com valores arbitrários (como `100`), garantindo que o programa funcione mesmo que todos os números digitados sejam menores que essa constante arbitrária.

### Questão 7: Teste de Mesa com Atribuição em Cadeia (Anagrama Amazonas)
*   **Enunciado:** Dado o vetor inicial `S M N A O Z A A` (índices 1 a 8) e as instruções de atribuição, determinar o estado final do vetor.
*   **🧠 Rastreamento passo a passo (Tabela de Estados):**

| Linha do Comando | `aux` | `vet[1]` | `vet[2]` | `vet[3]` | `vet[4]` | `vet[5]` | `vet[6]` | `vet[7]` | `vet[8]` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| *Início* | `-` | `S` | `M` | `N` | `A` | `O` | `Z` | `A` | `A` |
| `aux = vet[8]` | **`A`** | `S` | `M` | `N` | `A` | `O` | `Z` | `A` | `A` |
| `vet[8] = vet[1]` | `A` | `S` | `M` | `N` | `A` | `O` | `Z` | `A` | **`S`** |
| `vet[4] = vet[6]` | `A` | `S` | `M` | `N` | **`Z`** | `O` | `Z` | `A` | `S` |
| `vet[6] = vet[3]` | `A` | `S` | `M` | `N` | `Z` | `O` | **`N`** | `A` | `S` |
| `vet[3] = aux` | `A` | `S` | `M` | **`A`** | `Z` | `O` | `N` | `A` | `S` |
| `vet[1] = aux` | `A` | **`A`** | `M` | `A` | `Z` | `O` | `N` | `A` | `S` |

*   **Palavra Final:** **AMAZONAS** (Índices 1 a 8).
*   **⚠️ Lição Prática:** Nunca tente fazer testes de mesa complexos mentalmente. Escrever cada alteração linha por linha em formato de tabela evita "esquecer" de rodar o último comando (como aconteceu ao errar inicialmente o `vet[1] = aux`).

### Questão 9: Equivalência de Loops de Inicialização
*   **Estrutura Original:**
    ```c
    int i=0, a[5];
    for (i = 0; i < 5; i++)
        a[i] = i + 1;
    ```
*   **Estrutura Substituta:**
    ```c
    for (i = 0; i <= 4; i++)
    ```
*   **💡 Aprendizado:** Para variáveis inteiras, `i < 5` é equivalente a `i <= 4`. Ambas as condições iteram exatamente os mesmos índices (`0, 1, 2, 3, 4`). O uso de `i <= 5` causaria invasão de memória no índice `5` do vetor `a[5]`.

### Questão 10: Semântica de Atribuição e Variável Indexadora
*   **Enunciado:** *"A atribuição de valores a um vetor já criado é procedida de elemento em elemento, alterando‐se o valor do índice do vetor."*
*   **Gabarito:** **Verdadeiro**
*   **⚠️ Cuidado com a Terminologia:** Gramaticalmente e logicamente, os índices do vetor permanecem constantes na estrutura física de memória. No entanto, o gabarito oficial considera "alterar o valor do índice" como o ato de **variar o valor da variável indexadora** (ex: incrementar o `i` em um loop) para percorrer o vetor posição por posição.

---

## 📈 Autoavaliação e Plano de Ação

> [!TIP]
> *   **Escrever Sempre:** O erro no anagrama "Amazonas" consolidou que, para provas de lógica estruturada, o papel e a caneta para desenhar tabelas de estados são ferramentas indispensáveis.
> *   **Regra de Limites:** Fixar na mente que a última posição acessível em `tipo vetor[T]` é sempre `T-1`. Loops que terminam com `i <= T` ou iniciam em `i = T` devem ser eliminados de imediato em questões de múltipla escolha.
