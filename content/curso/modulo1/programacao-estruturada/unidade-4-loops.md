---
title: "Unidade 4: Estruturas de Repetição"
date: 2026-06-21T19:47:54-03:00
draft: false
weight: 2
---


Esta unidade cobre o funcionamento das três estruturas básicas de iteração da Linguagem C: `while`, `do-while` e `for`.

As anotações abaixo foram compiladas a partir da resolução e correção do **Questionário 4** do IFSULDEMINAS (que obteve nota máxima de 10,0/10,0, arquivo correspondente: [questionario4.pdf](file:///home/sl01/go/src/github.com/flccodes/youandi/sources/questionario4.pdf)).

---

## 📌 Teoria Básica

Na Linguagem C padrão, existem exatamente **três estruturas oficiais de repetição**:
1.  **`while`**: Avalia a condição no **início** do loop. Se for falsa logo de cara, o loop não roda nenhuma vez.
2.  **`do-while`**: Avalia a condição no **fim** do loop. O bloco é executado obrigatoriamente **pelo menos uma vez**.
3.  **`for`**: Consolida inicialização, condição e incremento em uma única linha. Ideal quando sabemos o número exato de repetições.

> [!NOTE]
> Desvios incondicionais usando `goto` podem criar loops, mas o comando `goto` é categorizado oficialmente como um **comando de desvio (jump statement)**, não uma estrutura de repetição.

---

## 📝 Questões Analisadas & Aprendizados

### Questão 1: Quantidade de Comandos de Repetição
*   **Enunciado:** *"Na linguagem C existem três comandos de repetição: `while`, `do-while`, `for`"*
*   **Gabarito:** **Verdadeiro**
*   **⚠️ Erro inicial de interpretação:** Responder falso por não se atentar que o `do-while` é de fato um comando de repetição distinto e nativo do C.

### Questão 3: Rastreamento de Valores Simples
*   **Código:**
    ```c
    int cont = 0, x = 2;
    while (cont < 5) {
        x = x + 10;
        cont++;
    }
    ```
*   **Gabarito:** `x = 52`
*   **🧠 Raciocínio (Teste de Mesa):**
    *   `cont` vai de 0 a 4 (5 iterações).
    *   A cada iteração, soma-se 10 a `x`.
    *   $x_{final} = 2 + (5 \times 10) = 52$.

### Questão 4: Identificação de Erros de Lógica (Loop Infinito)
*   **Código:**
    ```c
    int cont = 0, x = 2;
    while (cont < 5) {
        x = x + 10;
    }
    ```
*   **Gabarito:** **O código do programa possui um erro** (Erro de lógica/Loop infinito).
*   **💡 Aprendizado:** Como a variável de controle `cont` nunca é atualizada dentro do escopo do `while`, a condição `0 < 5` permanece verdadeira para sempre, gerando um loop infinito.

### Questão 5: Escopo do Printf fora do Loop
*   **Código:**
    ```c
    int cont, soma = 0;
    cont = 6;
    while (cont > 0) {
        soma = soma + cont;
        cont--;
    }
    printf("soma = %d\n", soma);
    ```
*   **Gabarito:** `soma = 21`
*   **💡 Aprendizado:** O `printf` está **fora** do corpo do `while` (após a chave de fechamento `}`). Logo, ele só executa uma única vez com a soma total final ($6 + 5 + 4 + 3 + 2 + 1 = 21$).

### Questão 6: A Pegadinha do `if` sem Chaves
*   **Código:**
    ```c
    int cont = 0;
    while (cont < 10) {
        if (cont == 5)
            printf("%d ", cont); // Apenas esta linha pertence ao IF
        printf("%d ", cont);     // Esta linha está fora do IF! Executa sempre
        cont = cont + 1;
    }
    printf("fim.");
    ```
*   **Gabarito:** `0 1 2 3 4 5 5 6 7 8 9 fim.`
*   **⚠️ Pegadinha de Prova:**
    *   Quando omitimos as chaves `{}` após um `if`, **somente a primeira instrução seguinte** fica associada a ele.
    *   Por isso, quando `cont == 5`, o programa imprime o `5` uma vez devido ao `if`, e uma segunda vez devido ao `printf` geral subsequente.
    *   O programa não entra em loop infinito porque `cont = cont + 1` executa normalmente.

### Questão 7: Loops com Parada por Sentinela (Diferente de Zero)
*   **Código incompleto:**
    ```c
    float numero = 1, soma = 0;
    while (numero != 0) { // Bloco 1: diferente de zero
        printf("Digite um número: ");
        scanf("%f", &numero);
        soma = soma + numero;
    }
    printf("Soma = %f", soma); // Bloco 2: printf completo
    ```
*   **💡 Aprendizado:** Para interromper quando o usuário digita `0`, a condição de continuação deve ser *"enquanto o número for diferente de zero"* (`numero != 0`). Se usássemos `== 0`, o loop nem começaria, pois a variável inicia com `1`.

### Questão 8: Contagem de Iterações com Limite Igual a Zero
*   **Código:**
    ```c
    int a = 11;
    while (a >= 0) {
        a--;
        printf("exibe k1\n");
    }
    ```
*   **Gabarito:** `12 vezes`
*   **🧠 Raciocínio Rápido:**
    *   De 11 até 1, temos 11 iterações.
    *   Como a condição aceita a igualdade a zero (`a >= 0`), quando `a` chega valendo `0`, o loop roda pela 12ª vez (o decremento dentro o joga para `-1`, permitindo a saída na verificação seguinte).

### Questão 9: Incremento de 2 em 2
*   **Código:**
    ```c
    int i = 0, x = 3;
    while (i <= 6) {
        x = x + 3;
        i = i + 2;
    }
    ```
*   **Gabarito:** `x = 15`
*   **🧠 Rastreamento Rápido:**
    *   `i` assume os valores: `0` (1ª), `2` (2ª), `4` (3ª), `6` (4ª).
    *   Como `6 <= 6` é verdadeiro, o bloco roda 4 vezes no total.
    *   $x_{final} = 3 + (4 \times 3) = 15$.

### Questão 10: Acúmulo Evitando Valor de Sentinela
*   **Código incompleto:**
    ```c
    float soma = 0, cont = 0, nota = 0; // Bloco 1: float
    while (nota != -1) {                // Bloco 2: condição de parada
        printf("Nota:");
        scanf("%f", &nota);
        if (nota != -1) {               // Bloco 3: evitar somar o -1
            soma = soma + nota;
            cont++;
        }
    }
    printf("Media = %f", soma / cont);  // Bloco 4: cálculo final
    ```
*   **⚠️ Erro comum de lógica:** Achar que o `if` interno deve verificar `== -1`. Se assim fosse, o programa somaria apenas o `-1` e ignoraria as notas válidas. A verificação `if (nota != -1)` garante que o valor sentinela usado para interromper o loop não seja computado na soma ou média final.

---

## 📈 Autoavaliação e Diretrizes de Estudo para Provas

> [!TIP]
> **Técnica para ler códigos sem compilador (Testes Escritos):**
> 1.  **Faça Tabelas de Rastreamento (Teste de Mesa):** Anote em um papel as colunas de cada variável e atualize os valores linha por linha.
> 2.  **Identifique a Sentinela:** Observe claramente qual é o valor que força a saída do loop e garanta que ele não é somado se não for para ser.
> 3.  **Cuidado com as Condições Inclusivas:** Diferencie bem `>` de `>=` ou `<` de `<=`. A igualdade geralmente acrescenta uma iteração inteira à conta.
> 4.  **Atenção ao Bloco Implícito:** Em C, loops e condicionais sem chaves `{}` controlam apenas **uma única linha** abaixo deles.
