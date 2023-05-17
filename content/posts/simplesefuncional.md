---
title: "Simples e Funcional"
date: 2023-05-17T18:57:40-03:00
author: "Fernando L. Couto"
tags: ["conhecimento", "kiss", "vim", "math"]
---


A conhecida "Metáfora do Martelo" versa sobre _saber onde usá-lo_. Essa é, de fato, uma habilidade esperada dos profissionais mais experimentados. De forma simples, ouso afirmar que ter conhecimento, _i.e._, apenas saber algo, não é suficiente. É necessário, pois, discenir sobre seu _uso_. Isto denota _ação_ e requer orientação, sentido, ou ainda direcionamento.

Em uma publicação no seu Blog[^1], o engenheiro Fatih Arslan[^2], argumenta que "_usar um único arquivo de configuração `init.lua` ou `vimrc` para o (Neo)Vim é melhor do que uma abordagem de layout de várias pastas/arquivos_". E prima pelo simples. 

Na discussão de fundo, o autor propõe uma comparação entre abordagens ao escrever testes em DAMP (usando _frases descritivas e significativas_) e no modelo DRY (_não se repita_). Esse é um tema muito rico e merece um _post_ específico. Agora, não vou entrar nos detalhes dessa discussão mas me concentrar na proposta de simplicidade na configuração do editor de texto - eu uso Vim, mas você pode pensar nesse sentido em relação à ferramenta de sua preferência. Igualmente, não é minha intenção propor algum modelo, ou formato, de arquivo de configuração para editor de texto - eu vou continuar com o `vimrc`.  

Meu propósito é, a partir dessa premissa de simplicidade, resgatar um trecho de uma conversa que mantive com um empresário onde ele me perguntou "_para você, qual seria a melhor fórmula do Excel_?". A minha resposta, por certo, foi "_aquela que resolve o meu problema_".   

O MS Excel, ou qualquer similar, é uma ferramenta e saber quando, como e onde fazer o uso é o que diferencia os profissionais.

Sendo mais específico e tomando um exemplo trivial, supondo que eu precise calcular o valor futuro de uma quantia, eu não me preocupo em _decorar_ como usar uma função do MS Excel, ou qualquer outra planilha eletrônica. Neste caso, sei que as variaveis são `PV`, `Taxa (i)`, o prazo `n` e preciso obter o `FV`. 

Percebo que há um demasiado foco na ferramenta. Alguns testes e [até mesmo alguns] "especialistas" ocupam-se em saber a ordem do _input_ desses dados na planilhai. Na minha visão, quando estiver de frente para a situação real, mais do que saber usar uma função de uma planilha eletrônica, julgo importante saber aplicar corretamente o conhecimento matemático para atingir o objetivo, neste caso, calcular o valor futuro.  

Seguindo com a proposição, seja o `VP` R$ 100,00, a Taxa `i` de 10% a.a. e o Prazo `n` igual a 3 anos. Fosse eu usar o LibreOffice precisaria invocar a Função `VF`, dentro da categoria `Financeiras`, e inserir:

```
FV = (Taxa;NPER;VP)

FV = (10/100;3;-100)

```

Lado outro, na HP12C a "sintaxe" (para ser _preciso_, a posição sequencial das teclas) é _input_ `n i PV` e `FV` para obter o _output_.  

Excetuando calculadoras (e.g.: HP12C), ao invés de decorar sequência e posição de _input_ eu entendo que, independentemente da ferramenta/planilha, para o problema proposto, o melhor a fazer é usar o conceito matemático de função exponencial e montar a equação como:


$$ FV = PV*(1+i)^n $$
$$ FV = 100*(1+0,1)^3 $$

$$ FV = 133,10 $$


É evidente que _manter a ferramenta bem afiada_ é parte importante do arsenal, mas a avaliação do conhecimento, e de forma mais ampla da capacidade profissional, não pode ficar restrita ao uso desse ou daquele instrumento.   

Logo, a ideia de manter um único arquivo de configuração para meu editor de texto preferido, sem a complexidade de múltiplas pastas/camadas, permite-me resgatar um arquivo, copiar, colar e **imediatamente tornar-me produtivo** em outra máquina que não seja a minha, denota capacidade de _manter a ferramenta pronta para uso_.   

Adicionalmente, não me prendendo às especificidades de uma ferramenta [e.g.: _planilha_], ao ter o domínio da fórmula, e do conhecimento que esta resume, posso resolver uma questão que demande sua aplicação, seja usando o navegador como calculadora ou lápis e papel.   

Saber o que está envolvido em um problema e como abordá-lo é a síntese do pensamento do Professor Polya[^3] em sua atualíssima Obra "A Arte de Resolver Problemas", mas isto será visto em outras notas.



[^1]: [The benefits of using a single configuration file](https://arslan.io/2023/05/10/the-benefits-of-using-a-single-init-lua-vimrc-file/)
[^2]: [https://github.com/fatih/vim-go?ref=arslan.io](https://github.com/fatih/vim-go?ref=arslan.io)
[^3]: Polya, George. 1887-1985.
