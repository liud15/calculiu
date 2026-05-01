# Documentación — EDO Solver

## Índice

1. [Descripción general](#descripción-general)
2. [Arquitectura](#arquitectura)
3. [Métodos de resolución](#métodos-de-resolución)
4. [Sintaxis de entrada](#sintaxis-de-entrada)
5. [Funcionalidades adicionales](#funcionalidades-adicionales)
6. [Uso local](#uso-local)
7. [Despliegue en GitHub Pages](#despliegue-en-github-pages)
8. [Dependencias externas](#dependencias-externas)
9. [Estructura del código](#estructura-del-código)
10. [Limitaciones conocidas](#limitaciones-conocidas)

---

## Descripción general

**EDO Solver** es una aplicación web que resuelve ecuaciones diferenciales ordinarias (EDO) de primer orden directamente en el navegador, sin necesidad de servidor backend ni instalaciones adicionales.

Funciona gracias a [Pyodide](https://pyodide.org), que ejecuta Python (y por ende [SymPy](https://www.sympy.org)) en WebAssembly dentro del propio navegador.

**Demo en vivo:** [https://edo.liuofc.me](https://edo.liuofc.me)

---

## Arquitectura

```
Navegador
├── index.html          ← Toda la aplicación (UI + lógica JS + código Python embebido)
│   ├── HTML/CSS        ← Interfaz de usuario responsive
│   ├── JavaScript      ← Carga de Pyodide, manejo de eventos, renderizado con MathJax
│   └── Python (inline) ← Lógica de resolución de EDOs usando SymPy
└── Pyodide (CDN)       ← Intérprete Python/WebAssembly (~10 MB, cacheado por el navegador)
```

La aplicación no requiere ningún servidor: toda la computación ocurre en el cliente.

---

## Métodos de resolución

El solver intenta los métodos en el siguiente orden de prioridad:

### 1. Lineal de primer orden
Forma: `y' + P(x)·y = Q(x)`

Se calcula el factor integrante `μ(x) = e^(∫P(x)dx)` y se integra directamente.

### 2. Variables separables
Forma: `g(y) dy = f(x) dx`

Se separan las variables y se integra cada lado de forma independiente.

### 3. Ecuación exacta
Forma: `M(x,y) dx + N(x,y) dy = 0`

Se verifica la condición `∂M/∂y = ∂N/∂x`. Si se cumple, se resuelve integrando `M` respecto a `x` y ajustando con `N`.

### 4. Factor integrante
Cuando la ecuación no es exacta, se busca un factor integrante `μ`:
- `μ(x)`: si `(∂M/∂y − ∂N/∂x) / N` depende solo de `x`.
- `μ(y)`: si `(∂N/∂x − ∂M/∂y) / M` depende solo de `y`.

### 5. Homogénea
Forma: `y' = f(y/x)`

Se aplica la sustitución `v = y/x` para reducirla a una ecuación separable.

### 6. Reducible a homogénea
Forma con términos lineales `ax + by + c` y `dx + ey + f`.

Se realiza una traslación de ejes para eliminar los términos independientes y se reduce al caso homogéneo.

### 7. Ecuación no despejada
Forma implícita: `F(x, y, y') = 0`

Se intenta despejar `y'` algebraicamente usando SymPy y se resuelve cada rama obtenida.

### 8. SymPy `dsolve` (respaldo)
Si ninguno de los métodos anteriores tiene éxito, se utiliza el solver simbólico completo de SymPy (`dsolve`) como respaldo general.

---

## Sintaxis de entrada

La ecuación se puede escribir en distintas notaciones:

| Notación | Ejemplo |
|---|---|
| Explícita con `y'` | `y' = 2*x*y` |
| Notación de Leibniz | `dy/dx = 2*x*y` |
| Forma diferencial | `2*x*y dx - dy = 0` |
| Forma implícita | `F(x, y, y') = 0` |

### Operadores y funciones soportados

| Símbolo | Significado |
|---|---|
| `*` | Multiplicación |
| `^` o `**` | Potencia |
| `sqrt(x)` | Raíz cuadrada |
| `sin(x)`, `cos(x)`, `tg(x)`, `tan(x)` | Trigonométricas |
| `ln(x)`, `log(x)` | Logaritmo natural |
| `exp(x)`, `e^x` | Exponencial |
| `abs(x)` | Valor absoluto |

### Constantes de integración

La constante de integración aparece en la solución como `C1` o `C`.

---

## Funcionalidades adicionales

### Historial local
Las últimas ecuaciones resueltas se guardan en `localStorage` del navegador. Puedes hacer clic en cualquier entrada del historial para volver a cargarla.

### Exportar solución
- **Copiar**: copia la solución en texto plano al portapapeles.
- **Markdown**: descarga la solución en formato `.md`.
- **PDF**: genera e imprime la solución como PDF usando el diálogo de impresión del navegador.

### Código Python equivalente
Muestra el bloque de código Python/SymPy que reproduce la solución, útil para aprendizaje o integración en scripts propios.

### Modo auto-resolver
Cuando está activado, el solver se ejecuta automáticamente mientras escribes la ecuación (con un pequeño retardo para evitar llamadas excesivas).

### Ejemplos predefinidos
El botón **Ejemplos** muestra una selección de ecuaciones de distintos tipos para probar rápidamente la herramienta.

---

## Uso local

No se necesita ningún servidor. Basta con clonar el repositorio y abrir el archivo HTML:

```bash
git clone https://github.com/liud15/calculiu.git
cd calculiu
```

Luego abre `index.html` directamente en el navegador:

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

> **Nota:** La primera carga descarga Pyodide (~10 MB) y SymPy desde la CDN de jsDelivr. Las visitas posteriores usan la caché del navegador y cargan instantáneamente.

---

## Despliegue en GitHub Pages

El proyecto está configurado para servirse desde GitHub Pages con el dominio personalizado `edo.liuofc.me` (definido en el archivo `CNAME`).

Para desplegar tu propia copia:

1. Haz un fork del repositorio.
2. Ve a **Settings → Pages** y activa GitHub Pages desde la rama `main`.
3. (Opcional) Edita el archivo `CNAME` con tu propio dominio.

---

## Dependencias externas

Todas las dependencias se cargan desde CDN; no hay `package.json` ni pasos de compilación.

| Biblioteca | Versión | Fuente |
|---|---|---|
| [Pyodide](https://pyodide.org) | 0.26.4 | jsDelivr CDN |
| [SymPy](https://www.sympy.org) | (incluida en Pyodide) | — |
| [MathJax](https://www.mathjax.org) | 3.x | cdn.jsdelivr.net |

---

## Estructura del código

Todo el proyecto reside en un único archivo `index.html`, organizado en tres bloques:

| Sección | Descripción |
|---|---|
| `<head>` | Meta-etiquetas, estilos CSS, carga de MathJax |
| `<body>` | Estructura HTML de la interfaz (formulario, paneles de resultado, historial) |
| `<script>` | Lógica JavaScript: carga de Pyodide, código Python embebido (como string), manejo de eventos, renderizado |

El código Python embebido en el `<script>` contiene todas las funciones de análisis y resolución de EDOs, y se ejecuta en el entorno Pyodide al cargar la página.

---

## Limitaciones conocidas

- Solo resuelve **EDOs de primer orden**. Las ecuaciones de orden superior no están soportadas de forma directa (aunque SymPy `dsolve` puede manejar algunos casos).
- La primera carga puede tardar varios segundos mientras se descarga e inicializa Pyodide.
- Algunas ecuaciones muy complejas pueden agotar el tiempo de respuesta del solver o devolver una solución en forma implícita.
- La detección del tipo de ecuación es heurística; en casos ambiguos puede seleccionar un método subóptimo.

---

<div align="center">Documentación de <a href="https://github.com/liud15/calculiu">EDO Solver</a> — Licencia MIT</div>
