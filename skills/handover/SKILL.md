---
name: handover
description: Escribir o actualizar el handover técnico de un proyecto (qué es, decisiones tomadas, estado real verificado, qué falta, referencias) para que otra sesión arranque sin re-descubrir. Usar cuando pida "handover", "resumen para otra sesión", "documenta el estado" o al cerrar un bloque grande de trabajo.
---

# handover

Un handover sirve si otra sesión puede actuar con él **sin re-medir lo que ya se
midió** y sin creer lo que nadie midió.

## Dónde

`HANDOVER.md` en la raíz del proyecto (o donde diga su `CLAUDE.md`), importado
desde el `CLAUDE.md` con `@HANDOVER.md`. Si ya existe, actualizarlo; no crear otro.

## Plantilla

```markdown
# <Proyecto> — Handover

**Versión X.Y** · actualizado **AAAA-MM-DD** · deadline: <si hay>

## 1. Qué es
Dos párrafos + tabla de piezas (producto/servicio → repo/carpeta → stack).

## 2. Decisiones tomadas
Tabla: decisión · por qué · alternativa descartada. No se re-abren sin preguntar.

## 3. Estado real (verificado AAAA-MM-DD)
Tabla: decisión/feature · ✅ / 🟡 / ❌ · evidencia (archivo, comando, URL).
"Decidido" ≠ "implementado". Lo no re-verificado se marca como tal.

## 4. Ambientes y cómo correr
URLs, comandos de arranque, dónde están las variables (nunca los valores).

## 5. Qué falta
Tabla: prioridad (🔴/🟠/🟡) · qué · dónde.

## 6. Deuda conocida y trampas
Lo que muerde y no es obvio leyendo el código.

## 7. Referencias
Docs, tableros, specs.
```

## Reglas

- Fechas absolutas, nunca "ayer" / "la semana pasada".
- Cada ✅ con su evidencia. Sin evidencia es 🟡.
- Snapshot viejo que quedó mal en un punto: nota fechada de actualización en esa
  sección, no reescribir la historia en silencio.
- Nada de secretos: ni tokens, ni contraseñas, ni `.env` pegados.
- Corto: si una sección no aporta a la siguiente sesión, se borra.
