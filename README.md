# EDO Solver — Solucionador de Ecuaciones Diferenciales Ordinarias

[![Live Demo](https://img.shields.io/badge/demo-live-brightgreen)](https://edo.liuofc.me)
[![Powered by SymPy](https://img.shields.io/badge/motor-SymPy%20%2B%20Pyodide-blue)](https://www.sympy.org)
[![MathJax](https://img.shields.io/badge/render-MathJax%203-orange)](https://www.mathjax.org)
[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey)](#licencia)

> Resuelve EDO de primer orden directamente en el navegador — sin servidor, sin instalaciones.

---

## 🌐 Demo en vivo

**[https://edo.liuofc.me](https://edo.liuofc.me)**

---

## ✨ Características

| Característica | Detalle |
|---|---|
| **100 % en el navegador** | Usa [Pyodide](https://pyodide.org) para ejecutar Python + SymPy en WebAssembly |
| **Múltiples métodos** | Lineal de primer orden, variables separables, exactas, homogéneas, reducibles a homogéneas y ecuaciones no despejadas |
| **Factor integrante** | Detección automática de μ(x) y μ(y) cuando la ecuación no es exacta de entrada |
| **SymPy `dsolve`** | Respaldo con el solver completo de SymPy cuando los métodos analíticos no son suficientes |
| **Renderizado LaTeX** | Resultados formateados con MathJax 3 |
| **Historial local** | Guarda las últimas ecuaciones resueltas en `localStorage` |
| **Exportar** | Copia la solución, descárgala en Markdown o en PDF |
| **Código Python** | Muestra el código Python/SymPy equivalente para replicar la solución |
| **Modo auto-resolver** | Resuelve automáticamente mientras escribes |
| **Diseño responsive** | Funciona en escritorio y dispositivos móviles |

---

## 🧮 Métodos de resolución soportados

1. **Lineal de primer orden** — `y' + P(x)·y = Q(x)`
2. **Variables separables** — `g(y) dy = f(x) dx`
3. **Ecuación exacta** — `M dx + N dy = 0` con `∂M/∂y = ∂N/∂x`
4. **Factor integrante** — cuando la ecuación no es exacta (μ en función de x o de y)
5. **Homogénea** — `y' = f(y/x)` mediante la sustitución `v = y/x`
6. **Reducible a homogénea** — traslación de ejes `(ax + by + c)`
7. **Ecuación no despejada** — resolución de `F(x, y, y') = 0` despejando `y'`
8. **SymPy `dsolve`** — solver simbólico completo como respaldo

---

## 📝 Sintaxis de entrada

Escribe la ecuación en notación matemática estándar:

```
y' = f(x, y)          # forma explícita
dy/dx = f(x, y)       # notación de Leibniz
M(x,y) dx + N(x,y) dy = 0   # forma diferencial
F(x, y, y') = 0       # forma implícita
```

### Ejemplos incluidos

| Ecuación | Tipo |
|---|---|
| `xy' - y = y^3` | No despejada |
| `sqrt(1+x^3) dy/dx = x^2 y + x^2` | Lineal / separable |
| `tg x.sen^2 y.dx + cos^2 x.ctg y.dy = 0` | Forma diferencial |
| `(1-y)e^x y' + y^2/(x ln x) = 0` | Con logaritmo |
| `dy/dx = (a*x+b)/(c*x+d)` | Con parámetros |
| `dy/dx = (2*y-x+5)/(2*x-y-4)` | Reducible a homogénea |

---

## 🚀 Uso local

No se necesita ningún servidor de backend. Basta con abrir el archivo en el navegador:

```bash
# Clona el repositorio
git clone https://github.com/liud15/calculiu.git
cd calculiu

# Abre directamente en el navegador
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

> La primera carga descarga Pyodide (~10 MB) y SymPy desde jsDelivr CDN. A partir de la segunda visita, el navegador usa la caché.

---

## 🛠️ Stack tecnológico

| Tecnología | Rol |
|---|---|
| [Pyodide v0.26.4](https://pyodide.org) | Intérprete Python en WebAssembly |
| [SymPy](https://www.sympy.org) | Computación simbólica (resolución de EDOs) |
| [MathJax 3](https://www.mathjax.org) | Renderizado de expresiones LaTeX |
| HTML / CSS / JavaScript | Interfaz de usuario (sin frameworks externos) |

---

## 📁 Estructura del proyecto

```
calculiu/
├── index.html   # Aplicación completa (UI + Python embebido + lógica JS)
├── CNAME        # Dominio personalizado para GitHub Pages
└── README.md    # Este archivo
```

---

## 🤝 Contribuir

1. Haz un fork del repositorio
2. Crea una rama: `git checkout -b feature/mi-mejora`
3. Realiza tus cambios en `index.html`
4. Abre un Pull Request describiendo la mejora

---

## 📄 Licencia

Este proyecto está bajo la licencia **MIT**. Consulta el archivo [LICENSE](LICENSE) para más detalles.

---

<div align="center">Hecho con ❤️ por <a href="https://github.com/liud15">Liu</a></div>