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

```bash

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
