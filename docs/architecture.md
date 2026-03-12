# Arquitetura do Sistema

## Visão geral

O projeto foi estruturado como um sistema multimodal dividido em dois blocos principais:

1. **Projeto na Unreal Engine 5.6.1**
   - interface de chat,
   - Blueprints de orquestração,
   - integração com modelo de linguagem via API,
   - encaminhamento do áudio para animação facial,
   - exibição do personagem 3D.

2. **Módulo externo local de síntese de voz**
   - recebe texto e áudio de referência,
   - gera arquivo `.wav` localmente,
   - entrega o resultado ao fluxo da Unreal e do Audio2Face.

Essa separação permitiu desacoplar a geração textual, a síntese de fala e a animação facial, facilitando testes, manutenção e substituição de componentes.

---

## Componentes principais

### 1. Interface de entrada
A interação do usuário acontece por meio de um widget de chat na Unreal Engine.  
Esse widget é aberto por tecla e permite o envio da mensagem para o módulo de linguagem.

### 2. Cliente de linguagem natural
A Unreal utiliza um `GPTClient`, descrito no projeto como um cliente de modelo de linguagem via API integrado por meio de um **GameInstance Subsystem**.

Esse componente é responsável por:
- enviar a entrada textual do usuário,
- receber a resposta do modelo,
- disponibilizar essa resposta para o restante do fluxo.

### 3. Gerenciamento da resposta
Quando a resposta chega, ela é capturada por um evento de retorno e armazenada em uma variável intermediária chamada `Chat Response`.

Esse valor é então reutilizado na etapa de geração de fala.

### 4. Geração de voz
A resposta textual é enviada para um módulo externo local de síntese de voz.

Esse módulo recebe três entradas principais:
- `Audio Ref Path`,
- `Text`,
- `Output Path`.

Com base nesses parâmetros, o sistema gera um arquivo `.wav` que será utilizado no pipeline de animação facial.

### 5. Animação facial
Após a geração do áudio, a Unreal encaminha o caminho do arquivo para o fluxo de **Audio2Face**.

Nesse estágio, o sistema:
- seleciona um provider ou modelo facial,
- prepara os parâmetros necessários,
- executa a animação do personagem a partir do arquivo de áudio.

### 6. Personagem 3D
O personagem final é exibido na Unreal por meio de um fluxo integrado com **MetaHuman**, utilizado como representação visual da resposta falada.

---

## Fluxo implementado

```text
Texto do usuário
→ Widget de chat na Unreal
→ GPTClient (modelo de linguagem via API)
→ resposta textual
→ armazenamento em Chat Response
→ módulo externo de síntese de voz
→ geração de arquivo .wav
→ Audio2Face
→ MetaHuman
→ resposta falada com animação facial
