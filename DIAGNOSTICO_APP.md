# Diagnóstico rápido de la app `finanzas-ml`

## Inventario de archivos

- `index.html`: aplicación principal (UI, estilos y lógica JS en un único archivo).
- `service-worker.js`: cache offline básico para PWA.
- `manifest.json`: metadatos PWA (nombre, colores, iconos, orientación).
- `icon-192.png` y `icon-512.png`: iconos para instalación PWA.

## Mejoras recomendadas (priorizadas)

### 1) Seguridad (alta)
1. **Eliminar `innerHTML` para renderizar gastos** y usar `textContent` + nodos DOM.
   - Motivo: hoy se inyecta `desc` directamente en HTML y existe riesgo de XSS si el usuario pega contenido malicioso.
2. **Fijar versión e integridad del CDN de Chart.js** (`integrity` + `crossorigin`) o mover librería a local.
   - Motivo: reduce riesgo de cadena de suministro y errores por cambios inesperados de versión.

### 2) Calidad de datos y UX (alta)
1. **Normalizar inputs con `type="number"`** en sueldo, ahorro, monto y cuotas.
2. **Validaciones explícitas**:
   - cuotas > 0
   - monto > 0
   - descripción no vacía y con `trim()`
3. **Formateo al escribir** (máscara CLP) y mensajes de error visibles en pantalla.

### 3) Funcionalidad financiera (media)
1. **Editar y eliminar gastos individuales** (hoy solo se agregan).
2. **Filtro por categoría** (arriendo, comida, transporte, etc.).
3. **Resumen anual**: ahorro total, gasto promedio mensual, mes con mayor gasto.
4. **Exportar/Importar JSON** para backup de `localStorage`.

### 4) PWA y rendimiento (media)
1. **Mejorar estrategia de cache** en `service-worker.js`:
   - `cache-first` para assets estáticos.
   - `stale-while-revalidate` para CDN.
2. **Versionado y limpieza de caches antiguas** en evento `activate`.
3. **Agregar fallback offline** (`offline.html`) para navegación sin conexión.

### 5) Mantenibilidad (media)
1. **Separar HTML/CSS/JS** en archivos dedicados (`index.html`, `styles.css`, `app.js`).
2. **Modularizar lógica** (storage, cálculos, UI, gráficos).
3. **Agregar pruebas unitarias** de utilidades (`limpiarNumero`, cálculos de disponible).
4. **Agregar lint/format** (ESLint + Prettier) y script de calidad.

## Propuesta de plan de implementación

1. **Iteración 1 (rápida)**
   - Sanitización de render de tabla + validaciones básicas + inputs numéricos.
2. **Iteración 2**
   - Edición/eliminación de gastos + categorías + resumen anual.
3. **Iteración 3**
   - Refactor a archivos separados + tests + mejoras de PWA.

## Resultado esperado

Con estas mejoras, la app debería quedar más segura, más estable para uso diario y más fácil de escalar sin perder simplicidad.
