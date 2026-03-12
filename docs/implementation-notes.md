# Notas de Implementação

## Objetivo do protótipo

O protótipo foi desenvolvido para validar a integração entre um modelo de linguagem, um mecanismo externo de síntese de voz, um pipeline de animação facial e um personagem 3D na Unreal Engine.

Em vez de focar apenas em um modelo isolado, a implementação teve como prioridade colocar em funcionamento um fluxo completo de interação:
- entrada textual,
- resposta gerada por LLM,
- conversão da resposta em áudio,
- sincronização da fala com um personagem virtual.

---

## Organização prática do projeto

Durante o desenvolvimento, o projeto foi organizado em dois blocos principais:

1. **Projeto Unreal Engine 5.6.1**
   - cena principal,
   - personagem 3D,
   - Blueprints,
   - widget de chat,
   - integração com cliente de linguagem,
   - encaminhamento do áudio para animação facial.

2. **Módulo externo local de síntese de voz**
   - executado separadamente da Unreal,
   - responsável por receber texto e um áudio de referência,
   - geração de arquivo `.wav` para consumo posterior no pipeline.

Essa divisão permitiu trabalhar a Unreal como orquestradora da experiência, enquanto a geração de voz foi mantida como um serviço externo acoplado por arquivo.

---

## Implementação do fluxo de chat

Na Unreal, a interação começa com um **widget de chat** aberto por tecla.

A mensagem inserida pelo usuário é enviada para um `GPTClient`, descrito no projeto como um cliente de modelo de linguagem integrado à Unreal. Pelas evidências dos Blueprints, esse cliente foi organizado como um subsistema persistente da aplicação, o que é compatível com o uso de **GameInstance Subsystem** para serviços globais de sessão.

Quando a resposta chega, ela é tratada por um evento específico (`OnResponseReceived_Event`) e armazenada em uma variável intermediária chamada `Chat Response`, utilizada nas etapas seguintes do pipeline.

---

## Implementação da geração de voz

Após o recebimento da resposta textual, a Unreal encaminha esse conteúdo para um nó de geração de voz.

Esse nó utiliza três parâmetros centrais:
- `Audio Ref Path`
- `Text`
- `Output Path`

Na prática, o fluxo foi construído para permitir que a resposta do LLM fosse convertida em um arquivo `.wav` por um mecanismo externo de síntese/clonagem de voz executado localmente.


# Notas de Implementação

## Objetivo deste documento

Este arquivo registra observações práticas sobre a implementação do protótipo, com foco nas decisões tomadas durante o desenvolvimento, nos componentes efetivamente integrados e nos ajustes feitos para viabilizar a apresentação final do projeto.

O objetivo não é descrever a proposta teórica completa do TCC, mas sim documentar **como o protótipo funcional foi estruturado na prática**.

***

## Estratégia de implementação

Durante o desenvolvimento, o projeto precisou ser ajustado para caber no tempo disponível até a banca.

Por esse motivo, a equipe optou por priorizar um **pipeline funcional ponta a ponta**, mesmo que parte da proposta conceitual do TCC não fosse implementada integralmente.

Na prática, isso significou concentrar esforços em:
- receber entrada textual na Unreal,
- obter resposta de um modelo de linguagem,
- transformar essa resposta em áudio,
- enviar o áudio para animação facial,
- apresentar o personagem 3D respondendo na cena.

***

## Organização em dois blocos

A implementação foi organizada em dois blocos principais:

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

Essa divisão foi importante porque permitiu desacoplar a lógica visual/interativa da lógica de geração de áudio.

***

## Implementação na Unreal Engine 5.6.1

O projeto foi desenvolvido na **Unreal Engine 5.6.1**.

A escolha da Unreal foi importante porque ela permitiu integrar:
- interface de entrada,
- Blueprints para orquestração,
- personagem 3D,
- pipeline de animação facial,
- estrutura de teste visual para demonstração.

A implementação foi baseada principalmente em **Blueprint Visual Scripting**, que na Unreal é um sistema de programação visual em nós, adequado para construir lógica de gameplay e integração entre módulos.

***

## Fluxo do chat

A entrada do usuário acontece por meio de um widget de chat aberto por tecla.

Esse widget envia a mensagem para um componente chamado `GPTClient`, descrito no projeto como um cliente de modelo de linguagem via API integrado na Unreal.

Pelo padrão observado, esse cliente está associado a um **GameInstance Subsystem**, o que faz sentido para um serviço persistente durante toda a sessão da aplicação. A própria Unreal define subsystems como classes instanciadas automaticamente com ciclo de vida gerenciado, e `UGameInstanceSubsystem` é apropriado para funcionalidades com escopo global da aplicação.

Quando a resposta chega, ela é capturada por um evento (`OnResponseReceived_Event`) e armazenada em uma variável chamada `Chat Response`, que depois é reutilizada no fluxo de voz.

***

## Fluxo de geração de voz

