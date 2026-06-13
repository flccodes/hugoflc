---
title: "Diretrizes de Commits e Fluxo Git"
date: 2026-06-13T20:35:00-03:00
draft: false
tags: ["Git", "Workflow", "Boas Práticas"]
---

# Diretrizes para Commits Atômicos e Fluxo de Trabalho

Esta página serve como diretriz para a organização e versionamento deste repositório de aprendizado. Ela resume a nossa conversa sobre a melhor forma de lidar com o Git no dia a dia.

---

## 1. O que é um "Commit Atômico"

Um commit é considerado **atômico** quando representa **uma única unidade lógica de trabalho completa**.

* **Evite commits fragmentados:** O Git não rastreia pastas vazias (apenas arquivos). Fazer um commit de uma pasta vazia (com apenas arquivos temporários) ou deixar alterações pela metade quebra o histórico de build.
* **Evite commits gigantes:** Misturar coisas completamente diferentes (como reescrever o currículo, adicionar notas de matemática e mexer no layout do site) no mesmo commit dificulta reverter ou alterar apenas uma parte no futuro.

---

## 2. A Regra de Ouro: Uma Tarefa, Um Commit

O ideal é agrupar ações relacionadas que fazem sentido juntas:

* **Bom agrupamento (Um commit único):**
  `feat: cria seção de matemática e migra o post simples e funcional`
  *(Se no futuro você quiser desativar/reverter a seção de matemática, basta dar revert nesse único commit e tudo relacionado a ela some de forma limpa).*
* **Outro bom agrupamento (Outro commit separado):**
  `feat: adiciona notas de slices em golang`
  *(Isso é independente da matemática, então deve ser feito em outro momento).*

---

## 3. Nosso Fluxo de Trabalho (Pair Programming)

Para documentar o aprendizado passo a passo como um diário:

1. **Trabalhar em Branches de Desenvolvimento:** Mantemos a branch `main` limpa e pronta para publicação/deploy. Criamos branches como `feat-estrutura-curso` para novas seções ou revisões.
2. **Organização Automática:** Você não precisa se preocupar em solicitar um commit para cada pequena ação técnica. A IA organizará as mudanças em blocos lógicos e fará os commits com mensagens semânticas claras.
3. **Merge na `main`:** Quando um ciclo de estudos ou reestruturação estiver concluído, fazemos o merge na `main` e realizamos o `git push` para enviar as atualizações ao GitHub.
