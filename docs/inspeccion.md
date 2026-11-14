# Acta de inspección por pares

## Datos de la sesión

- Autor: Jorge Rivera
- Revisor: Jorge Rivera
- Registrador: Jorge Rivera
- Fecha: 14/11/2026
- Modalidad: individual, con los tres roles asumidos por la misma persona en momentos separados

Revisé el código de `src/descuentos.js` tal como me llegó, los requisitos y la salida de ESLint (`evidencias/lint-inicial.txt`). No ejecuté el código. Como guía usé el [checklist](checklist.md).

Hice la inspección solo, así que separé los roles para no mezclarlos. Primero, como autor, describí qué hace cada línea. Después, como revisor, recorrí el código pregunta por pregunta con el checklist, como si lo hubiera escrito otra persona. Al final, como registrador, anoté cada hallazgo con su severidad. Para la severidad usé este criterio:

- Alta: el problema puede terminar en un cobro equivocado.
- Media: puede causar un comportamiento inesperado o hace difícil verificar el módulo.
- Baja: afecta la legibilidad o el mantenimiento, pero no cambia el resultado.

## Hallazgos de ESLint

ESLint marcó cinco errores en el código inicial. Los revisé uno por uno y los confirmé todos.

| ID | Regla | Línea:col | Evidencia | Severidad | Acción | Estado |
|---|---|---|---|---|---|---|
| E-01 | `no-var` | 2:3 | `var descuento` | Baja | Cambiar a `let`, porque la variable se modifica en la línea 3. | Corregido |
| E-02 | `eqeqeq` | 3:13 | `esVip == true` | Media | Cambiar a `===`. Ver H-07. | Corregido |
| E-03 | `curly` | 3:22 | `if` sin llaves | Baja | Agregar llaves. | Corregido |
| E-04 | `curly` | 4:25 | `if` sin llaves | Baja | Agregar llaves. | Corregido |
| E-05 | `no-var` | 5:3 | `var total` | Baja | Cambiar a `const`, porque no se vuelve a asignar. | Corregido |

La regla `no-unused-vars` no encontró nada, ya que todas las variables se usan. La dejé activa para futuros cambios. Después de corregir volví a ejecutar `npm run lint` y ya no aparecieron errores (`evidencias/lint-final.txt`).

## Hallazgos de la revisión humana

Estos problemas ESLint no los detecta, porque tienen que ver con la lógica y con las reglas del negocio, no con la forma en que está escrito el código.

| ID | Línea | Evidencia | Severidad | Acción | Estado |
|---|---|---|---|---|---|
| H-01 | 4 | Si el porcentaje es mayor a 100, solo se muestra un mensaje y el cálculo sigue. | Alta | Rechazar la operación con un error (S-07). | Pendiente del negocio |
| H-02 | 5 | `precio - descuento` puede dar negativo. Con (5, 100, true) el total sería -10. | Alta | Poner un tope para que el total no baje de 0 (S-08). | Pendiente del negocio |
| H-03 | 3 | Se suma un 10 sin decir si son dólares o puntos de porcentaje, y no hay una política VIP escrita. | Alta | Definir el beneficio (S-03) y guardarlo en una constante con nombre. | Pendiente del negocio |
| H-04 | 1 y 2 | No se valida nada. Un porcentaje negativo sube el precio (con 100 y -10 el total sería 110) y se aceptan un precio 0 o un texto. | Media | Validar el tipo y el rango del precio y del porcentaje (S-06, S-07). | Pendiente del negocio |
| H-05 | 5 | No hay moneda ni redondeo. Con (19.99, 15) el total sería 16.9915. | Media | Redondear a dos decimales en USD (S-04, S-05). | Pendiente del negocio |
| H-06 | 4 | Se usa `console.log` para avisar de un error. Quien llama a la función nunca se entera. | Media | Lanzar un error en lugar de escribir en la consola. | Pendiente del negocio |
| H-07 | 3 | Al cambiar `==` por `===` cambió el comportamiento. Antes, si `esVip` llegaba como 1, se aplicaba el beneficio (total 70). Ahora no se aplica (total 80). | Media | Dejar claro que `esVip` siempre es booleano (S-02) y validarlo. | Aceptado, sujeto a S-02 |
| H-08 | R-01 | El requisito de rendimiento no tiene un tiempo máximo, así que no se puede verificar. | Baja | Confirmar el umbral S-01 y medirlo con CP-01. | Pendiente del negocio |

## Qué decidí

Corregí solo E-01 a E-05. Son reglas confirmadas por ESLint y en la revisión, y no cambian la lógica del negocio, salvo el efecto del `===` que anoté en H-07.

Los hallazgos H-01 a H-08 todavía no los corregí. Todos dependen de decisiones que tiene que tomar el negocio (los supuestos S-01 a S-09 de [requisitos.md](requisitos.md)). Si los cambiara ahora estaría inventando reglas.

## Comentarios técnicos de la revisión

1. Sobre el porcentaje (H-01 y H-06): "Si el porcentaje es mayor a 100, el código solo muestra un mensaje en la consola y sigue calculando. Con (100, 150) el total sería -50. Hay que decidir si se rechaza la operación, si se limita al 100 % o si se avisa al usuario. Mientras tanto propongo lanzar un `RangeError`."
2. Sobre el beneficio VIP (H-03 y H-07): "No está claro si el +10 son dólares o puntos de porcentaje, ni si va antes o después del descuento. Además, con el cambio a `===`, un cliente que llegue con `esVip = 1` se queda sin beneficio. Antes de tocar la fórmula hace falta que el negocio defina las dos cosas."

## Cierre

Encontré 13 hallazgos: 5 de ESLint y 8 de la revisión humana. Corregí los 5 de ESLint. Los 8 restantes quedan pendientes hasta que el negocio confirme los supuestos.

Esta acta es el registro equivalente de la revisión por pares. Al hacerla una sola persona, tiene la limitación de que el revisor no es independiente del autor. Para la siguiente iteración la revisión debería hacerla otra persona.

- Pull request: ________
- Firma: Jorge Rivera
