# Advanced Git

## Níveis de configuração

- `--system`

- `--global`

- `--local`

- Prioridade `local` > `global` > `system`

Lista de configurações:

```bash
git config --list
```

Configurando o editor:

```bash
git config --global core.editor code
```

Abrindo no VSCode:

```bash
git config --global --edit
```

## Alias

Atalhos personalizados no arquivo de config.

```bash
[alias]
  s = !git status
  ss = !git status -s
  c = !git add --all && git commit -m
  l = !git log --pretty=format:'%C(green)%h %C(red)%d %C(cyan)%s %n %C(white)Author: %cn, %C(yellow)%cr %n Date: %ai %n'
  # l = !git log --pretty=reference
  # l = !git log --pretty=fuller
  # l = !git log --pretty=full
  p = !git push origin
```

Para o `--pretty=format:`, vide [docs.](https://git-scm.com/docs/pretty-formats)

## Comandos

- `--amend --no-edit` - Comita um arquivo junto com o commit anterior, ou seja, dois commits diferentes se tornam um só, no caso o commit anterior.

```bash
git commit --amend --no-edit
```

- `stash` - coloca modificações em uma área escondida e esta não entra nos commits seguintes. Isto permite retornar ao estado anterior de como o código foi colocado no stash. Para retornar ao estado que foi colocado no stash, basta rodar o comando `apply`, porém ele não remove o stash da lista. Já o comando `pop`, além de retornar ao estado que foi colocado no stash, ele remove o stash da lista.

```bash
git add .

git stash

git stash list

git stash apply

git stash pop
```

## Tags

Criar tags que podem ser relacionadas a versões, por exemplo. Muito util para melhorar rastreabilidade do projeto e localizar bugs, features, etc...

```bash
git tag 1.0.0
```

Listar as tags:

```bash
git tag [-l, --list]
```

Listar as tags a partir de um pattern:

```bash
git tag -l "v1.0*"
```

Remover uma tag:

```bash
git tag -d [--delete] 1.0.0  
```

Criando tags anotadas ([annotated tag][4]):

```bash
git tag 1.0.0 -m "release 1.0.0"
```

Adicionando tags para commits existentes:

```bash
git tag -a 1.0.0 -m "message" <hash do commit>
```

Enviando para o repositório as tags anotadas:

```bash
git push origin master --follow-tags
```

## [Conventional Commits][1]

Convenção criada pelo Angular sobre padronização de mensagens de commits, tornando as mensagens de commits muito mais fáceis de serem identificadas e do quê se trata determinada mensagem de commit.

O link acima contém todos os padrões a serem seguidos.

A lib [`git-commit-msg-linter`][2] para Node.js oferece suporte para as mensagens de commit e faz um _linter_ (semelhante ao eslint para códigos) para validação dos formatos das mensagens de commit. No caso, ele valida o `type` das mensagens e tamanho das mensagens (_max-len is 100_). Agora para que os commits possam ser concluidas, deve se seguir os padrões do conventional commits exigidos pela lib.

### Trecho [retirado da documentação do Angular][3]:

#### Type

Must be one of the following:

- `build`: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- `ci`: Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
- `docs`: Documentation only changes
- `feat`: A new feature
- `fix`: A bug fix
- `perf`: A code change that improves performance
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `style`: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- `test`: Adding missing tests or correcting existing tests

```bash

```

[1]: https://www.conventionalcommits.org/en/v1.0.0/
[2]: https://www.npmjs.com/package/git-commit-msg-linter
[3]: https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines
[4]: https://git-scm.com/book/en/v2/Git-Basics-Tagging
