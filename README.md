# ASIR Fighter

**ASIR Fighter** es un juego educativo en HTML, CSS y JavaScript para practicar contenidos de **1º de ASIR** relacionados con:

- XML y XSD
- HTML
- CSS
- SQL
- Puzzles de orden SQL

El objetivo del proyecto es convertir la práctica de ejercicios técnicos en una dinámica tipo juego: vidas, puntuación, modo contrarreloj, feedback inmediato, soluciones al fallar e historial local del alumno.

---

## Vista general

ASIR Fighter funciona como una aplicación web estática. No necesita servidor, base de datos ni instalación de dependencias. Basta con abrir el archivo `.html` en el navegador.

El juego permite:

- Elegir el tipo de ejercicios.
- Importar ejercicios personalizados desde un archivo JSON.
- Guardar ejercicios importados en el navegador.
- Guardar historial de partidas mediante `localStorage`.
- Ver soluciones cuando se pierde.
- Recibir feedback inmediato de errores y aciertos.
- Practicar con modos de entrenamiento, combate y simulacro de examen.

---

## Características principales

### Modos de juego

El juego incluye tres modos:

| Modo | Descripción |
|---|---|
| Entrenamiento | Sin presión de tiempo, pensado para practicar tranquilamente. |
| Combate | Modo contrarreloj con vidas. Cada error importante penaliza. |
| Examen | Simulación más seria para repasar antes de una prueba. |

### Tipos de ejercicios

Se pueden practicar distintos bloques:

| Tipo | Qué permite practicar |
|---|---|
| XML/XSD | Raíz, anidación, orden, atributos, tipos, patrones, enumeraciones y valores válidos. |
| HTML | Estructura, etiquetas semánticas, formularios, tablas, listas, accesibilidad y padres correctos. |
| CSS | Selectores, propiedades, valores, unidades, flexbox, grid y media queries. |
| SQL | Consultas, filtros, ordenación, inserciones, actualizaciones, borrados, agrupaciones y dialectos. |
| Puzzles SQL | Orden correcto de sentencias SQL mediante piezas reordenables. |

### Importación de ejercicios

Los ejercicios pueden añadirse de dos formas:

1. Pegando un array JSON en el cuadro de importación.
2. Seleccionando directamente un archivo `.json` desde el ordenador.

Los ejercicios importados se guardan en el navegador usando `localStorage`, por lo que siguen disponibles aunque se cierre la página.

### Historial del alumno

El juego guarda un historial local con:

- Fecha de la partida.
- Modo utilizado.
- Módulo practicado.
- Puntuación.
- Aciertos e intentos.
- Último ejercicio realizado.

Todo se guarda localmente en el navegador. No se envían datos a ningún servidor.

---

## Instalación y uso

### Opción 1: Abrir directamente

1. Descarga el archivo HTML del proyecto.
2. Ábrelo con un navegador moderno.
3. Configura la partida.
4. Empieza a practicar.

### Opción 2: Usar con GitHub Pages

1. Sube el archivo HTML al repositorio.
2. Renómbralo como `index.html`.
3. Activa GitHub Pages desde la configuración del repositorio.
4. Publica la página.

---

## Estructura recomendada del repositorio

```text
asir-fighter/
├── index.html
├── README.md
├── exercises/
│   ├── xml.json
│   ├── html.json
│   ├── css.json
│   ├── sql.json
│   └── sql-puzzles.json
└── docs/
    └── json-schema-examples.md
```

El proyecto puede funcionar solo con `index.html`, pero separar los ejercicios en la carpeta `exercises/` facilita mantenerlos y ampliarlos.

---

## Formato JSON de ejercicios

El importador espera un **array JSON**. Cada elemento del array representa un ejercicio.

Ejemplo general:

```json
[
  {
    "m": "xml",
    "title": "XML 001 · usuario básico",
    "enemy": "Error XML/XSD",
    "text": "Corrige el XML del usuario.",
    "root": "usuario",
    "schema": {
      "children": [
        {
          "name": "nombre",
          "type": "string"
        },
        {
          "name": "edad",
          "type": "integer"
        }
      ]
    },
    "starter": "<usuario>\n  <nombre>Ana</nombre>\n  <edad>veinte</edad>\n</usuario>",
    "ref": "usuario → nombre, edad",
    "hint": "edad debe ser un número entero.",
    "theory": "Un XML válido debe respetar raíz, orden, anidación y tipos."
  }
]
```

---

## Ejercicios XML/XSD

Los ejercicios XML usan:

- `m: "xml"`
- `root`
- `schema.children`
- opcionalmente `schema.attrs`

Tipos soportados:

| Tipo | Descripción |
|---|---|
| string | Texto no vacío. |
| integer | Número entero. |
| decimal | Número decimal con punto. |
| date | Fecha en formato `AAAA-MM-DD`. |
| boolean | `true`, `false`, `1` o `0`. |
| enum | Valor dentro de una lista permitida. |
| pattern | Valor que cumple una expresión regular. |
| complex | Elemento con hijos internos. |

Ejemplo:

