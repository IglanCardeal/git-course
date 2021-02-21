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
  l = !git log --pretty=format:'%C(green)%h %C(red)%d%C(cyan)%s - %C(white)%cn, %C(yellow)%cr'
  p = !git push origin
```

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

## [Conventional Commits][1]

Convenção criada pelo Angular sobre padronização de mensagens de commits, tornando as mensagens de commits muito mais fáceis de serem identificadas e do quê se trata determinada mensagem de commit.

O link acima contém todos os padrões a serem seguidos.

A lib [`git-commit-msg-linter`][2] para Node.js oferece suporte para as mensgagens de commit e faz um _linter_ (semelhante ao eslint para códigos) para validação dos formatos das mensagens de commit.

```bash
```

[1]: https://www.conventionalcommits.org/en/v1.0.0/
[2]: https://www.npmjs.com/package/git-commit-msg-linter