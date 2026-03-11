# Emotional Virtual Character with LLM, Speech Synthesis and Unreal Engine

Protótipo de **personagem virtual emocionalmente responsivo** desenvolvido como Trabalho de Conclusão de Curso, integrando **modelo de linguagem via API**, **síntese de fala local**, **Audio2Face** e **MetaHuman** na **Unreal Engine 5.6.1**.

> Este repositório documenta a arquitetura, o fluxo implementado e os aprendizados do projeto.  
> Nem todos os componentes originais podem ser distribuídos, especialmente módulos proprietários de síntese de voz, áudios de teste e determinados assets de terceiros.

---

## Visão geral

O objetivo do projeto foi construir um sistema capaz de receber uma entrada textual, gerar uma resposta com um modelo de linguagem natural e apresentá-la por meio de um personagem 3D com fala sintetizada e animação facial. [file:201][file:202]

A proposta acadêmica completa também considerava um núcleo de computação afetiva, com resposta emocionalmente modulada e maior expressividade do personagem, mas o protótipo final entregue concentrou-se principalmente na integração entre **LLM + Unreal + síntese de voz + Audio2Face + MetaHuman**. [file:201][file:202]

---

## Arquitetura do sistema

O sistema foi organizado em dois blocos principais:

1. **Projeto Unreal Engine**
   - Interface de chat
   - Integração com modelo de linguagem via API
   - Orquestração dos Blueprints
   - Encaminhamento do áudio para animação facial
   - Exibição do personagem 3D

2. **Módulo externo de síntese de voz**
   - Execução local
   - Recebe texto e áudio de referência
   - Gera arquivo `.wav` de saída
   - Entrega o áudio ao fluxo da Unreal/Audio2Face

### Fluxo real implementado

```text
Texto do usuário
→ LLM via API/plugin na Unreal
→ resposta textual
→ Blueprint da Unreal
→ módulo externo de síntese de voz
→ arquivo .wav
→ Audio2Face
→ MetaHuman
→ resposta falada com animação facial
