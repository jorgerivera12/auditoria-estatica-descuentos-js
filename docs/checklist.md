# Checklist de revisión

Estas son las preguntas que usé en el rol de revisor durante la inspección. Las apliqué a los requisitos R-01 a R-04 y al código de `src/descuentos.js`, sin ejecutarlo. En la última columna anoté en qué hallazgo o propuesta terminó cada respuesta negativa.

| Nº | Pregunta | Respuesta | Evidencia | Relacionado con |
|---|---|---|---|---|
| 1 | ¿Cada requisito tiene un identificador único? | Sí | Van de R-01 a R-04. | - |
| 2 | ¿Los requisitos evitan palabras subjetivas? | No | Usan "rápidamente", "importantes", "especial" y "correcto". | RF-01 a RF-04 |
| 3 | ¿Cada requisito tiene un criterio de aceptación medible? | No | Ninguno dice qué valor o resultado se espera. | RF-01 a RF-04, CP-01 a CP-04b |
| 4 | ¿Están definidas las excepciones por promociones? | No | R-03 dice "excepto en promociones" sin más detalle. | H-02, S-09 |
| 5 | ¿Se puede relacionar cada requisito con el código? | No | Los requisitos no mencionan ninguna parte del código. | Tabla de trazabilidad |
| 6 | ¿La política VIP dice quién la recibe, cuánto y en qué unidad? | No | R-02 no lo dice y el código suma un 10 sin explicar qué es. | H-03, S-02, S-03 |
| 7 | ¿Se valida que el precio y el porcentaje tengan valores razonables? | No | El código no revisa ni el tipo ni el rango. | H-04 |
| 8 | ¿El código detiene las operaciones inválidas? | No | Muestra un mensaje con `console.log` y sigue calculando. | H-01, H-06 |
| 9 | ¿Están definidos la moneda y el redondeo del resultado? | No | `return total` devuelve el número sin moneda ni redondeo. | H-05 |
| 10 | ¿Las comparaciones usan igualdad estricta? | No | Se usa `esVip == true`. | E-02, H-07 |
| 11 | ¿Se evita declarar variables con `var`? | No | `descuento` y `total` se declaran con `var`. | E-01, E-05 |
| 12 | ¿Todos los `if` tienen llaves? | No | Los dos `if` están escritos en una sola línea sin llaves. | E-03, E-04 |
