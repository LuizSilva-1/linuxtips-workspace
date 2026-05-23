# Migração de Config Legacy em Massa
Migra URLs do banco legado pra novo em 17 arquivos via vim/sed/awk. 3 com KEEP_OLD ficam como estão.
---
## Instrucoes

# ✏️ Migração de Config Legacy em Massa

> **Cenário:** Sua empresa migrou o banco. URL nova:
> `postgres://app:senha@db-new.empresa.local:5433/...`. URL velha:
> `postgres://app:senha@db-old.empresa.local:5432/...`. **17 arquivos**
> em `/etc/configs-legacy/` precisam migrar. **3 outros** (marcados com
> `KEEP_OLD` no comentário) **devem ficar como estão** porque ainda
> apontam pra ambiente de testes legacy.

## Suas missões

1. **Edite um arquivo via vim** usando substituição por comando
   (`:%s/old/new/g`) — pelo menos 1 arquivo modificado assim.
2. **Faça a migração em massa via sed** — `-i` pra editar in-place.
   Cuidado: não migra os 3 com KEEP_OLD.
3. **Valide** com `grep -r 'db-old' /etc/configs-legacy/` — devem sobrar
   só os 3 KEEP_OLD.
4. **Valide também** que `db-new` aparece em 17 arquivos (`grep -rl`).
5. **Configure seu vimrc** (`~/.vimrc`) com pelo menos 4 settings úteis
   (numbers, syntax, indent, search highlight).
6. **Use awk** pra extrair colunas do `/tmp/usuarios.csv` — gera lista
   de emails dos roles "admin".

## Dicas

- vim subst: `:%s/old/new/g` (todos no arquivo). `c` no fim pra confirmar
  cada um: `:%s/old/new/gc`.
- sed seguro com filtro: `grep -L KEEP_OLD /etc/configs-legacy/*.conf | xargs sed -i 's|...|...|g'`. Lembra: `-L` lista os que NÃO têm.
- vim tip: `vimtutor` te leva 30min e vale ouro.
- awk: `awk -F, '$4=="admin"{print $3}' /tmp/usuarios.csv`.

## Tom

Módulo 3 é o que separa quem "tem medo do vim" de quem **é produtivo**
no shell. Cada minuto investido aqui rende pra vida toda.

---
*Desafio LINUXtips - devops-pro*