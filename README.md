# Emotional Virtual Character with LLM, Speech Synthesis and Unreal Engine

Protótipo de **personagem virtual emocionalmente responsivo** desenvolvido como Trabalho de Conclusão de Curso, integrando **modelo de linguagem via API**, **síntese de fala local**, **Audio2Face** e **MetaHuman** na **Unreal Engine 5.6.1**.

> Este repositório documenta a arquitetura, o fluxo implementado e os aprendizados do projeto.  
> Nem todos os componentes originais podem ser distribuídos, especialmente módulos proprietários de síntese de voz, áudios de teste e determinados assets de terceiros.

---

## Visão geral

O objetivo do projeto foi construir um sistema capaz de receber uma entrada textual, gerar uma resposta com um modelo de linguagem natural e apresentá-la por meio de um personagem 3D com fala sintetizada e animação facial.

A proposta acadêmica completa também considerava uma camada emocional mais ampla, mas o protótipo final entregue concentrou-se principalmente na integração entre **LLM + Unreal + síntese de voz + Audio2Face + MetaHuman**.

---

## Fluxo do sistema

```text
Texto do usuário
→ Widget de chat na Unreal
→ modelo de linguagem via API
→ resposta textual
→ módulo externo de síntese de voz
→ arquivo .wav
→ Audio2Face
→ MetaHuman
→ resposta falada com animação facial
```

## Documentação adicional
Para mais detalhes, consulte:

docs/architecture.md — arquitetura do sistema e fluxo entre módulos

docs/implementation-notes.md — decisões práticas de implementação

docs/limitations.md — restrições técnicas, computacionais e de distribuição

docs/references.md — referências conceituais e documentação oficial

## Privacidade e licenciamento
Este repositório tem finalidade acadêmica e de portfólio.

A ênfase desta publicação está na arquitetura, no fluxo implementado e nos aprendizados técnicos.

## Disponibilidade do código

Os arquivos completos do projeto na Unreal Engine e determinados componentes externos não estão incluídos nesta versão pública por questões de:
- privacidade,
- restrições de licença,
- uso de conteúdo de terceiros,
- e segurança na publicação de caminhos, assets e integrações locais.

## Autores
Projeto desenvolvido como Trabalho de Conclusão de Curso em grupo.

