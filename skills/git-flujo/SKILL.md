---
name: git-flujo
description: Flujo git general — de dónde cortar una rama, worktrees en checkouts compartidos, cómo integrar a ambientes, PRs que se mergean solos o quedan out-of-date. Usar en cualquier tarea que cree una rama, integre trabajo, abra un PR o pregunte "¿de dónde corto?" / "¿cómo subo esto?".
---

# git-flujo

Si el `CLAUDE.md` del proyecto define otro flujo, gana el del proyecto.

## 1. Antes de tocar nada

```bash
git branch --show-current
git status --short
git worktree list
git fetch --prune
```

- Cambios sin commitear que no son míos → no tocarlos, avisar.
- Worktrees ajenos vivos → hay otras sesiones; aplica §2 sí o sí.

## 2. Rama nueva = worktree

Nunca `git checkout -b` en el checkout principal: le cambia la rama a todas las
sesiones que lo comparten.

```bash
git worktree add ../wt-<repo>-<tema> -b <tipo>/<tema> origin/<base>
cd ../wt-<repo>-<tema>
# install real (no symlinkear node_modules de otra rama)
# copiar a mano .env / .env.local si existen y no están versionados
```

- `<base>`: la rama de la que nacen las features en ese repo (normalmente `main`).
  Si no está claro, mirar `git log --oneline -5 origin/main origin/dev` y preguntar.
- Nombres: `feature/`, `fix/`, `chore/`, `docs/` + minúsculas con guiones.
- Al terminar: `git worktree remove <dir>` (la rama sobrevive).

## 3. Integrar a ambientes (si el repo tiene dev/qa/stg)

La feature nace de la base y **sube** por los ambientes; nunca absorbe un ambiente.

```bash
# ✗ estando en la feature
git merge origin/dev    git rebase origin/dev    git pull origin dev
```

Una feature que absorbió `dev` se lleva puesto todo lo no liberado de otros. Si la
feature quedó vieja, se actualiza desde la base (`git rebase origin/main`), nunca
desde un ambiente. Conflictos se resuelven en el ambiente, una vez.

Excepción: si un PR a un ambiente sale "out-of-date" y el repo exige rama al día,
mergear ese ambiente en la rama, verificar, pushear. Nunca force-push.

## 4. Push y PR — solo con orden explícita

Ver `PRINCIPIOS.md §1`. Cuando la haya:

1. Gates del repo en verde (los que existan: lint, typecheck, test, build).
   Rojo = no se sube.
2. Antes de pushear un commit de seguimiento a una rama con PR:
   ```bash
   gh pr view <n> --json state,mergedAt
   ```
   Si ya está `MERGED`, el push queda huérfano: comprobar con
   `git merge-base --is-ancestor <sha> origin/<base>` y abrir PR nuevo con lo que falte.
3. Cambio que cruza repos: mismo nombre de rama en todos y se sube en orden de
   dependencia (librería/contrato → backend → frontends).

## 5. Prohibido sin pedirlo

`push --force`, `reset --hard` sobre trabajo ajeno, borrar ramas remotas, reescribir
historia publicada, `--no-verify`.
