# Requisitos del módulo de descuentos

Revisé los cuatro requisitos del enunciado. El problema que tienen en común es que ninguno se puede comprobar tal como está escrito: usan palabras como "rápidamente", "importantes" o "correcto", que cada persona puede entender de forma distinta.

Para cada uno propuse un requisito funcional (RF) que sí se pueda verificar. Cuando tuve que poner un valor concreto, lo dejé como supuesto, porque esa decisión le corresponde al negocio y todavía no está confirmada.

Requisitos originales:

- R-01: «El descuento debe calcularse rápidamente».
- R-02: «Los clientes importantes obtendrán un descuento especial».
- R-03: «El descuento no puede superar el precio, excepto en promociones».
- R-04: «Se debe mostrar el resultado correcto».

## Matriz de requisitos

| ID | Problema | Impacto | Propuesta | Artefacto | Estado |
|---|---|---|---|---|---|
| R-01 | No dice qué significa "rápidamente". No hay un tiempo máximo ni una forma de medirlo. | No hay manera de decir si el módulo cumple o no. | RF-01: `calcularDescuento` responde en 1 ms o menos en el 95 % de 10 000 llamadas con datos válidos (S-01). | `src/descuentos.js` | Pendiente de confirmar |
| R-02 | No define quién es un cliente importante, ni de cuánto es el descuento especial, ni si es en dinero o en porcentaje. | Dos desarrolladores podrían programar beneficios distintos. | RF-02: si `esVip` es `true`, se restan USD 10,00 después de aplicar el porcentaje. Con cualquier otro valor no hay beneficio VIP (S-02, S-03). | Condición `esVip` | Pendiente de confirmar |
| R-03 | La excepción de "promociones" queda abierta: no dice cuáles son ni hasta dónde puede llegar el descuento. | Si el descuento supera el precio, el total queda negativo y la tienda pierde dinero. | RF-03: con datos válidos, el descuento total nunca supera el precio, así que el total nunca baja de 0 (S-08, S-09). | Cálculo de `descuento` y `total` | Pendiente de confirmar |
| R-04 | "Correcto" no aclara nada: falta la moneda, el redondeo y qué hacer con datos inválidos. | Sin un resultado esperado no hay contra qué comparar. | RF-04: el total se entrega en USD con dos decimales. Si los datos no son válidos, la función lanza un error en lugar de devolver un total (S-04 a S-07). | `return total` | Pendiente de confirmar |

## Supuestos que debe confirmar el negocio

Estos valores los propuse yo para poder escribir requisitos verificables. Todos están pendientes. Si el negocio decide otra cosa, hay que ajustar el RF correspondiente.

| ID | Supuesto | Afecta a |
|---|---|---|
| S-01 | La función debe responder en 1 ms o menos en el 95 % de 10 000 llamadas. | RF-01 |
| S-02 | `esVip` siempre llega como booleano (`true` o `false`). | RF-02 |
| S-03 | El beneficio VIP son USD 10,00 fijos y se aplican después del porcentaje. | RF-02 |
| S-04 | La moneda es USD. | RF-04 |
| S-05 | El total se redondea a dos decimales. | RF-04 |
| S-06 | El precio debe ser mayor que 0 y el porcentaje debe estar entre 0 y 100. | RF-03, RF-04 |
| S-07 | Si un dato no es válido, la función lanza un error (`TypeError` o `RangeError`) en vez de solo mostrar un mensaje. | RF-03, RF-04 |
| S-08 | El descuento total nunca supera el precio, así que el total mínimo es 0,00. | RF-03 |
| S-09 | La excepción por promociones no se aplica hasta que el negocio la defina. | RF-03 |

## Criterios de aceptación

Para cada RF dejé algunos casos con el resultado esperado. No los ejecuté, porque esta revisión es estática. Quedan listos para la etapa de pruebas.

| Caso | RF | Datos (precio, porcentaje, esVip) | Resultado esperado |
|---|---|---|---|
| CP-01 | RF-01 | 10 000 llamadas con datos válidos | 1 ms o menos en el 95 % |
| CP-02a | RF-02 | (100, 20, false) | 80,00 |
| CP-02b | RF-02 | (100, 20, true) | 70,00 |
| CP-02c | RF-02 | (100, 20, 1) | 80,00 |
| CP-03a | RF-03 | (5, 100, true) | 0,00 |
| CP-03b | RF-03 | (100, 150, false) | Error |
| CP-03c | RF-03 | (100, -10, false) | Error |
| CP-04a | RF-04 | (19.99, 15, false) | 16,99 |
| CP-04b | RF-04 | ("abc", 10, false) | Error |

## Trazabilidad

Esta tabla relaciona cada requisito con la parte del código donde se implementa, con los hallazgos de la inspección (están en [inspeccion.md](inspeccion.md)) y con los casos que servirían para verificarlo.

| RF | Viene de | Supuestos | Parte del código | Hallazgos | Casos | Estado |
|---|---|---|---|---|---|---|
| RF-01 | R-01 | S-01 | Toda la función `calcularDescuento` | H-08 | CP-01 | Pendiente de confirmar |
| RF-02 | R-02 | S-02, S-03 | Condición `esVip` | E-02, H-03, H-07 | CP-02a a CP-02c | Pendiente de confirmar |
| RF-03 | R-03 | S-06 a S-09 | Condición `porcentaje > 100` y resta `precio - descuento` | H-01, H-02, H-04, H-06 | CP-03a a CP-03c | Pendiente de confirmar |
| RF-04 | R-04 | S-04 a S-07 | `return total` | H-04, H-05 | CP-04a y CP-04b | Pendiente de confirmar |