A etapa de geração de voz foi organizada como uma chamada a um módulo externo local de síntese/clonagem de voz.

No Blueprint, o nó de geração recebe três parâmetros centrais:
- `Audio Ref Path`
- `Text`
- `Output Path`

O `Audio Ref Path` aponta para um arquivo de referência de voz.  
O campo `Text` recebe o conteúdo da variável `Chat Response`.  
O `Output Path` define onde o novo arquivo `.wav` será salvo.

Essa estrutura mostra que o pipeline foi pensado para transformar a resposta textual em áudio de maneira desacoplada da Unreal, mantendo a engine como orquestradora do processo.

***

## Uso do módulo externo de TTS

O mecanismo de síntese de voz foi executado apenas localmente e tratado como um componente externo ao projeto da Unreal.

Na prática, era executado um script que:
1. recebia o texto,
2. utilizava um áudio de referência,
3. gerava um arquivo `.wav` sintetizado em uma pasta de saída.

Esse arquivo era então consumido pelo restante do pipeline de animação facial.

Por restrições de licença e direitos autorais, esse componente não será distribuído no repositório público, nem seus modelos ou áudios de teste.

***

## Fluxo de Audio2Face

Após a geração do `.wav`, a Unreal encaminhava esse arquivo para o fluxo de **Audio2Face**.

Nos Blueprints, isso aparece como uma sequência em que:
- o sistema escolhe um provider/modelo facial,
- cria uma struct de parâmetros,
- define o caminho do áudio,
- dispara uma tarefa assíncrona de animação.

Essa lógica é coerente com a documentação da NVIDIA, que recomenda o uso de nós assíncronos como `Animate Character from Wav File Async` para evitar bloqueio do fluxo principal enquanto o áudio é enviado ao Audio2Face.

O projeto também utilizou modelos faciais prontos, como a configuração baseada na “Claire”, para alimentar o pipeline de animação.

***

## Personagem 3D e apresentação visual

O personagem foi apresentado em uma cena da Unreal como a interface final do sistema.

A ideia do protótipo era fazer com que a resposta do modelo não aparecesse apenas como texto, mas fosse comunicada por um personagem 3D com fala sintetizada e animação facial sincronizada.

Isso foi importante para aproximar o projeto de uma experiência multimodal, combinando linguagem natural, voz e representação visual em um único fluxo.

***

## Decisões práticas tomadas no desenvolvimento

Como nem todos os módulos do projeto teórico puderam ser finalizados a tempo, a equipe tomou algumas decisões de implementação para garantir a demonstração funcional do sistema.

Entre essas decisões:
- priorizar a integração entre módulos em vez de aprofundar todos os componentes teóricos,
- utilizar respostas em inglês na apresentação final,
- testar o lip sync inicialmente com arquivos de áudio já gerados,
- manter toda a execução local,
- trabalhar com ferramentas e modelos já disponíveis para acelerar a validação do protótipo.

Essas escolhas permitiram demonstrar a arquitetura funcionando, ainda que com limitações importantes de desempenho e completude.

***

## Testes de lip sync

Antes da execução final ponta a ponta, parte dos testes foi feita com arquivos de áudio previamente gerados.

Isso permitiu validar o comportamento do lip sync e da animação facial na Unreal antes de depender totalmente da geração de áudio em tempo de execução.

Essa etapa foi útil para isolar problemas de integração e verificar se o personagem respondia corretamente ao arquivo de áudio de entrada.

***

## Desempenho observado

Todo o pipeline foi executado localmente.

Na apresentação final, a configuração utilizada foi:
- **RTX 4060 Ti 16 GB**
- **16 GB de RAM**

Nos logs do sistema, a geração de aproximadamente **15 segundos de áudio** levou cerca de **328 segundos**, ou aproximadamente **5 minutos**.

Esse resultado evidenciou que o gargalo mais forte do protótipo estava no módulo de síntese de voz, comprometendo a possibilidade de interação realmente em tempo real.

***

## O que ficou consolidado

Mesmo sem implementar integralmente toda a proposta conceitual do TCC, o projeto consolidou uma prova de conceito funcional com:

- entrada textual via widget de chat;
- resposta de modelo de linguagem via API;
- encaminhamento da resposta para síntese de voz;
- geração de arquivo `.wav`;
- uso do áudio em pipeline de animação facial;
- apresentação final em personagem 3D na Unreal.

Isso foi suficiente para demonstrar a viabilidade do conceito e os principais desafios de integração entre IA, voz e computação gráfica interativa.

***

## Conclusão técnica

A principal contribuição prática desta implementação foi validar a integração entre módulos heterogêneos em um sistema multimodal interativo.

Mais do que treinar um único modelo, o projeto exigiu coordenação entre:
- cliente de linguagem natural,
- geração de fala,
- animação facial,
- Blueprints,
- Unreal Engine,
- personagem 3D.

Esse tipo de integração tornou o projeto especialmente relevante como estudo de arquitetura aplicada a IA interativa.
