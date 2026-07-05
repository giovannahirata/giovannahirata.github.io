---
title: "[Weeks 9-(...)] Second phase: Arkanjo contribution"
date: 2026-07-04 22:00:00 -0300
last_modified_at: 2026-07-05 14:25:00 -0300
categories:
  - Free Software Development
  - Second phase
  - Arkanjo contribution
tags:
  - open source
  - free software development
---

# Sobre a contribuição

Juntamente com a minha dupla, Naili, decidimos contribuir para o ArKanjo, uma ferramenta de linha de comando (CLI) que possui um pipeline orquestrado para processar bases de código e analisar similaridades e duplicações entre trechos de código.

Escolhemos a issue #25 que consiste em adicionar um comando para que o usuário pudesse descobrir facilmente onde o ArKanjo guarda os arquivos de cache (/home/user/.cache/arkanjo). Isso ajuda na manutenção manual e na resolução de problemas técnicos.

A funcionalidade deve funcionar da seguinte forma:

```bash
arkanjo preprocessor path
```

Com o output esperado:

```bash
/home/user/.cache/arkanjo
```

Em nossa primeira tentativa, tentamos registrar o comando diretamente no orquestrador global.

Após abrir um PR, o revisor Guilherme Ivo sugeriu ajustes, explicando o padrão de como os comandos da ferramenta estavam estruturadas no projeto e, por isso, não era necessário mexer no main.cpp global. A solução era manter o registro global como nullptr (para que o ArKanjo saiba delegar a tarefa) e implementar a lógica apenas no binário dedicado arkanjo-preprocessor, adicionando o subcomando path lá.

Basicamente, o feedback que recebemos foi direcionado a manter a arquitetura limpa e padronizada, com boas práticas para futuras manutenções no projeto. Seguindo as orientações, removemos as mudanças do main.cpp e orchestrator_commands.hpp preservando o fluxo de execução original, e incluímos uma vírgula à direita (trailing comma) na tabela de comandos para facilitar futuros commits e não bagunçar o histórico do git blame.

Depois dessas correções, com a implementação do comando path dentro do arkanjo-preprocessor e mantendo o orquestrador global apenas como roteador, a nossa contribuição ficou alinhada aos padrões do projeto. E o PR foi aprovado e integrado!