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

```bash
[alias]
  s = !git status
  ss = !git status -s
  c = !git add --all && git commit -m
  l = !git log --pretty=format:'%C(green)%h %C(red)%d%C(cyan)%s - %C(white)%cn, %C(yellow)%cr'
  p = !git push origin
```

## Comandos

`--amend --no-edit` - Comita um arquivo junto com o commit anterior, ou seja, dois commits diferentes se tornam um só.

```bash
git commit --amend --no-edit
```
