---
title: "html/template"
subtitle: "Estudos sobre templates HTML seguros em Go"
layout: "section"
---

# html/template

O pacote `html/template` implementa templates baseados em dados para gerar saídas HTML seguras contra injeção de código (XSS). 

O Hugo utiliza este mesmo sistema de templates sob o capô para renderizar os layouts das páginas a partir dos arquivos Markdown.

### Ementa / Tópicos de Estudo
* Sintaxe básica de Go templates: `{{ . }}` e `{{ .Campo }}`
* Estruturas de controle: `{{ range }}`, `{{ if }}`, `{{ with }}`
* Pipelines e funções de template (ex: `printf`, `safeHTML`, `urlize`)
* Context-aware escaping (segurança e sanitização automática de HTML, JS, CSS e URLs)

---

### Modelo de Cabeçalho (Front Matter) para Novas Notas

Sempre que quiser criar um novo arquivo de estudos, como nesta pasta (ex: `estudo01.md`), usar o cabeçalho abaixo no início do arquivo:

```yaml
---
title: "Título do seu Estudo"
date: 2026-06-13T20:11:00-03:00
draft: false
tags: ["Go", "HTML", "Template", "Web"]
---

# Escrever o conteúdo aqui...
```
