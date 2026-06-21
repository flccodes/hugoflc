---
title: "Diário de Estudos - Redes de Computadores"
date: 2026-06-21T19:47:54-03:00
draft: false
weight: 1
---

**Curso Técnico em Desenvolvimento de Sistemas — IFSULDEMINAS**
**Aluno:** Hugo (Espaço de Trabalho: `hugoflc`)

---

## 🛠️ Trilha Tecnológica: Automação de Transcrições
Nesta seção, registramos o progresso no desenvolvimento do script para extração de transcrições do YouTube.

### O Script: [extract_transcripts.py](file:///home/sl01/go/src/github.com/flccodes/redes/extract_transcripts.py)
Desenvolvido em Python para extrair de forma assíncrona as transcrições das videoaulas e estruturar o material para estudo no *NotebookLM*.

> [!TIP]
> **Boas Práticas de Engenharia de Software:**
> 1. **Ambiente Virtual Isolado (`.venv`):** Foi criado um ambiente virtual independente para a disciplina de Redes de Computadores. Isso garante que as dependências (como `youtube-transcript-api`) fiquem isoladas, prevenindo conflitos com outros projetos (como a disciplina de *Programação Estruturada*).
> 2. **Reaproveitamento e CLI (Command Line Interface):** O script foi parametrizado usando a biblioteca `argparse`. Agora ele aceita argumentos no terminal para que possa ser reaproveitado em qualquer outra matéria bastando passar as bandeiras `-i` (input), `-u` (unidades) e `-o` (pasta de saída).

#### Resolução de Desafios e Ajustes no Script:
1. **Bloqueio de IP por IPv6 (YouTube Rate Limit):** 
   * **Problema:** O YouTube bloqueia requisições de raspagem/transcrições vindas de blocos de IP domésticos e de datacenters usando IPv6.
   * **Solução:** Aplicamos um *patch* em nível de socket (`socket.getaddrinfo`) para forçar a resolução DNS exclusivamente em IPv4. Isso contornou com sucesso o mecanismo de bloqueio.
2. **Incompatibilidade de Tipos da API:**
   * **Problema:** Dependendo da versão da biblioteca `youtube-transcript-api`, os trechos retornados mudavam entre estruturas de dicionário (`dict`) ou objetos tipados (`FetchedTranscriptSnippet`), gerando erros de indexação (`TypeError: object is not subscriptable`).
   * **Solução:** Implementamos validação por reflexão (`hasattr`) para suportar de forma híbrida tanto acessos a propriedades (`item.start`) quanto a chaves de dicionário (`item['start']`).
3. **Resiliência de Sintaxe do Parser:**
   * **Problema:** Um link da Unidade 4 estava quebrado no arquivo de dados (`sources/mod_01_redes.txt`) por falta do colchete de abertura (`[`).
   * **Solução:** A expressão regular de varredura foi modificada para tornar o colchete inicial opcional (`\[?`), garantindo que nenhum vídeo fosse ignorado.

---

## 📚 Trilha de Aprendizagem Teórica e Questionários

