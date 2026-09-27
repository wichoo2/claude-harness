---
name: verificar
description: Cómo verificar un cambio de verdad y no con una medición que "corrió pero no midió nada" — gates vs sistema real, trampas del navegador automatizado, grep, tests, deploys y reportes de otras sesiones. Usar antes de decir "funciona", "verificado", "no está" o "está roto".
---

# verificar

Toda verificación responde: **¿esto podía haber fallado?** Si la medición da lo
mismo con el bug que sin el bug, no midió nada.

## 1. Escalera

1. Gates del repo (lint, typecheck, test, build) — necesarios, no suficientes.
2. Contrato: verbo + path + forma de la respuesta contra la fuente (OpenAPI, tipos
   del backend), **por script**, no leyendo.
3. Comportamiento real: llamada al servicio, clic en la app, salida impresa.
4. En el ambiente desplegado: confirmar que corre **el código nuevo** (ver §4).

Reportar hasta qué peldaño se llegó.

## 2. Navegador automatizado

- `form_input` / setear `.value` no llega al estado de React: el diff sale vacío.
  Para probar interacción, **clic real y tipeo real**.
- El árbol de accesibilidad omite elementos que existen. "No está" → confirmar con
  screenshot o `querySelectorAll`.
- Un `window.confirm()` descartado se ve como botón roto: pisarlo con `() => true`,
  clic, restaurar.
- Screenshot reducido no es evidencia de un defecto visual: zoom primero.
- Tras un error no hay refresco: el DOM es una lectura vieja. Para afirmar estado
  tras una mutación, **navegación nueva**.
- Cuando una interacción no produce efecto, comprobar primero que llegó.

## 3. grep y lecturas

- grep matchea líneas: no prueba ausencias en bloques multilínea. Para afirmar que
  falta algo, leer el bloque (`sed -n 'a,bp'`).
- Buscar por lo que el patrón **hace**, no por su nombre (`set…(true)` no encuentra
  `setStatus("x")`).
- Un path puede matchear en texto/documentación. Buscar la llamada real.
- Sin `2>/dev/null`. Confirmar que la ruta existe antes de leer un vacío.
- Filtros de fecha (`git log --since`) que no matchean se leen como "no hay nada".
- Checkout compartido: leer contra `origin/<rama>`, no contra el árbol.

## 4. Tests y deploys

- Un test puede pasar porque el dato elegido no expresa el bug (hora donde UTC y la
  zona local coinciden, id que casualmente coincide). Elegir datos donde el bug
  **sí** cambiaría el resultado.
- Deploy `finished`/`success` ≠ código nuevo sirviendo. Prueba barata: hash de un
  chunk antes/después, o un dato que solo el código nuevo produce.
- Errores de framework engañosos: algunos (p.ej. Fastify) dan 404, no 405, con el
  verbo equivocado.
- Validación antes de transformar (p.ej. Zod 4: `z.email().trim()` no recorta).

## 5. Reportes

- Dos errores de medición se tapan entre sí y dan un reporte confiado. Si "el
  arreglo del otro no funciona", sospechar primero del instrumento propio.
- Mismo rigor para código ajeno que propio: ejecutar y mostrar la salida, o decir
  "no lo ejercité".
- Reporte medio acertado: el hallazgo verdadero presta credibilidad al falso de al
  lado. Separar lo ejercitado de lo inferido.
