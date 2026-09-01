# Seis piezas de SQL

Una página que explica, en simple, qué hace cada pieza de SQL que hizo falta para
convertir un archivo público de 41,188 filas en un almacén de datos completo.

**→ [klismann21.github.io/seis-piezas-de-sql](https://klismann21.github.io/seis-piezas-de-sql/)**

No hay nada que instalar ni que ejecutar. Está pensada para quien sabe hacer un
`SELECT` y todavía no construyó nada propio.

---

## De qué trata

Un banco vendió depósitos a plazo llamando por teléfono entre mayo de 2008 y
noviembre de 2010. En esos tres años llamó a más del 90 % menos de gente, y la
proporción que aceptó pasó de 5 a 52 de cada 100.

| Año | Llamadas | Aceptaron | De cada 100 |
|-----|---------:|----------:|------------:|
| 2008 | 27,690 | 1,339 | 5 |
| 2009 | 11,440 | 2,228 | 19 |
| 2010 | 2,058 | 1,073 | 52 |

La página recorre las seis piezas que hicieron falta para poder preguntarse por qué:

| | Pieza | Qué resuelve |
|--|-------|--------------|
| 01 | `CREATE TABLE` | Meter el archivo crudo, todo como texto, sin corregir nada todavía |
| 02 | `GROUP BY` + `COUNT()` | Convertir 41,188 filas sueltas en un número que alguien puede leer |
| 03 | `LAG()` | Reconstruir el año, que el archivo no traía |
| 04 | `JOIN` | La unión normal — y el error caro: unir por fecha, no solo por id |
| 05 | `CREATE PROCEDURE` | Dejar la carga funcionando en vez de una consulta suelta |
| 06 | `--` y el nombre de la tabla | Marcar a la vista lo que es inventado |

---

## Qué hay en este repositorio

```
docs/index.html   la página entera: HTML y CSS en un solo archivo
docs/og.png       imagen de vista previa para redes
```

Sin frameworks, sin dependencias, sin compilación y sin JavaScript. Se publica con
GitHub Pages desde la carpeta `docs/`.

El script de SQL Server del proyecto no forma parte de este repositorio. Aquí vive
únicamente la página que lo explica.

---

## Los datos

**Bank Marketing Data Set** — UCI Machine Learning Repository, 41,188 registros.
Público y gratuito: <https://archive.ics.uci.edu/dataset/222/bank-marketing>

Qué es qué, porque conviene decirlo antes de que alguien lo descubra solo:

- **Reales.** Las 41,188 llamadas, con edad, trabajo, estado civil, educación,
  hipoteca, mora, préstamo, canal, mes, duración, campaña, historial previo y
  resultado. Es el archivo tal cual.
- **Deducidos.** El año de cada llamada y la identidad del cliente. Salen de los
  datos, pero siguiendo reglas que hubo que elegir; otras reglas darían otros
  números.
- **Inventados.** La clasificación de clientes y sus cambios en el tiempo. El
  archivo no los trae. Se generaron de forma determinista y viven aislados en una
  tabla con el prefijo `simulado_`.

Como esa clasificación se repartió al azar, la diferencia entre medir con la
clasificación histórica y con la actual salió chica: 0.40 puntos porcentuales en el
caso mayor. La estructura que permite medirlo funciona y está validada; el número es
pequeño porque el dato de entrada es inventado.

---

Klismann Saavedra
