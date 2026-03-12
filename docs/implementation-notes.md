# Notas de Implementação

## Objetivo deste documento

Este arquivo registra observações práticas sobre a implementação do protótipo, com foco nas decisões tomadas durante o desenvolvimento, nos componentes efetivamente integrados e nos ajustes feitos para viabilizar a apresentação final do projeto.

O objetivo não é descrever toda a proposta teórica do TCC, mas sim documentar **como o protótipo funcional foi estruturado na prática**.

---

## Estratégia de implementação

Durante o desenvolvimento, o projeto precisou ser ajustado para caber no tempo disponível até a banca.

Por esse motivo, a equipe optou por priorizar um **pipeline funcional ponta a ponta**, mesmo que parte da proposta conceitual não fosse implementada integralmente.

Na prática, isso significou concentrar esforços em:
- receber entrada textual na Unreal,
- obter resposta de um modelo de linguagem,
- transformar essa resposta em áudio,
- enviar o áudio para animação facial,
- apresentar o personagem 3D respondendo na cena.

---

## Organização em dois blocos

A implementação foi organizada em dois blocos principais.

### 1. Bloco Unreal Engine
Responsável por:
- interface do usuário,
- controle dos Blueprints,
- recepção da resposta textual,
- acionamento do fluxo de geração de voz,
- envio do áudio para animação facial,
- apresentação do personagem final.

### 2. Bloco externo de síntese de voz
Responsável por:
- receber texto e áudio de referência,
- gerar o arquivo de saída em `.wav`,
- disponibilizar esse arquivo para o restante do pipeline.

Essa divisão permitiu desacoplar a lógica visual e interativa da lógica de geração de áudio.

---

## Implementação na Unreal Engine

O projeto foi desenvolvido na **Unreal Engine 5.6.1**.

A escolha da Unreal foi importante porque ela permitiu integrar:
- interface de entrada,
- Blueprints para orquestração,
- personagem 3D,
- pipeline de animação facial,
- estrutura de teste visual para demonstração.

A implementação foi baseada principalmente em **Blueprint Visual Scripting**, sistema de programação visual em nós utilizado para construir a lógica do protótipo.

---

## Fluxo do chat

A entrada do usuário acontece por meio de um widget de chat aberto por tecla.

Esse widget envia a mensagem para um `GPTClient`, descrito no projeto como um cliente de modelo de linguagem via API integrado na Unreal.

Pelo padrão adotado, esse cliente foi tratado como um serviço persistente da aplicação, o que é compatível com o uso de **GameInstance Subsystem** para gerenciamento de funcionalidades globais da sessão.

Quando a resposta chega, ela é capturada por um evento de retorno e armazenada em uma variável chamada `Chat Response`, que depois é reutilizada no fluxo de voz.

---

## Fluxo de geração de voz

Após o recebimento da resposta textual, a Unreal encaminha esse conteúdo para um nó de geração de voz.

Esse nó utiliza três parâmetros centrais:
- `Audio Ref Path`,
- `Text`,
- `Output Path`.

Na prática, o fluxo foi construído para permitir que a resposta do LLM fosse convertida em um arquivo `.wav` por um mecanismo externo de síntese de voz executado localmente.

Essa escolha manteve a Unreal como orquestradora do processo, sem embutir a geração de áudio diretamente na engine.

---

## Uso do módulo externo de síntese de voz

O mecanismo de síntese de voz foi executado apenas localmente e tratado como um componente externo ao projeto da Unreal.

Na prática, era executado um script que:
1. recebia o texto,
2. utilizava um áudio de referência,
3. gerava um arquivo `.wav` sintetizado em uma pasta de saída.

Esse arquivo era então consumido pelo restante do pipeline de animação facial.

Por restrições de licença e direitos autorais, esse componente não será distribuído no repositório público, nem seus modelos ou áudios de teste.

---

## Fluxo de Audio2Face

Após a geração do `.wav`, a Unreal encaminhava esse arquivo para o fluxo de **Audio2Face**.

Nos Blueprints, isso foi organizado em uma sequência em que o sistema:
- escolhe um provider ou modelo facial,
- cria uma struct de parâmetros,
- define o caminho do áudio,
- dispara uma tarefa assíncrona de animação.

Esse desenho é coerente com a documentação da NVIDIA, que prevê o uso de funções assíncronas para animar personagens a partir de áudio e manter a sincronização facial.

O projeto utilizou modelos faciais prontos para alimentar o pipeline de animação.

---

## Personagem 3D e apresentação visual

O personagem foi apresentado em uma cena da Unreal como a interface final do sistema.

A ideia do protótipo era fazer com que a resposta do modelo não aparecesse apenas como texto, mas fosse comunicada por um personagem 3D com fala sintetizada e animação facial sincronizada.

Isso tornou o projeto uma experiência multimodal, combinando linguagem natural, voz e representação visual em um único fluxo.

---

## Decisões práticas tomadas no desenvolvimento

Como nem todos os módulos do projeto teórico puderam ser finalizados a tempo, a equipe tomou algumas decisões para garantir uma demonstração funcional.

Entre elas:
- priorizar a integração entre módulos em vez de aprofundar todos os componentes teóricos;
- utilizar respostas em inglês na apresentação final;
- testar o lip sync inicialmente com arquivos de áudio previamente gerados;
- manter toda a execução local;
- trabalhar com ferramentas e modelos já disponíveis para acelerar a validação do protótipo.

Essas escolhas permitiram demonstrar a arquitetura funcionando, ainda que com limitações importantes de desempenho e completude.

---

## Testes de lip sync

Antes da execução ponta a ponta em tempo de demonstração, parte dos testes foi feita com arquivos de áudio previamente gerados.

Isso permitiu validar o comportamento do lip sync e da animação facial na Unreal antes de depender totalmente da geração de áudio em tempo de execução.

Essa etapa foi útil para isolar problemas de integração e verificar se o personagem respondia corretamente ao arquivo de áudio de entrada.

---

## Desempenho observado

Todo o pipeline foi executado localmente.

Na apresentação final, a configuração utilizada foi:
- RTX 5060 Ti 16 GB,
- 16 GB de RAM.

A geração de aproximadamente 15 segundos de áudio levou cerca de 328 segundos, ou aproximadamente 5 minutos.

Esse resultado evidenciou que o gargalo mais forte do protótipo estava no módulo de síntese de voz, comprometendo a possibilidade de interação realmente em tempo real.

---

## O que ficou consolidado

Mesmo sem implementar integralmente toda a proposta conceitual do TCC, o projeto consolidou uma prova de conceito funcional com:
- entrada textual via widget de chat;
- resposta de modelo de linguagem via API;
- encaminhamento da resposta para síntese de voz;
- geração de arquivo `.wav`;
- uso do áudio em pipeline de animação facial;
- apresentação final em personagem 3D na Unreal.

Isso foi suficiente para demonstrar a viabilidade do conceito e os principais desafios de integração entre IA, voz e computação gráfica interativa.


