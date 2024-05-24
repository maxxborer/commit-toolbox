# commit-toolbox

![License](https://img.shields.io/github/license/maxxborer/commit-toolbox.svg)
![Build Status](https://github.com/maxxborer/commit-toolbox/workflows/CI/badge.svg)
![Coverage Status](https://coveralls.io/repos/github/maxxborer/commit-toolbox/badge.svg?branch=main)
![npm version](https://img.shields.io/npm/v/commit-toolbox.svg)
![Code Style](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)
![Commitizen](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)
![Downloads](https://img.shields.io/npm/dt/commit-toolbox.svg)

> Node.js CLI библиотека для генерации, линтинга коммитов и генерации CHANGELOG.md с гибкой кастомизацией, приближенной к [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.4/#summary).

| [English](./README.md) | [Русский](./README_RU.md) |
| ---------------------- | ------------------------------ |

> **⚠️ Внимание!** Проект находится в стадии разработки и тестирования. Пожалуйста, сообщайте об ошибках и предложениях в [Issues](https://github.com/maxxborer/commit-toolbox/issues).

## Table of Contents

- [commit-toolbox](#commit-toolbox)
  - [Table of Contents](#table-of-contents)
  - [О проекте](#о-проекте)
  - [Требования](#требования)
  - [Установка](#установка)
  - [Использование](#использование)
  - [Конфигурация](#конфигурация)
    - [Файл конфигурации](#файл-конфигурации)
    - [Поля конфигурации](#поля-конфигурации)
      - [Дополнительные поля](#дополнительные-поля)
    - [Пример конфигурации](#пример-конфигурации)
  - [Примеры](#примеры)
    - [Пример вывода интерактивного режима `commit`](#пример-вывода-интерактивного-режима-commit)
    - [git history](#git-history)
    - [CHANGELOG.md](#changelogmd)
  - [Дополнительно](#дополнительно)
    - [Git Hooks](#git-hooks)
  - [Вклад в проект](#вклад-в-проект)
  - [Благодарность за идеи и вдохновение](#благодарность-за-идеи-и-вдохновение)
  - [Лицензия](#лицензия)

## О проекте

- Генерация коммитов в соответствии с правилами Conventional Commits и CHANGELOG.md.
- Тонкая настройка правил генерации.
- Работает стабильно в Windows, Linux и MacOS.
- Поддерживает как CommonJS, так и ES6 модули.
- Полностью типизирован на TypeScript.
- Полностью покрыт тестами.
- С локализацией.
- С минимальным количеством внешних зависимостей.

## Требования

- Node.js >= 16
- Git >= 2.13.0

## Установка

| Package Manager | Local                                            | Global                                    |
| --------------- | ------------------------------------------------ | ----------------------------------------- |
| npm             | <pre>npm install --save-dev commit-toolbox</pre> | <pre>npm install -g commit-toolbox</pre>  |
| yarn            | <pre>yarn add --dev commit-toolbox</pre>         | <pre>yarn global add commit-toolbox</pre> |
| pnpm            | <pre>pnpm add --save-dev commit-toolbox</pre>    | <pre>pnpm add -g commit-toolbox</pre>     |
| bun             | <pre>bun add --dev commit-toolbox</pre>          | <pre>bun add -g commit-toolbox</pre>      |

## Использование

> Все команды можно так же выполнять через `npx commit-toolbox` / `yarn exec commit-toolbox` / `yarn dlx commit-toolbox` / `pnpx commit-toolbox` / `bun exec commit-toolbox`.

| Команда (флаг)                                       | Описание                                                           | Пример                                                                                 | Версия внедрения |
| ---------------------------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------- | ---------------- |
| `commit`                                             | Генерация коммита                                                  | <pre>commit-toolbox commit</pre>                                                       | 1.0.0            |
| `commit` `--copy` (`-c`)                             | Генерация коммита с копированием в буфер и выводом в консоль       | <pre>commit-toolbox commit -c</pre>                                                    | 1.0.0            |
| `update` `--patch` (`-p`)                            | Создание новой версии тега в Git с патчем                          | <pre>commit-toolbox update -p</pre>                                                    | 1.0.0            |
| `update` `--minor` (`-mi`)                           | Создание новой версии тега в Git с минором                         | <pre>commit-toolbox update -mi</pre>                                                   | 1.0.0            |
| `update` `--major` (`-ma`)                           | Создание новой версии тега в Git с мажором                         | <pre>commit-toolbox update -ma</pre>                                                   | 1.0.0            |
| `update` `--version` `${кастомный_тег}` (`-v`)       | Создание новой версии тега в Git с кастомным тегом                 | <pre>commit-toolbox update -v v1.0.0</pre>                                             | 1.0.0            |
| `changelog`                                          | Генерация CHANGELOG.md                                             | <pre>commit-toolbox changelog</pre>                                                    | 1.0.0            |
| `changelog` `--last` (`-l`)                          | Генерация CHANGELOG.md с последнего тега                           | <pre>commit-toolbox changelog -l</pre>                                                 | 1.0.0            |
| `changelog` `--from ${тег_от}` (`-f`)                | Генерация CHANGELOG.md с указанного тега                           | <pre>commit-toolbox changelog --from v1.0.0</pre>                                      | 1.0.0            |
| `changelog` `--to ${тег_до}` (`-t`)                  | Генерация CHANGELOG.md до указанного тега                          | <pre>commit-toolbox changelog --to v1.0.0</pre>                                        | 1.0.0            |
| `changelog` `--output ${путь_к_файлу}` (`-o`)        | Генерация CHANGELOG.md между указанными тегами и сохранение в файл | <pre>commit-toolbox changelog --from v0.99.0 --to v1.0.0 --output ./CHANGELOG.md</pre> | 1.0.0            |
| `lint` `--message ${переданное_сообщение}` (`-m`)    | Проверка коммита на соответствие правилам                          | <pre>commit-toolbox lint -m "TASK-123 ✨ feat(cart): sort products"</pre>              | 1.0.0            |
| `lint` `--last` (`-l`)                               | Проверка последнего коммита на соответствие правилам               | <pre>commit-toolbox lint -l</pre>                                                      | 1.0.0            |
| `lint` `--pre-commit` (`-p`)                         | Проверка создаваемого коммита на соответствие правилам             | <pre>commit-toolbox lint -p</pre>                                                      | 1.0.0            |
| \* `--help` (`-h`)                                   | Вывод справки                                                      | <pre>commit-toolbox --help</pre>                                                       | 1.0.0            |
| \* `--version` (`-v`)                                | Вывод версии                                                       | <pre>commit-toolbox --version</pre>                                                    | 1.0.0            |
| \* `--config` (`-c`)                                 | Вызов команды с указанием пути к конфигурации                      | <pre>commit-toolbox commit --config ./config.js</pre>                                  | 1.0.0            |
| \* `--show-config` (`-sc`)                           | Вывод полной конфигурации                                          | <pre>commit-toolbox --show-config</pre>                                                | 1.0.0            |
| \* `--show-default-config` (`-sdc`)                  | Вывод дефолтной конфигурации                                       | <pre>commit-toolbox --show-default-config</pre>                                        | 1.0.0            |

## Конфигурация

### Файл конфигурации

Для конфигурации используются файлы:

- `.commit-toolbox.config.js` в корне проекта.
- или `commit-toolbox.config.json` в корне проекта.
- или любой другой файл в форматах `.js`, `.cjs`, `.mjs` и `.json` с указанием пути через флаг `--config`.

### Поля конфигурации

| Поле                                    | Описание                                                                                                                                                                                                                                   | Тип                                                                | Примеры                                                                                                                                                                                                                                      |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `convention`                            | Правила генерации коммитов                                                                                                                                                                                                                 | `object`                                                           |                                                                                                                                                                                                                                              |
| `convention.commitTypes`                | Типы коммитов                                                                                                                                                                                                                              | <pre>Array<{ type: string, section: string, emoji: string }></pre> | <pre>[{ type: "feat", section: "Features", emoji: "✨" }]</pre>                                                                                                                                                                              |
| `convention.scopes`               | Скоупы коммитов                                                                                                                                                                                                                            | `string[]`                                                         | <pre>["orders", "products", "settings", "*"]</pre>                                                                                                                                                                                           |
| `convention.allowCustomScopes`          | Разрешение кастомных скоупов                                                                                                                                                                                                               | `boolean`                                                          | <pre>true</pre>                                                                                                                                                                                                                              |
| `convention.releaseTagGlobPattern`      | Паттерн версии                                                                                                                                                                                                                             | `string`                                                           | <pre>"v[0-9]_.[0-9]_.[0-9]\*"</pre>                                                                                                                                                                                                          |
| `convention.useEmoji`                   | Использование эмоджи в коммитах и changelog                                                                                                                                                                                                | `boolean`                                                          | <pre>true</pre>                                                                                                                                                                                                                              |
| `convention.format`                     | Формат коммита (если не указать какой-либо ключ, например scope, он не будет запрашиваться в интерактивном режиме при вызове команды `commit-toolbox commit` и не будет отображаться в самом коммите). Так же "task_id" - это номер задачи | `string`                                                           | <pre>"\${task_id}\${emoji}\${type}\${scope}: \${subject}"</pre> <br> где значения оборачиваются в <br> <pre>[\${task_id}] \${emoji} \${type}(\${scope}): \${subject} \| \${description}</pre> <br> при этом `${description}` на новой строке |
| `convention.maxSubjectLength`           | Максимальная длина заголовка коммита                                                                                                                                                                                                       | `number`                                                           | <pre>100</pre>                                                                                                                                                                                                                               |
| `convention.requiredBreakingChanges` | Типы коммитов с обязательными breaking changes                                                                                                                                                                                             | `string[]`                                                         | <pre>["feat", "fix", "perf", "revert"]</pre>                                                                                                                                                                                                 |
| `changelog`                             | Правила генерации CHANGELOG.md                                                                                                                                                                                                             | `object`                                                           |                                                                                                                                                                                                                                              |
| `changelog.output`                    | Имя файла CHANGELOG.md                                                                                                                                                                                                                     | `string`                                                           | <pre>"CHANGELOG.md"</pre>                                                                                                                                                                                                                    |
| `changelog.commitIgnoreRegex`    | Паттерн игнорирования коммитов                                                                                                                                                                                                             | `string`                                                           | <pre>"^WIP "</pre>                                                                                                                                                                                                                           |
| `changelog.commitUrl`                   | URL коммита в CHANGELOG.md                                                                                                                                                                                                                 | `string`                                                           | <pre>"https://gitlab.com/group/project/-/commit/\${commit_hash}"</pre>                                                                                                                                                             |
| `changelog.issueRegex`           | Паттерн задачи                                                                                                                                                                                                                             | `string`                                                           | <pre>"[A-Z][A-Z0-9]+-[0-9]+"</pre>                                                                                                                                                                                                           |
| `changelog.issueUrl`                    | URL задачи                                                                                                                                                                                                                                 | `string`                                                           | <pre>"https://app.clickup.com/t/some-team-or-project/\${task_id}"</pre>                                                                                                                                                                      |

#### Дополнительные поля

| Поле                            | Описание                      | Тип      | Примеры                                      |
| ------------------------------- | ----------------------------- | -------- | -------------------------------------------- |
| `localization`                  | Локализация                   | `object` |                                              |
| `localization.selectType`       | Выбор типа коммита            | `string` | <pre>"Select the type of commit"</pre>       |
| `localization.selectScope`      | Выбор скоупа коммита          | `string` | <pre>"Select the scope of commit"</pre>      |
| `localization.enterTaskId`      | Ввод номера задачи            | `string` | <pre>"Enter the task ID"</pre>               |
| `localization.enterSubject`     | Ввод заголовка коммита        | `string` | <pre>"Enter the subject of commit"</pre>     |
| `localization.enterDescription` | Ввод описания коммита         | `string` | <pre>"Enter the description of commit"</pre> |
| `localization.commitGenerated`  | Сообщение о генерации коммита | `string` | <pre>"Commit generated successfully"</pre>   |
| `localization.useArrowKeys`     | Использование стрелок         | `string` | <pre>"Use arrow keys"</pre>                  |
| `localization.default`          | По умолчанию                  | `string` | <pre>"default"</pre>                         |
| `localization.required`         | Обязательно                   | `string` | <pre>"required"</pre>                        |
| `localization.optional`         | Не обязательно                | `string` | <pre>"optional"</pre>                        |
| `localization.from`             | От                            | `string` | <pre>"from"</pre>                            |
| `localization.to`               | До                            | `string` | <pre>"to"</pre>                              |
| `localization.DATE_FORMAT`      | Формат даты                   | `string` | <pre>"DD.MM.YYYY"</pre>                      |

### Пример конфигурации

```js
import { createCommitToolbox } from 'commit-toolbox';

export default createCommitToolbox({
  convention: {
    commitTypes: [
      {
        type: 'feat',
        description: 'A new feature',
        section: 'Features',
        emoji: '✨',
      },
      {
        type: 'fix',
        description: 'A bug fix',
        section: 'Bug Fixes',
        emoji: '🐛',
      },
      {
        type: 'perf',
        description: 'A code change that improves performance',
        section: 'Performance Improvements',
        emoji: '🚄',
      },
      {
        type: 'refactor',
        description: 'A code change that neither fixes a bug nor adds a feature',
        section: 'Refactor',
        emoji: '🔨',
      },
      {
        type: 'style',
        description:
          'Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)',
        section: 'Style',
        emoji: '💄',
      },
      {
        type: 'test',
        description: 'Adding missing tests or correcting existing tests',
        section: 'Tests',
        emoji: '🧪',
      },
      {
        type: 'build',
        description: 'Changes that affect the build system or external dependencies',
        section: 'Build System',
        emoji: '👷',
      },
      {
        type: 'docs',
        description: 'Documentation only changes',
        section: 'Documentation',
        emoji: '📚',
      },
      {
        type: 'chore',
        description: "Other changes that don't modify src or test files",
        section: 'Chores',
        emoji: '🧹',
      },
      {
        type: 'merge',
        description: 'Merge branch',
        section: 'Merges',
        emoji: '🔀',
      },
      {
        type: 'revert',
        description: 'Revert to a commit',
        section: 'Reverts',
        emoji: '⏪',
      },
    ],
    scopes: ['orders', 'products', 'settings', '*'],
    allowCustomScopes: true,
    releaseTagGlobPattern: 'v[0-9]*.[0-9]*.[0-9]*',
    useEmoji: true,
    format: '${task_id} ${emoji} ${type}${scope}: ${subject}', // учтите, что вниз автоматически добавится ${description}
    requiredBreakingChanges: ['feat', 'fix', 'perf', 'revert'],
    maxSubjectLength: 100,
  },
  changelog: {
    output: 'CHANGELOG.md',
    commitIgnoreRegex: '^WIP ',
    commitUrl: 'https://gitlab.com/group/project/-/commit/${commit_hash}',
    issueRegex: '[A-Z][A-Z0-9]+-[0-9]+',
    issueUrl: 'https://app.clickup.com/t/00000/${task_id}',
  },
  localization: {
    selectType: 'Выберите тип коммита',
    selectScope: 'Выберите скоуп коммита',
    enterTaskId: 'Введите ID задачи',
    enterSubject: 'Введите заголовок коммита',
    enterDescription: 'Введите описание коммита',
    commitGenerated: 'Коммит сгенерирован успешно',
    useArrowKeys: 'Используйте стрелки',
    default: 'по умолчанию',
    required: 'обязательно',
    optional: 'не обязательно',
    from: 'от',
    to: 'до',
    DATE_FORMAT: 'DD.MM.YYYY',
  },
});
```

## Примеры

### Пример вывода интерактивного режима `commit`

```text
❔ Select the type of commit: (Use arrow keys)
❯   ✨ feat:      A new feature
    🐛 fix:       A bug fix
    🚄 perf:       A code change that improves performance
    🔨 refactor:  A code change that neither fixes a bug nor adds a feature
    💄 style:     Changes that do not affect the meaning of the code
    🧪 test:      Adding missing tests or correcting existing tests
    👷 build:     Changes that affect the build system or external dependencies
    📚 docs:      Documentation only changes
    🧹 chore:     Other changes that don't modify src or test files
    🔀 merge:     Merge branch
    ⏪ revert:    Revert to a commit
# ---------------------------------------------------------------------------- #
❔ Select the scope of commit: (Use arrow keys)
❯ orders
  products
  settings
  *
# ---------------------------------------------------------------------------- #
❔ Enter the task ID: (e.g. TASK-123) # default = branch name
❯
# ---------------------------------------------------------------------------- #
❔ Enter the subject of commit: # required (0/100)
❯
# ---------------------------------------------------------------------------- #
❔ Enter the description of commit: # optional
❯
# ---------------------------------------------------------------------------- #
✅ Commit generated successfully
ABC-123 ✨ feat(cart): add new product
| Add new product to the cart
```

### git history

```bash
ABC-123 ✨ feat(cart): add new product
| Add new product to the cart
```

### CHANGELOG.md

> Пример генерации CHANGELOG.md с использованием заголовков из MR.

```md
# CHANGELOG

## [[v1.0.0](https://gitlab.com/group/project/-/compare/${from_commit_changelog_hash}...${to_commit_changelog_hash}?diff=split)] 07.07.2024

- [ABC-123](https://app.clickup.com/t/00000/ABC-123) add new product
- [ABC-124](https://app.clickup.com/t/00000/ABC-124) add new order
```

## Дополнительно

### Git Hooks

Поддержка git hooks намеренно не включена в проект, так как это может привести к конфликтам с другими плагинами. Но вы можете использовать их в своем проекте.

Например, [husky](https://typicode.github.io/husky/) в package.json:

```json
{
  "husky": {
    "hooks": {
      "commit-msg": "commit-toolbox lint -p",
      "prepare-commit-msg": "commit-toolbox lint -p"
    }
  }
}
```

Или [lefthook](https://github.com/evilmartians/lefthook) в .lefthook.yml:

```yaml
pre-commit:
  commands:
    commit-lint:
      run: commit-toolbox lint -p
```

## Вклад в проект

Пожалуйста, прочитайте [CONTRIBUTING.md](./.github/CONTRIBUTING.md) для получения подробной информации о нашем кодексе поведения и процедурах внесения вклада в проект.

## Благодарность за идеи и вдохновение

- [git-conventional-commits](https://github.com/qoomon/git-conventional-commits)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.4/#summary)
- [Commitizen](https://commitizen-tools.github.io/commitizen/)
- [Commitlint](https://commitlint.js.org/#/)
- [Semantic Release](https://semantic-release.gitbook.io/semantic-release/)

## Лицензия

[Apache-2.0](./LICENSE)
