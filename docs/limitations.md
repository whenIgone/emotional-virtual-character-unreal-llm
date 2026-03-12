# Limitações do Projeto

## Escopo implementado vs. escopo proposto

O projeto acadêmico foi concebido com uma proposta mais ampla de personagem virtual emocionalmente responsivo, incluindo uma camada de linguagem natural, modulação emocional e apresentação audiovisual integrada.

No entanto, devido ao tempo disponível para desenvolvimento, a versão funcional entregue concentrou-se principalmente na integração entre:
- entrada textual do usuário,
- resposta gerada por um modelo de linguagem via API,
- síntese de voz local,
- animação facial,
- personagem 3D na Unreal Engine.

Assim, parte da arquitetura descrita no trabalho foi implementada como visão de sistema, enquanto a demonstração final priorizou o pipeline funcional de comunicação entre os módulos.

---

## Latência de geração de áudio

Uma das principais limitações observadas no protótipo foi o tempo de geração da fala.

Nos testes da apresentação final, a geração de aproximadamente 15 segundos de áudio levou cerca de **328 segundos**, em uma máquina com **RTX 4060 Ti 16 GB** e **16 GB de RAM**.

Isso mostra que, embora o sistema tenha funcionado como prova de conceito, o comportamento ainda está distante de uma experiência conversacional em tempo real.

---

## Dependência de componentes externos

O pipeline depende de múltiplos componentes externos e heterogêneos, incluindo:
- Unreal Engine,
- cliente de modelo de linguagem via API,
- mecanismo externo de síntese de voz,
- Audio2Face,
- plugins específicos de integração.

Essa dependência torna o ambiente mais sensível a incompatibilidades de versão, instalação incompleta de plugins e diferenças entre máquinas.

---

## Reprodutibilidade limitada

Nem todos os elementos do protótipo podem ser distribuídos publicamente.

Alguns componentes utilizados durante o desenvolvimento e a apresentação final não podem ser incluídos no repositório por motivos de:
- licença,
- direitos autorais,
- uso de conteúdo de terceiros,
- restrições sobre modelos e áudios de teste.

Por esse motivo, o projeto público deve ser entendido principalmente como documentação da arquitetura, do fluxo implementado e dos aprendizados técnicos.

---

## Restrições sobre o módulo de síntese de voz

O módulo de síntese/clonagem de voz utilizado no protótipo foi executado localmente e tratado como um componente externo ao projeto da Unreal.

Esse módulo não será distribuído junto ao repositório, assim como:
- modelos proprietários,
- áudios de referência,
- áudios gerados durante os testes.

Isso limita a reprodução integral do comportamento original do sistema por terceiros.

---

## Dependência de plugins na Unreal

Parte do fluxo na Unreal depende de plugins e integrações específicas.

Na ausência desses plugins, alguns nós dos Blueprints podem aparecer com erro ou deixar de funcionar corretamente, mesmo que a lógica geral do fluxo permaneça documentada.

Essa limitação afeta principalmente a portabilidade imediata do projeto entre ambientes diferentes.

---

## Resposta em inglês na demonstração final

A apresentação final do protótipo foi conduzida com respostas em inglês.

O projeto não foi otimizado para português nessa etapa, o que significa que suporte multilíngue e adaptação mais natural ao contexto brasileiro permanecem como pontos de evolução futura.

---

## Pipeline emocional não completamente finalizado

A proposta conceitual do TCC incluía um personagem emocionalmente responsivo de forma mais profunda.

Entretanto, a implementação final concentrou-se mais na integração entre:
- linguagem natural,
- fala sintetizada,
- animação facial,
- personagem 3D.

Dessa forma, a camada emocional deve ser entendida como uma base conceitual e parcialmente prototipada, com espaço para expansão posterior em direção a respostas mais expressivas e ajustadas ao contexto afetivo.

---

## Execução totalmente local

Todo o sistema foi executado localmente, sem apoio de infraestrutura em nuvem.

Embora isso facilite controle do ambiente e privacidade dos testes, também aumenta a carga computacional na máquina utilizada e contribui para limitações de desempenho, especialmente em tarefas pesadas como geração de voz e animação facial.

---

## Limitação de produto vs. protótipo

Este projeto não deve ser interpretado como produto final ou solução pronta para produção.

Ele deve ser entendido como um **protótipo acadêmico funcional**, desenvolvido para validar integração entre múltiplas tecnologias de IA e computação gráfica, além de explorar os desafios práticos de sistemas multimodais interativos.
