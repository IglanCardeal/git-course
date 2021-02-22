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
  c = !git add --all &git commit -m
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
git tag -d [--delete] "<tagname>"
```

```bash
git tag -d "v1.0.0"
```

Remover uma tag do repositório remoto:

```bash
git push --delete origin "<tagname>"
```

```bash
git push --delete origin "v1.0.0"
```

Criando tags anotadas ([annotated tag][4]):

```bash
git tag 1.0.0 -m "release 1.0.0"
```

Adicionando tags para commits existentes:

```bash
git tag -a 1.0.0 -m "message" <commit hash>
```

Enviando para o repositório incluindo as tags anotadas:

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

## Revert & Reset

### Reset

Basicamente é um operação que desfaz o `git add`, ele remove os arquivos da staged area e move-os para o unstaged area novamente, mantendo o estado que eles estavam antes de entrar na staged area.

Exemplo:

```bash
$ git ss
M README.md # red
?? test.js # red
```

```bash
git add .
```

```bash
$ git ss
M README.md # green
A test.js # green
```

removendo o `test.js` da staged area:

```bash
$ git reset test.js
Unstaged changes after reset:
M       README.md
```

```bash
$ git ss
M README.md # green
?? test.js # red
```

Para _resetar_ todos os arquivos da staged area, basta não passar nenhum parâmetro:

```bash
$ git add . && git ss
M README.md # green
A test.js # green
```

```bash
$ git reset
M README.md # red
?? test.js # red
```

O `reset` também pode reverter commits de uma branch. Podemos _resetar_ e voltar para o estado de um commit anterior, passando a hash do commit desejado:

```bash
git reset <commit hash>
```

Podemos também fazer um `reset` indicando quantos (em número) commits anteriores eu quero voltar, ou seja, quantos commits eu quero que o HEAD aponte para trás. Exemplos:

```bash
git reset HEAD~1 # commit anterior
```

```bash
git reset HEAD~2 # 2 commits atrás
```

Uma coisa interessante do `~1` é que ele pode ser usado com outras referências, como branches (exemplo: `master~1`) e até hash de commit (pode usar se copiou o hash da linha de cima, ou está fazendo um script que recebe a hash do commit, mas o comando precisaria do commit anterior). Outra coisa é que se o número desejado for 1, o mesmo pode ser ocultado (exemplo: `HEAD~`).

O `reset` possui 3 níveis para _resetar_:

- `--soft`: para os arquivos relacionados a todos os commits acima do commit apontado no `git reset <hash>`, ele remove os arquivos relacionandos aos commits da branch e coloca eles novamente na staged area.

  Ex:

```bash
git reset 8d89344 --soft
```

- `--mixed`: (default) ele remove os arquivos relacionandos aos commits acima da branch e coloca eles novamente no workspace (unstaged area) além de manter o estado que eles estavam na unstaged area. Exemplo:

```bash
git add .
```

```bash
git c "test: testing git reset and revert"
```

```bash
$ git reset HEAD~1 --mixed
Unstaged changes after reset:
M       README.md
```

```bash
$ git ss
M README.md # red
?? test.js # red
```

- `--hard`: ele remove os arquivos relacionandos aos commits acima da branch e ainda remove os arquivos do projeto para aqueles que estavam como _untracked_ na unstaged area e desfaz as alterações dos arquivos que estavam como modificados (`M`) tambem na unstaged area.

```bash
git reset 8d89344 --hard
```

```bash

```

[1]: https://www.conventionalcommits.org/en/v1.0.0/
[2]: https://www.npmjs.com/package/git-commit-msg-linter
[3]: https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines
[4]: https://git-scm.com/book/en/v2/Git-Basics-Tagging
