---
title: "Migração do Controle de Consumo de Água (CCA)"
subtitle: "De COPASA Monitor para CCA: Relato de migração e discussões arquiteturais em Go"
date: 2026-06-14T15:52:00-03:00
draft: false
description: "Documentação detalhada da reestruturação da aplicação de controle de consumo de água, mudanças nas fórmulas e discussões sobre melhores práticas em Go."
---

# Diário de Aprendizado Go: Migração do Controle de Consumo de Água (CCA)

Este documento registra a evolução e os bastidores técnicos da migração do sistema **COPASA Monitor** para a sua nova etapa evolutiva: **CCA (Controle de Consumo de Água)**. Aqui detalhamos a arquitetura do projeto em Go, as discussões sobre persistência e infraestrutura doméstica gratuita, e o refatoramento matemático das fórmulas.

---

## 📸 Retrato 1: Como Nasceu a Ideia (COPASA Monitor)

O projeto original nasceu com o objetivo prático de automatizar a coleta de dados de consumo de água residencial. O fluxo básico consistia em:
1. Tirar uma foto do hidrômetro pelo celular.
2. Fazer o envio para uma aplicação Go.
3. Usar a API do Gemini (`gemini-1.5-flash`) para extrair os dígitos numéricos (OCR).
4. Calcular o consumo médio diário projetado, variação e estimativa de 30 dias.
5. Sincronizar com uma planilha Google Sheets (aba `atual`) e manter um arquivo CSV local (`COPASA.csv`).

### Os Limites do Primeiro Modelo
* **Branding restrito:** A marca "COPASA" associava o software a uma empresa distribuidora específica (Minas Gerais, Brasil).
* **CSV Não-Padrão:** O arquivo local continha metadados customizados na primeira linha (`Tipo Leitura,DIÁRIA,,,,,,,@,`), impedindo que programas de análise padrão (como Pandas em Python ou PowerBI) abrissem o arquivo sem tratamento prévio de strings.
* **Complexidade no Código:** Lógica de cálculo, interface web, OCR e banco de dados estavam misturados no pacote central `main`, reduzindo a testabilidade do projeto.

---

## 📈 Retrato 2: A Evolução para o CCA

Na segunda fase de testes, o projeto foi refatorado e migrado para o workspace `cca` (`github.com/flccodes/cca`), recebendo as seguintes evoluções de engenharia:

### 1. Novo Branding e Layout Limpo
* Nome alterado para **CCA (Controle de Consumo de Água)**, tornando-o neutro e utilizável em qualquer estado ou país.
* O arquivo CSV local passou a se chamar `sources/CCA - Controle de Consumo de Água - leitura_diaria.csv`.
* Cabeçalhos padronizados iniciando na **Linha 1** (padrão RFC 4180).
* Introdução de uma linha de base limpa (baseline) na **Linha 2** (`.... Initial...`), com os dados efetivos começando na **Linha 3**.

### 2. Separação de Conceitos (Clean Architecture)
Dividiu-se a aplicação em sub-pacotes claros:
* Pacote **`parser`**: Lógica pura de leitura/escrita de CSV, conversões numéricas brasileiras e cálculos matemáticos. Não contém dependências de APIs web ou IA.
* Pacote **`cmd`**: Arquivos de configuração, cliente da API Gemini OCR, cliente da API do Google Sheets (mirando na aba `leitura_diaria`) e servidor web principal.
* Pacote **`web`**: Assets HTML, CSS e JavaScript puros embutidos em tempo de compilação por `go:embed`.

### 3. Ajustes de Fórmulas e Robustez Matemática
As fórmulas foram refinadas para tratar anomalias, como leituras salvas no mesmo timestamp (duração zero) e evitar divisão por zero:

* **Variação (Coluna D):**
  $$Variação = \begin{cases} 0, & \text{se } \Delta t = 0 \text{ e } Horas_{anterior} = 0 \\ Medidor_{atual} - Medidor_{anterior}, & \text{caso contrário} \end{cases}$$
* **Horas Decorridas (Coluna J):**
  $$Horas = \begin{cases} Horas_{anterior}, & \text{se } \Delta t = 0 \\ \Delta t \times 24, & \text{caso contrário} \end{cases}$$
* **Estimado 1d (Coluna E):**
  $$Estimado\ 1d = \begin{cases} 0, & \text{se } Medidor = Variação \text{ e } Horas = 0 \\ 0, & \text{se } Horas = 0 \text{ (evita divisão por zero)} \\ \frac{Variação}{Horas} \times 24, & \text{caso contrário} \end{cases}$$

---

## 🧠 Discussão Filosófica e Arquitetural (Professor & Aprendiz)

### I. Ponto Flutuante vs. Inteiros em Bancos de Dados
Quando tratamos de volumes físicos (como $m^3$ de água com precisão de litros), o uso de ponto flutuante (`float64`) pode introduzir imprecisões decimais devido à representação binária dos computadores.
* **A Recomendação:** Em sistemas profissionais, os valores devem ser tratados como **inteiros** na persistência (banco de dados), representando a menor unidade de medida possível (no caso do CCA, **litros**).
* **Exemplo:** $445,34 \text{ m}^3$ é armazenado como o inteiro $445340$ litros. Ao carregar a tela, faz-se a divisão por 1000 apenas na exibição. Isso remove 100% dos erros acumulados de ponto flutuante em somatórios históricos.

### II. Estratégias de Captura Automatizada de Data e Hora
Para reduzir a fricção do usuário doméstico, foram mapeados três métodos de captura de tempo:
1. **Timestamp Instantâneo:** O JavaScript gera `new Date()` no momento do upload.
2. **Componente Nativo:** Uso do input HTML5 `<input type="datetime-local">`, gerando seletores otimizados pelo sistema operacional do smartphone.
3. **Extração de Metadados EXIF:** Análise do cabeçalho binário da imagem enviada para resgatar a data/hora real de captura da câmera do telefone, independentemente de quando o upload foi feito.

### III. Infraestrutura Gratuita para Aplicações Domésticas
Para quem deseja rodar o monitor sem custos recorrentes:
* **Hospedagem (Backend):**
  * **Raspberry Pi ou Mini PC doméstico:** Executar a aplicação Go localmente. Go consome pouca memória e CPU, permitindo rodar em hardware minimalista de baixo custo de energia elétrica.
  * **Oracle Cloud (Always Free):** Oferece servidores virtuais gratuitos para sempre com excelentes recursos para múltiplos sistemas caseiros.
* **Persistência (Dados):**
  * **SQLite:** Banco de dados relacional de arquivo único local (zero configuração de servidor).
  * **Supabase / Neon / ElephantSQL:** Provedores de PostgreSQL na nuvem com planos gratuitos generosos.
  * **Google Sheets:** Como implementado no projeto, serve como banco de dados visual excelente com gráficos integrados nativos e sem custo.

---

## 🧪 Cobertura de Testes Unitários

Para garantir a estabilidade das fórmulas descritas, foram implementados testes unitários automatizados cobrindo os seguintes cenários:
* Conversão de moedas/floats brasileiros (`FormatFloat` e `ParseFloatBr`).
* Resiliência de data e hora (`ParseDateTime`).
* Mudança matemática com leituras simultâneas em [CalculateNextReading](file:///home/sl01/go/src/github.com/flccodes/cca/parser/calculator.go#L24-L64).
* Roteador de fallback local para gravação no arquivo CSV quando a rede ou credenciais do Google Sheets falham.

*O resultado prático se consolidou em uma aplicação limpa, confiável e modularizada.*