```json
{
  "m": "xml",
  "title": "XML · equipo informático",
  "enemy": "Error XML/XSD",
  "text": "Corrige el XML de equipo informático.",
  "root": "equipo",
  "schema": {
    "attrs": [
      {
        "name": "id",
        "type": "pattern",
        "pattern": "^EQ-[0-9]{3}$",
        "example": "EQ-001"
      }
    ],
    "children": [
      {
        "name": "tipo",
        "type": "string"
      },
      {
        "name": "marca",
        "type": "string"
      },
      {
        "name": "ramGB",
        "type": "integer"
      }
    ]
  },
  "starter": "<equipo>\n  <tipo>Portátil</tipo>\n  <marca>Lenovo</marca>\n  <ramGB>dieciseis</ramGB>\n</equipo>",
  "ref": "<equipo id=\"EQ-001\">...",
  "hint": "Falta el atributo id y ramGB debe ser entero.",
  "theory": "Los atributos obligatorios van en la etiqueta de apertura."
}
```

---

## Ejercicios HTML

Los ejercicios HTML usan:

- `m: "html"`
- `rules`

Cada regla puede usar:

| Campo | Uso |
|---|---|
| sel | Selector CSS que debe existir. |
| min | Número mínimo de elementos requeridos. |
| parent | Padre directo esperado. |
| ancestor | Ancestro esperado, aunque no sea padre directo. |

Ejemplo:

```json
{
  "m": "html",
  "title": "HTML · formulario login",
  "enemy": "Error HTML",
  "text": "Crea un formulario de login accesible.",
  "rules": [
    {
      "sel": "form"
    },
    {
      "sel": "label[for]",
      "min": 2,
      "parent": "form"
    },
    {
      "sel": "input[name]",
      "min": 2,
      "parent": "form"
    },
    {
      "sel": "input[type=\"password\"]",
      "parent": "form"
    },
    {
      "sel": "button[type=\"submit\"]",
      "parent": "form"
    }
  ],
  "starter": "<form>\nUsuario <input>\nClave <input>\n<button>Entrar</button>\n</form>",
  "ref": "label for + input id/name + password + submit",
  "hint": "Añade labels, names y type password.",
  "theory": "Los formularios accesibles deben relacionar label e input."
}
```

---

## Ejercicios CSS

Los ejercicios CSS usan:

- `m: "css"`
- `rules`
- `rules[].sel`
- `rules[].props`

También se admiten reglas con `media`.

Ejemplo:

```json
{
  "m": "css",
  "title": "CSS · Flex center",
  "enemy": "Error CSS",
  "text": "Crea un contenedor flex centrado.",
  "rules": [
    {
      "sel": ".contenedor",
      "props": {
        "display": "flex",
        "justify-content": "center",
        "align-items": "center"
      }
    }
  ],
  "starter": ".contenedor { display:block; }",
  "ref": ".contenedor { display:flex; justify-content:center; align-items:center; }",
  "hint": "display flex es imprescindible.",
  "theory": "Flexbox permite alinear elementos en eje principal y transversal."
}
```

Ejemplo con media query:

```json
{
  "m": "css",
  "title": "CSS · Responsive",
  "enemy": "Error CSS",
  "text": "En móvil .layout pasa a una columna.",
  "rules": [
    {
      "sel": ".layout",
      "props": {
        "display": "grid",
        "grid-template-columns": "1fr 1fr"
      }
    },
    {
      "media": "max-width: 700px",
      "sel": ".layout",
      "props": {
        "grid-template-columns": "1fr"
      }
    }
  ],
  "starter": ".layout {\n display:grid;\n grid-template-columns:1fr 1fr;\n}",
  "ref": "@media (max-width: 700px) { .layout { grid-template-columns:1fr; } }",
  "hint": "Falta media query.",
  "theory": "Las media queries permiten adaptar el diseño al tamaño de pantalla."
}
```

---

## Ejercicios SQL

Los ejercicios SQL usan:

- `m: "sql"`
- `dialect`
- `checks`

Dialectos recomendados:

- `mysql`
- `phpmyadmin`
- `oracle`

Tipos de comprobación:

| Tipo | Qué comprueba |
|---|---|
| starts | Que la consulta empiece por un texto. |
| contains | Que contenga un fragmento. |
| notcontains | Que no contenga un fragmento. |
| from | Que contenga `FROM tabla`. |

Ejemplo:

```json
{
  "m": "sql",
  "dialect": "mysql",
  "title": "SQL · SELECT columnas",
  "enemy": "Error SQL",
  "text": "Obtén nombre y email de usuarios.",
  "starter": "SELECT * FROM usuarios;",
  "solution": "SELECT nombre, email FROM usuarios;",
  "checks": [
    {
      "type": "starts",
      "value": "select"
    },
    {
      "type": "contains",
      "value": "nombre"
    },
    {
      "type": "contains",
      "value": "email"
    },
    {
      "type": "from",
      "value": "usuarios"
    },
    {
      "type": "notcontains",
      "value": "*"
    }
  ],
  "ref": "Selecciona solo nombre y email.",
  "hint": "Evita SELECT *.",
  "theory": "SELECT indica las columnas y FROM indica la tabla."
}
```

