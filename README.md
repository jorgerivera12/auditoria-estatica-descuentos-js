# Auditoría estática del módulo de descuentos

En este repositorio está la auditoría estática del módulo que calcula los descuentos de una tienda en línea. Revisé los requisitos, hice una inspección del código siguiendo los roles de una revisión por pares y lo analicé con ESLint. No ejecuté pruebas funcionales.

Repositorio: https://github.com/jorgerivera12/auditoria-estatica-descuentos-js

## Contenido

- `src/descuentos.js`: el módulo de descuentos.
- `docs/requisitos.md`: el análisis de los requisitos R-01 a R-04, los requisitos verificables RF-01 a RF-04, los supuestos que debe confirmar el negocio y la trazabilidad.
- `docs/checklist.md`: las 12 preguntas que usé en la revisión.
- `docs/inspeccion.md`: el acta de la inspección, con los roles, los hallazgos y lo que decidí.
- `evidencias/`: la salida de ESLint antes (`lint-inicial.txt`) y después (`lint-final.txt`) de las correcciones.
- `eslint.config.mjs` y `package.json`: la configuración de ESLint y el script `lint`.

## Cómo reproducirlo

Se necesita Node.js 18.18 o superior. Abre la carpeta en Visual Studio Code y ejecuta en la terminal:

```bash
npm install
npm run lint
```

## Reglas de ESLint

- `no-var`: una variable declarada con `var` existe en toda la función, incluso antes de la línea donde se declara. Con `let` y `const` la variable solo existe dentro de su bloque, y `const` además evita que se reasigne por error.
- `eqeqeq`: `==` convierte los tipos antes de comparar, así que `1 == true` da `true`. Con `===` se comparan el valor y el tipo, y no hay sorpresas.
- `no-unused-vars`: avisa cuando una variable se declara y nunca se usa, lo que suele indicar código sobrante o un cálculo que se olvidó.
- `curly`: obliga a poner llaves en los `if`. Sin llaves, si alguien agrega una segunda línea, esa línea queda fuera de la condición.

## Qué se corrigió

Solo corregí lo que marcó ESLint y confirmé en la revisión: los `var`, el `==` y los `if` sin llaves. El +10 del cliente VIP, el caso del porcentaje mayor a 100, el tope del descuento y el redondeo los dejé igual, porque dependen de decisiones del negocio que todavía no están confirmadas.
