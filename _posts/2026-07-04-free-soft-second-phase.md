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

Correções aplicadas:

- Criação de `src/commands/pre/path/preprocessor_path.cpp`:

```cpp
#include "preprocessor_path.hpp"
#include <arkanjo/base/config.hpp>

PreprocessorPath::PreprocessorPath() { }

bool PreprocessorPath::validate([[maybe_unused]] const ParsedOptions& options) {
    return true;
}

bool PreprocessorPath::run([[maybe_unused]] const ParsedOptions& options) {
    std::cout << Config::config().base_path.string() << "\n";
    return true;
}
```

- Criação de `src/commands/pre/path/preprocessor_path.hpp`:

```cpp
/**
 * @file preprocessor_path.hpp
 * @brief Interface for displaying the cache directory path
 * 
 * Defines the PreprocessorPath command that outputs the absolute path
 * of the ArKanjo cache directory. This is useful for manual maintenance,
 * inspecting cache contents, and troubleshooting.
 */

 #pragma once

 #include <arkanjo/commands/command_base.hpp>
 #include <iostream>

 /**
  * @brief Displays the location of the ArKanjo cache directory
  * 
  * Handles the execution of the 'path' command, which retrieves and
  * cleanly prints the base cache path configuration to standard output.
  */
class PreprocessorPath : public CommandBase<PreprocessorPath> {
public:
    PreprocessorPath();

    COMMAND_DESCRIPTION("Display the location of the ArKanjo cache directory.")

    bool validate(const ParsedOptions& options) override;

    bool run(const ParsedOptions& options) override;
};
```

- Inclusão do novo comando:

```cpp
#include "commands/pre/path/preprocessor_path.hpp"
```

Depois dessas correções, com a implementação do comando path dentro do arkanjo-preprocessor e mantendo o orquestrador global apenas como roteador, a nossa contribuição ficou alinhada aos padrões do projeto. E o PR foi aprovado e integrado!