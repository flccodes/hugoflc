---
title: "Guia Master: Recomendações e Práticas de Debug em Go"
date: 2026-06-16T21:46:00-03:00
author: "Antigravity"
subtitle: "Estratégias eficazes para diagnosticar e corrigir problemas em aplicações Go"
description: "Um guia prático sobre testes unitários, formatação de logs, uso do Delve e tratamento de erros de API para desenvolvedores Go."
---

# Guia Master: Depurando em Go (Debug)

Depurar código em Go de forma eficiente requer a combinação de boas práticas estruturais da linguagem e o conhecimento das ferramentas corretas. Abaixo estão as recomendações essenciais de depuração, abordando desde testes automatizados até o uso de debuggers interativos.

---

## 1. O Poder dos Testes Unitários (`go test`) como Ferramenta de Debug

Em Go, a melhor forma de depurar um comportamento estranho não é rodar o servidor web inteiro e tentar simular a ação no navegador. O caminho mais rápido e eficiente é isolar a lógica em um teste automatizado.

* **Isolar o teste:** Ao notar um erro em uma função de cálculo ou manipulação de dados, abra o arquivo de testes correspondente (ex: `calculator_test.go`), adicione uma massa de dados que reproduza o problema e execute apenas aquele teste específico:
  ```bash
  go test -v -run TestCalculateNextReading ./parser/...
  ```
* **Debug Visual nos Testes:** Se o resultado for diferente do esperado, enriqueça a saída usando a função `t.Logf()` do pacote `testing`. Ela é ideal pois só é impressa no terminal se o teste falhar ou se você passar explicitamente a flag `-v`.

---

## 2. Print Debugging Inteligente (`%+v` e `%#v`)

Quando precisar imprimir dados no console para inspecionar structs ou variáveis complexas:

* **Use `%+v`**: Imprime a estrutura mostrando o nome de cada campo juntamente com seu valor.
  ```go
  log.Printf("Leitura recebida: %+v", reading)
  // Saída: Leitura recebida: {Timestamp:2026-06-16 08:00:00 MeterValue:473.02}
  ```
* **Use `%#v` (Ainda melhor)**: Imprime o valor em formato de sintaxe Go literal, exibindo o tipo exato e ponteiros. É fantástico para distinguir se uma string vazia é de fato `""` ou se um campo está como `nil`.
  ```go
  log.Printf("Configuração carregada: %#v", config)
  ```

---

## 3. Utilizando o Delve (`dlv`) — O Debugger Oficial do Go

O **Delve** é a ferramenta padrão do ecossistema Go para depurar o código linha por linha diretamente pela linha de comando ou integrado a IDEs.

* **Depurar os testes:** Você pode inicializar o depurador diretamente no contexto dos seus testes unitários:
  ```bash
  dlv test ./parser
  ```
* **Depurar o executável principal:** Para iniciar o depurador na aplicação web (a partir do ponto de entrada `main.go`):
  ```bash
  dlv debug ./cmd
  ```
* **Comandos principais do Delve no terminal:**
  * `break main.go:195` (ou `b`): Define um ponto de parada (breakpoint) na linha 195.
  * `continue` (ou `c`): Executa o código até atingir o próximo breakpoint.
  * `next` (ou `n`): Avança para a próxima linha no mesmo escopo/arquivo.
  * `step` (ou `s`): Entra para dentro de uma função que está sendo chamada na linha atual.
  * `print <variavel>` (ou `p`): Exibe o valor de uma variável em tempo de execução.
  * `locals`: Lista todas as variáveis locais e seus valores no escopo atual.

---

## 4. Validação de Configurações e APIs Externas

Muitos problemas em produção decorrem de variáveis de ambiente mal configuradas ou payloads inesperados de APIs externas.

* **Sanitização de logs de config:** Implemente logs que validem a existência e o tamanho das variáveis carregadas em arquivos `.env` na inicialização, mas evite expor segredos.
  ```go
  if len(config.GeminiAPIKey) > 6 {
      log.Printf("Gemini API Key carregada com sucesso (sufixo: ...%s)", config.GeminiAPIKey[len(config.GeminiAPIKey)-4:])
  } else {
      log.Println("AVISO: Gemini API Key ausente ou muito curta!")
  }
  ```
* **Inspeção detalhada de erros de API:** Clientes HTTP e bibliotecas oficiais (como as do Google API) retornam objetos de erros detalhados. Evite engolir ou simplificar o erro. Faça o log completo de `err.Error()` para entender se a falha é de rede, de quota ou de permissão de acesso (`403 Forbidden`).