### 🌐 Unidade 4: Protocolos de Camada de Rede (Internet) e Diagnósticos
*Foco no estudo do endereçamento lógico (IP) e ferramentas de teste de conectividade.*
* **Arquivo de Questões:** [Atividade Avaliativa IV_ Redes - IFSULDEMINAS.pdf](file:///home/sl01/go/src/github.com/flccodes/redes/sources/Atividade%20Avaliativa%20IV_%20Redes%20-%20IFSULDEMINAS.pdf)

#### 📝 Questões e Erros Evitados:
* **Q1 (Coexistência IPv4 e IPv6):** 
  * *Conceito:* O IPv4 **não foi desativado** nem totalmente substituído pelo IPv6. Ambos coexistem ativamente na internet através de técnicas de pilha dupla (Dual Stack), tunelamento e tradução. 
  * *Status:* **Falso**.
* **Q2 (Validação de Endereço IPv4):** 
  * *Conceito:* Endereços IPv4 usam 32 bits divididos em 4 octetos decimais de 8 bits. Portanto, cada número obrigatoriamente varia entre **0 e 255**. 
  * *Ajuste:* O endereço `1.255.316.14` é **inválido** porque o terceiro octeto contém o número `316` (excedendo 255).
* **Q3 (Classes de Endereçamento Clássico):**
  * *Associação:*
    * **Classe A:** `1.0.0.0 - 126.255.255.255`
    * **Classe B:** `128.0.0.0 - 191.255.0.0`
    * **Classe C:** `192.0.0.0 - 223.255.255.0`
    * **Classe D (Multicast):** `224.0.0.0 - 239.255.255.255`
    * **Classe E (Testes/Pesquisa):** `240.0.0.0 - 255.255.255.254`
* **Q4 (Endereço IPv6):**
  * *Conceito:* O IPv6 possui **128 bits**, representados em notação hexadecimal dividida em **8 quartetos** separados por dois pontos (`:`), criada especificamente para suprir o esgotamento do IPv4.
* **Q5 (Protocolo ICMP):**
  * *Conceito:* O **ICMP** (Internet Control Message Protocol) reporta erros de tráfego de dados e é a base de ferramentas essenciais de diagnóstico como `ping` (envio de requisições de Eco) e `traceroute` (mapeamento de rotas usando manipulação de TTL).

---

### 🔌 Unidade 5: Meios de Transmissão Físicos e Redes PAN/Wireless
*Foco na estrutura física (cabos e ondas) que suporta a passagem dos dados.*
* **Arquivo de Questões:** [Atividade Avaliativa V_ Redes - IFSULDEMINAS.pdf](file:///home/sl01/go/src/github.com/flccodes/redes/sources/Atividade%20Avaliativa%20V_%20Redes%20-%20IFSULDEMINAS.pdf)

#### 📝 Questões e Erros Evitados:
* **Q1 (Crimpagem de Cabo Crossover):** 
  * *Conceito:* Para ligar dois computadores diretamente (dispositivos iguais) sem switch, cruzam-se os pinos de TX (transmissão) e RX (recepção). Uma ponta usa o padrão T568A e a outra T568B.
  * *Ajuste:* Pinos 1 e 2 na ponta A (T568A) ligam os fios **Branco Verde – Verde**. Na ponta B (T568B) ligam os fios **Branco Laranja – Laranja**. 
* **Q2 e Q3 (Conexão Direta e Padrões):**
  * *Conceito:* O cabo cruzado é chamado de **crossover** e serve para unir dois dispositivos de rede da mesma camada (PC-PC ou Switch-Switch) diretamente.
* **Q4 (Imunidade a Ruídos - UTP vs FTP):**
  * *Conceito:* O cabo UTP (Unshielded Twisted Pair) não possui blindagem metálica, sendo **menos imune** (mais vulnerável) a interferências eletromagnéticas do que o cabo FTP (Foiled Twisted Pair), que possui uma folha de metal protetora.
* **Q5 (Meios Guiados vs Não Guiados):**
  * *Conceito:* Cabos (coaxiais, par trançado e fibra óptica) são **meios guiados** porque confinam o sinal dentro de uma estrutura física. Redes wireless são meios não guiados.
* **Q6 (Redes de Área Pessoal - PAN):**
  * *Conceito:* **PAN** define a interconexão de dispositivos de uso pessoal em curtíssimo alcance. As tecnologias sem fio de área pessoal (WPAN) mais comuns são **Bluetooth** e redes locais de uso pessoal temporário via **Wi-Fi** (tethering/Wi-Fi Direct).
* **Q7 (Fibra Óptica - Monomodo vs Multimodo):**
  * *Conceito:* Fibras **Monomodo (SMF)** têm o núcleo finíssimo (8 a 10 mícrons) permitindo que a luz trafegue de forma reta usando **LASERs**. Isso gera pouca atenuação e alcança longas distâncias (até 50 km). Fibras **Multimodo (MMF)** têm núcleo maior (62.5 mícrons), usam **LEDs**, ricocheteiam a luz e limitam-se a curtas distâncias (~2.5 km).
* **Q8 (Padrão Sem Fio 802.11a):**
  * *Conceito:* O padrão 802.11a opera exclusivamente na faixa de **5 GHz**, atinge taxas de transmissão de até **54 Mbps** e faz uso de multiplexação **OFDM**.

---

### 🎛️ Unidade 6: Dispositivos de Interconexão e Comutação
*Foco nos equipamentos que controlam a entrega e o fluxo dos pacotes de dados.*
* **Arquivo de Questões:** [Atividade Avaliativa VI_ Redes - IFSULDEMINAS.pdf](file:///home/sl01/go/src/github.com/flccodes/redes/sources/Atividade%20Avaliativa%20VI_%20Redes%20-%20IFSULDEMINAS.pdf)

#### 📝 Questões e Erros Evitados:
* **Q1 e Q3 (Associação de Dispositivos):**
  * **Roteador:** Toma decisões lógicas e calcula a melhor rota entre redes distintas (Camada 3).
  * **Switch:** Encaminha quadros ponto a ponto usando endereços MAC físicos de destino (Camada 2).
  * **Bridge (Ponte):** Segmenta e interliga segmentos de rede local de mesmo protocolo, agindo como um "repetidor inteligente" de pacotes (Camada 2).
  * **Repetidor:** Amplifica/regenera o sinal físico para aumentar a extensão (Camada 1).
  * **Gateway:** Interliga e traduz dados entre redes de protocolos e arquiteturas totalmente distintos.
* **Q4 (Diferença Crucial entre Roteador e Switch - *Ponto de Queda*):**
  * *Conceito:* **O Switch comum não faz roteamento** (seleção de melhores rotas). Ele comuta quadros dentro da mesma rede. Quem faz a "seleção lógica de rotas de dados" é o Roteador. A afirmativa "Switch faz seleção lógica de melhores rotas" está incorreta.
* **Q5 (Hub vs Switch):**
  * *Conceito:* O hub envia todos os dados recebidos para todas as portas de saída (*broadcast* elétrico), gerando colisões de pacotes. O switch aprende a tabela MAC e envia os dados diretamente para a porta do destinatário (*unicast*), eliminando colisões e otimizando a largura de banda.

---

> [!IMPORTANT]
> **Visão de Negócios e Administração:**
> Para um profissional corporativo ou administrador de sistemas que não lida diretamente com cabos e crimpagem no dia a dia, a compreensão lógica destas camadas e dispositivos (especialmente a transição IPv4/IPv6, a diferença de segurança/performance entre Hubs e Switches, e o papel de gateway/roteador) é essencial ao estruturar projetos de infraestrutura, avaliar custos de cabeamento e planejar a segurança da informação corporativa.
