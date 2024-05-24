# commit-toolbox

![License](https://img.shields.io/github/license/maxxborer/commit-toolbox.svg)
![Build Status](https://github.com/maxxborer/commit-toolbox/workflows/CI/badge.svg)
![Coverage Status](https://coveralls.io/repos/github/maxxborer/commit-toolbox/badge.svg?branch=main)
![npm version](https://img.shields.io/npm/v/commit-toolbox.svg)
![Code Style](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)
![Commitizen](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)
![Downloads](https://img.shields.io/npm/dt/commit-toolbox.svg)

> Node.js CLI library for generating, linting commits and generating CHANGELOG.md with flexible customization, close to [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.4/#summary).

| [English](./README.md) | [Русский](./README_RU.md) |
| ---------------------- | ------------------------------ |

> **⚠️ Attention!** The project is in the development and testing stage. Please report bugs and suggestions in [Issues](https://github.com/maxxborer/commit-toolbox/issues).

## Table of Contents

- [commit-toolbox](#commit-toolbox)
  - [Table of Contents](#table-of-contents)
  - [About the project](#about-the-project)
  - [Requirements](#requirements)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Configuration](#configuration)
    - [Configuration file](#configuration-file)
    - [Configuration fields](#configuration-fields)
      - [Additional fields](#additional-fields)
    - [Configuration example](#configuration-example)
  - [Examples](#examples)
    - [Example of interactive mode output `commit`](#example-of-interactive-mode-output-commit)
    - [git history](#git-history)
    - [CHANGELOG.md](#changelogmd)
  - [Additional](#additional)
    - [Git Hooks](#git-hooks)
  - [Contribution to the project](#contribution-to-the-project)
  - [Thanks for ideas and inspiration](#thanks-for-ideas-and-inspiration)
  - [License](#license)

## About the project

- Generating commits in accordance with Conventional Commits rules and CHANGELOG.md.
- Fine-tuning generation rules.
- Works stably in Windows, Linux and MacOS.
- Supports both CommonJS and ES6 modules.
- Fully typed in TypeScript.
- Fully covered by tests.
- With localization.
- With a minimum number of external dependencies.

## Requirements

- Node.js >= 16
- Git >= 2.13.0

## Installation

| Package Manager | Local                                            | Global                                    |
| --------------- | ------------------------------------------------ | ----------------------------------------- |
| npm             | <pre>npm install --save-dev commit-toolbox</pre> | <pre>npm install -g commit-toolbox</pre>  |
| yarn            | <pre>yarn add --dev commit-toolbox</pre>         | <pre>yarn global add commit-toolbox</pre> |
| pnpm            | <pre>pnpm add --save-dev commit-toolbox</pre>    | <pre>pnpm add -g commit-toolbox</pre>     |
| bun             | <pre>bun add --dev commit-toolbox</pre>          | <pre>bun add -g commit-toolbox</pre>      |

## Usage

> All commands can also be executed via `npx commit-toolbox` / `yarn exec commit-toolbox` / `yarn dlx commit-toolbox` / `pnpx commit-toolbox` / `bun exec commit-toolbox`.

| Command (flag)                                       | Description                                                           | Example                                                                                 | Implementation version |
| ---------------------------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------- | ---------------- |
| `commit`                                             | Commit generation                                                    | <pre>commit-toolbox commit</pre>                                                       | 1.0.0            |
| `commit` `--copy` (`-c`)                             | Commit generation with copying to the buffer and output to the console | <pre>commit-toolbox commit -c</pre>                                                    | 1.0.0            |
| `update` `--patch` (`-p`)                            | Creating a new Git tag version with a patch                          | <pre>commit-toolbox update -p</pre>                                                    | 1.0.0            |
| `update` `--minor` (`-mi`)                           | Creating a new Git tag version with a minor                          | <pre>commit-toolbox update -mi</pre>                                                   | 1.0.0            |
| `update` `--major` (`-ma`)                           | Creating a new Git tag version with a major                          | <pre>commit-toolbox update -ma</pre>                                                   | 1.0.0            |
| `update` `--version` `${custom_tag}` (`-v`)           | Creating a new Git tag version with a custom tag                     | <pre>commit-toolbox update -v v1.0.0</pre>                                             | 1.0.0            |
| `changelog`                                          | CHANGELOG.md generation                                              | <pre>commit-toolbox changelog</pre>                                                    | 1.0.0            |
| `changelog` `--last` (`-l`)                          | CHANGELOG.md generation from the last tag                           | <pre>commit-toolbox changelog -l</pre>                                                 | 1.0.0            |
| `changelog` `--from ${tag_from}` (`-f`)              | CHANGELOG.md generation from the specified tag                       | <pre>commit-toolbox changelog --from v1.0.0</pre>                                      | 1.0.0            |
| `changelog` `--to ${tag_to}` (`-t`)                  | CHANGELOG.md generation to the specified tag                         | <pre>commit-toolbox changelog --to v1.0.0</pre>                                        | 1.0.0            |
| `changelog` `--output ${file_path}` (`-o`)            | CHANGELOG.md generation between the specified tags and saving to a file | <pre>commit-toolbox changelog --from v0.99.0 --to v1.0.0 --output ./CHANGELOG.md</pre> | 1.0.0            |
| `lint` `--message ${passed_message}` (`-m`)           | Commit check for compliance with the rules                           | <pre>commit-toolbox lint -m "TASK-123 ✨ feat(cart): sort products"</pre>              | 1.0.0            |
| `lint` `--last` (`-l`)                               | Check the last commit for compliance with the rules                  | <pre>commit-toolbox lint -l</pre>                                                      | 1.0.0            |
| `lint` `--pre-commit` (`-p`)                         | Check the created commit for compliance with the rules               | <pre>commit-toolbox lint -p</pre>                                                      | 1.0.0            |
| \* `--help` (`-h`)                                   | Help output                                                          | <pre>commit-toolbox --help</pre>                                                       | 1.0.0            |
| \* `--version` (`-v`)                                | Version output                                                       | <pre>commit-toolbox --version</pre>                                                    | 1.0.0            |
| \* `--config` (`-c`)                                 | Command call with specifying the path to the configuration           | <pre>commit-toolbox commit --config ./config.js</pre>                                  | 1.0.0            |
| \* `--show-config` (`-sc`)                           | Full configuration output                                            | <pre>commit-toolbox --show-config</pre>                                                | 1.0.0            |
| \* `--show-default-config`                            | Default configuration output                                         | <pre>commit-toolbox --show-default-config</pre>                                        | 1.0.0            |

## Configuration

### Configuration file

Configuration files are used for configuration:

- `.commit-toolbox.config.js` in the root of the project.
- or `commit-toolbox.config.json` in the root of the project.
- or any other file in formats `.js`, `.cjs`, `.mjs` and `.json` with specifying the path through the `--config` flag.

### Configuration fields

| Field                                    | Description                                                                                                                                                                                                                                   | Type                                                                | Examples                                                                                                                                                                                                                                      |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `convention`                            | Commit generation rules                                                                                                                                                                                                                      | `object`                                                           |                                                                                                                                                                                                                                              |
| `convention.commitTypes`                | Commit types                                                                                                                                                                                                                                | <pre>Array<{ type: string, section: string, emoji: string }></pre> | <pre>[{ type: "feat", section: "Features", emoji: "✨" }]</pre>                                                                                                                                                                              |
| `convention.scopes`               | Commit scopes                                                                                                                                                                                                                              | `string[]`                                                         | <pre>["orders", "products", "settings", "*"]</pre>                                                                                                                                                                                           |
| `convention.allowCustomScopes`          | Custom scopes permission                                                                                                                                                                                                                     | `boolean`                                                          | <pre>true</pre>                                                                                                                                                                                                                              |
| `convention.releaseTagGlobPattern`      | Version pattern                                                                                                                                                                                                                             | `string`                                                           | <pre>"v[0-9]_.[0-9]_.[0-9]\*"</pre>                                                                                                                                                                                                          |
| `convention.useEmoji`                   | Emoji usage in commits and changelog                                                                                                                                                                                                         | `boolean`                                                          | <pre>true</pre>                                                                                                                                                                                                                              |
| `convention.format`                     | Commit format (if you do not specify any key, for example, scope, it will not be requested in interactive mode when calling the `commit-toolbox commit` command and will not be displayed in the commit itself). Also, "task_id" is the task number | `string`                                                           | <pre>"\${task_id}\${emoji}\${type}\${scope}: \${subject}"</pre> <br> where values are wrapped in <br> <pre>[\${task_id}] \${emoji} \${type}(\${scope}): \${subject} \| \${description}</pre> <br> in this case `${description}` on a new line |
| `convention.maxSubjectLength`           | Maximum commit header length                                                                                                                                                                                                               | `number`                                                           | <pre>100</pre>                                                                                                                                                                                                                               |
| `convention.requiredBreakingChanges` | Commit types with required breaking changes                                                                                                                                                                                             | `string[]`                                                         | <pre>["feat", "fix", "perf", "revert"]</pre>                                                                                                                                                                                                 |
| `changelog`                             | CHANGELOG.md generation rules                                                                                                                                                                                                               | `object`                                                           |                                                                                                                                                                                                                                              |
| `changelog.output`                    | CHANGELOG.md file name                                                                                                                                                                                                                     | `string`                                                           | <pre>"CHANGELOG.md"</pre>                                                                                                                                                                                                                    |
| `changelog.commitIgnoreRegex`    | Commit ignore pattern                                                                                                                                                                                                                     | `string`                                                           | <pre>"^WIP "</pre>                                                                                                                                                                                                                           |
| `changelog.commitUrl`                   | Commit URL in CHANGELOG.md                                                                                                                                                                                                                 | `string`                                                           | <pre>"https://gitlab.com/group/project/-/commit/\${commit_hash}"</pre>                                                                                                                                                             |
| `changelog.issueRegex`           | Task pattern                                                                                                                                                                                                                             | `string`                                                           | <pre>"[A-Z][A-Z0-9]+-[0-9]+"</pre>                                                                                                                                                                                                           |
| `changelog.issueUrl`                    | Task URL                                                                                                                                                                                                                                 | `string`                                                           | <pre>"https://app.clickup.com/t/some-team-or-project/\${task_id}"</pre>                                                                                                                                                                      |

#### Additional fields

| Field                            | Description                      | Type      | Examples                                      |
| ------------------------------- | ----------------------------- | -------- | -------------------------------------------- |
| `localization`                  | Localization                   | `object` |                                              |
| `localization.selectType`       | Commit type selection          | `string` | <pre>"Select the type of commit"</pre>       |
| `localization.selectScope`      | Commit scope selection         | `string` | <pre>"Select the scope of commit"</pre>      |
| `localization.enterTaskId`      | Task number input              | `string` | <pre>"Enter the task ID"</pre>               |
| `localization.enterSubject`     | Commit header input            | `string` | <pre>"Enter the subject of commit"</pre>     |
| `localization.enterDescription` | Commit description input       | `string` | <pre>"Enter the description of commit"</pre> |
| `localization.commitGenerated`  | Commit generation message      | `string` | <pre>"Commit generated successfully"</pre>   |
| `localization.useArrowKeys`     | Use arrows                     | `string` | <pre>"Use arrow keys"</pre>                  |
| `localization.default`          | Default                        | `string` | <pre>"default"</pre>                         |
| `localization.required`         | Required                       | `string` | <pre>"required"</pre>                        |
| `localization.optional`         | Optional                       | `string` | <pre>"optional"</pre>                        |
| `localization.from`             | From                           | `string` | <pre>"from"</pre>                            |
| `localization.to`               | To                             | `string` | <pre>"to"</pre>                              |
| `localization.DATE_FORMAT`      | Date format                    | `string` | <pre>"DD.MM.YYYY"</pre>                      |

### Configuration example

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
    format: '${task_id} ${emoji} ${type}${scope}: ${subject}', // Please note that ${description} will be automatically added below
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
            selectType: 'Select the type of commit',
            selectScope: 'Select the scope of commit',
            enterTaskId: 'Enter the task ID',
            enterSubject: 'Enter the subject of commit',
            enterDescription: 'Enter the description of commit',
            commitGenerated: 'Commit generated successfully',
            useArrowKeys: 'Use arrow keys',
            default: 'default',
            required: 'required',
            optional: 'optional',
            from: 'from',
            to: 'to',
            DATE_FORMAT: 'DD.MM.YYYY',
        },
    });
```

## Examples

### Example of interactive mode output `commit`

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

> Example of generating CHANGELOG.md using headers from MR.

```md
# CHANGELOG

## [[v1.0.0](https://gitlab.com/group/project/-/compare/${from_commit_changelog_hash}...${to_commit_changelog_hash}?diff=split)] 07.07.2024

- [ABC-123](https://app.clickup.com/t/00000/ABC-123) add new product
- [ABC-124](https://app.clickup.com/t/00000/ABC-124) add new order
```

## Additional

### Git Hooks

Support for git hooks is intentionally not included in the project, as this may conflict with other plugins. But you can use them in your project.

For example, [husky](https://typicode.github.io/husky/) in package.json:

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

Or [lefthook](https://github.com/evilmartians/lefthook) in .lefthook.yml:

```yaml
pre-commit:
  commands:
    commit-lint:
      run: commit-toolbox lint -p
```

## Contribution to the project

Please read [CONTRIBUTING.md](./.github/CONTRIBUTING.md) for detailed information on our code of conduct and contribution procedures.

## Thanks for ideas and inspiration

- [git-conventional-commits](https://github.com/qoomon/git-conventional-commits)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.4/#summary)
- [Commitizen](https://commitizen-tools.github.io/commitizen/)
- [Commitlint](https://commitlint.js.org/#/)
- [Semantic Release](https://semantic-release.gitbook.io/semantic-release/)

## License

[Apache-2.0](./LICENSE)