---

## Puzzles SQL

Los puzzles SQL usan:

- `m: "sqlpuzzle"`
- `pieces`
- `solution`

El jugador debe ordenar las piezas.

Ejemplo:

```json
{
  "m": "sqlpuzzle",
  "dialect": "mysql",
  "title": "SQL Puzzle · SELECT WHERE ORDER",
  "enemy": "Puzzle SQL",
  "text": "Ordena una consulta con filtro y orden.",
  "pieces": [
    "ORDER BY nota DESC",
    "SELECT nombre, nota",
    "WHERE curso = '1ASIR'",
    "FROM alumnos"
  ],
  "solution": [
    "SELECT nombre, nota",
    "FROM alumnos",
    "WHERE curso = '1ASIR'",
    "ORDER BY nota DESC"
  ],
  "ref": "Identifica columnas, tabla, filtro y orden final.",
  "hint": "SELECT, FROM, WHERE, ORDER BY.",
  "theory": "Piensa la función de cada pieza antes de ordenarlas."
}
```

---

## Prompt recomendado para generar ejercicios con IA

Puedes usar este prompt para generar nuevos ejercicios compatibles:

```text
Genera un array JSON válido para ASIR Fighter.

Quiero ejercicios para 1º de ASIR en España.

Incluye ejercicios de:
- XML/XSD
- HTML
- CSS
- SQL
- Puzzles SQL

Respeta exactamente estas estructuras:

XML:
{
  "m": "xml",
  "title": "...",
  "enemy": "Error XML/XSD",
  "text": "...",
  "root": "...",
  "schema": {
    "attrs": [],
    "children": []
  },
  "starter": "...",
  "ref": "...",
  "hint": "...",
  "theory": "..."
}

HTML:
{
  "m": "html",
  "title": "...",
  "enemy": "Error HTML",
  "text": "...",
  "rules": [
    {
      "sel": "...",
      "parent": "...",
      "ancestor": "...",
      "min": 1
    }
  ],
  "starter": "...",
  "ref": "...",
  "hint": "...",
  "theory": "..."
}

CSS:
{
  "m": "css",
  "title": "...",
  "enemy": "Error CSS",
  "text": "...",
  "rules": [
    {
      "sel": "...",
      "props": {
        "propiedad": "valor"
      }
    }
  ],
  "starter": "...",
  "ref": "...",
  "hint": "...",
  "theory": "..."
}

SQL:
{
  "m": "sql",
  "dialect": "mysql | phpmyadmin | oracle",
  "title": "...",
  "enemy": "Error SQL",
  "text": "...",
  "starter": "...",
  "solution": "...",
  "checks": [
    {
      "type": "contains | notcontains | starts | from",
      "value": "..."
    }
  ],
  "ref": "...",
  "hint": "...",
  "theory": "..."
}

Puzzle SQL:
{
  "m": "sqlpuzzle",
  "dialect": "mysql | phpmyadmin | oracle",
  "title": "...",
  "enemy": "Puzzle SQL",
  "text": "...",
  "pieces": [],
  "solution": [],
  "ref": "...",
  "hint": "...",
  "theory": "..."
}

Devuelve únicamente JSON válido, sin markdown, sin comentarios y sin texto adicional.
```

---

## Limitaciones actuales

El proyecto está pensado como herramienta educativa, no como validador profesional completo.

Algunas limitaciones:

- La validación XSD es educativa y simplificada.
- La validación SQL se basa en comprobaciones de texto, no en ejecución real contra una base de datos.
- La validación HTML usa reglas de selectores, no un validador W3C completo.
- El historial se guarda solo en el navegador actual.
- Si se borra el almacenamiento local del navegador, se pierden ejercicios importados e historial.

---

## Privacidad

ASIR Fighter no envía datos a servidores externos.

Todo se guarda localmente en el navegador mediante `localStorage`:

- ejercicios importados;
- historial;
- mejor puntuación.

---

## Ideas de mejora

Posibles mejoras futuras:

- Exportar historial del alumno a JSON o CSV.
- Añadir perfiles de usuario.
- Añadir niveles por dificultad.
- Añadir más validaciones SQL.
- Añadir modo profesor con packs de ejercicios.
- Añadir estadísticas por módulo.
- Añadir importación múltiple de archivos.
- Añadir editor con resaltado de sintaxis.

---

## Tecnologías usadas

- HTML
- CSS
- JavaScript
- `localStorage`

No usa frameworks ni dependencias externas.

---

## Licencia

Puedes usar este proyecto como recurso educativo, adaptarlo y ampliarlo para clase o estudio personal.

Si lo publicas, se recomienda añadir una licencia explícita, por ejemplo:

- MIT
- GPL-3.0
- Creative Commons BY-SA para contenido educativo

---

## Autoría

Proyecto educativo creado para practicar contenidos de **Lenguajes de Marcas** y **Bases de Datos** en **1º de ASIR**.

