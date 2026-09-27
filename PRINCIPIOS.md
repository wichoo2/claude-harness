# Principios de trabajo (todo proyecto)

Siempre cargado desde `~/.claude/CLAUDE.md`. Vale en cualquier repo o carpeta; el
`CLAUDE.md` de cada proyecto manda sobre esto cuando choquen. Skills del plugin
`harness@personal`: `git-flujo`, `verificar`, `handover`.

## 1. Local hasta que diga

- Se edita y se verifica en local. **Sin `git push`, sin PR, sin merge** hasta que
  diga "sube", "abre el PR" o equivalente. Yo pruebo primero.
- No commitear salvo que lo pida. En la duda, cambios sin commitear.
- Al terminar: qué cambió, qué corrió en verde y **qué tengo que probar yo**.

## 2. Verde no es "funciona"

lint / typecheck / test / build prueban que el código es coherente consigo mismo,
no con el sistema real (API, base, dispositivo). "Verificado" solo se dice cuando se
ejercitó el comportamiento: una llamada real, un clic real, la salida impresa. Si no
se ejercitó, decir "no lo ejercité". Detalle en la skill `verificar`.

## 3. Checkout compartido

Puede haber otras sesiones sobre la misma carpeta. Nunca `git checkout`/`switch` en
el checkout principal: rama nueva = `git worktree`. Antes de leer para afirmar algo,
`git branch --show-current`, y medir contra la ref (`git show origin/<base>:<archivo>`),
no contra el árbol. Detalle en `git-flujo`.

## 4. Medir sin mentirse

- Nunca `2>/dev/null` en un grep exploratorio: silencia "esa ruta no existe" y el
  vacío se lee como "no hay nada".
- Un resultado vacío no es un hallazgo negativo hasta confirmar que el filtro/ruta
  podía matchear.
- Un comentario que promete una garantía es una afirmación sin verificar.
- Antes de reportar un defecto en código ajeno: ejecutarlo y mostrar la salida.

## 5. Decisiones

- Una decisión tomada no es una decisión implementada: verificar en el código.
- Decisiones ya cerradas en el `CLAUDE.md` del proyecto no se re-abren sin preguntar.
- Si algo es mío de decidir (negocio, alcance, credenciales), preguntar; lo demás,
  elegir el default sensato y decirlo en una línea.

## 6. Credenciales

No escribo contraseñas ni tokens reales en formularios. Si una verificación lo
necesita, dejo el criterio exacto de qué mirar y lo miro yo.
