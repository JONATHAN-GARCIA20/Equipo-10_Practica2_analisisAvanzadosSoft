# 💹 Simulador de Bolsa de Valores — Web

## 📘 Descripción del Proyecto
El **Simulador de Bolsa de Valores Web** permite al usuario comprar acciones virtuales de empresas reconocidas y observar el valor de su portafolio en tiempo real mediante una gráfica interactiva.  
Está desarrollado con **HTML**, **CSS** y **JavaScript**, utilizando la librería **Chart.js** para la visualización dinámica de datos.

---

## 🧩 Funcionalidades Principales

- **Compra de acciones virtuales** mediante el ingreso del ticker y la cantidad.
- **Conversión de divisas** entre **MXN**, **USD** y **EUR**.
- **Visualización interactiva** de la evolución del portafolio con **Chart.js**.
- **Tabla dinámica** que muestra el valor actualizado de cada acción según la divisa seleccionada.
- **Diseño adaptable y moderno** con estilo limpio y componentes visuales minimalistas.

---

## 🧮 Tecnologías Utilizadas

| Tecnología | Descripción |
|-------------|-------------|
| **HTML5** | Estructura base de la interfaz web. |
| **CSS3** | Estilos visuales responsivos y modernos. |
| **JavaScript (ES6)** | Lógica principal de compra, actualización y conversión de valores. |
| **Chart.js** | Librería para generar gráficos de líneas dinámicos. |

---

## 📊 Estructura del Sistema

### 1. Panel de Control
Permite ingresar el **ticker**, la **cantidad** de acciones a comprar y seleccionar la **divisa** base.

### 2. Tabla de Acciones
Muestra una lista de acciones disponibles con:
- Nombre de la empresa  
- Precio actualizado en la divisa seleccionada  
- Etiqueta visual de moneda  

### 3. Gráfica del Portafolio
La gráfica se actualiza automáticamente con cada compra, mostrando la evolución del valor total del portafolio.

---

## ⚙️ Variables Principales

- `acciones`: contiene la información de las empresas (nombre, precio y moneda).
- `tipoCambio`: define los valores de conversión entre **USD**, **EUR** y **MXN**.
- `portafolio`: almacena las posiciones compradas y el historial de evolución.

---

## 🧠 Funciones Clave

| Función | Descripción |
|----------|-------------|
| `renderTabla(divisaBase)` | Actualiza la tabla de precios según la divisa seleccionada. |
| `actualizarTablaDivisa()` | Refresca la tabla y el valor del portafolio al cambiar de divisa. |
| `comprarAccion()` | Registra la compra de acciones según el ticker y cantidad ingresados. |
| `actualizarValor(divisa)` | Calcula el valor total del portafolio y actualiza la gráfica. |

---

## 🧾 Ejemplo de Acciones Incluidas

| Ticker | Empresa | Moneda |
|--------|----------|--------|
| AAPL | Apple Inc. | USD |
| TSLA | Tesla Motors | USD |
| BMW | Bayerische Motoren Werke AG | EUR |
| AMXL | América Móvil | MXN |
| CEMEX | Cemex S.A.B. de C.V. | MXN |
| GOOG | Alphabet Inc. | USD |
| MSFT | Microsoft Corp. | USD |
| BIMBO | Grupo Bimbo S.A.B. de C.V. | MXN |
| WALMEX | Walmart de México | MXN |
| AMZN | Amazon.com Inc. | USD |

---

## 🖥️ Vista del Proyecto


```plaintext
📊 Interfaz General
┌────────────────────────────┬─────────────────────────────┐
│ Tabla de Acciones          │ Gráfica del Portafolio      │
│ Valor Total (MXN/USD/EUR)  │ Evolución dinámica de datos │
└────────────────────────────┴─────────────────────────────┘
