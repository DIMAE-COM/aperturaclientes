# Solicitud de Condiciones Comerciales - Apertura de Clientes

Este proyecto es una aplicación web estática de página única diseñada para rellenar de forma interactiva el formulario de **Solicitud de Condiciones Comerciales** para **DIMAE** (marca comercial de *Dielectro Manchego S.A.*) y exportarlo como un documento PDF en formato A4 listo para imprimir.

---

## 📂 Estructura del Proyecto

El proyecto consta de los siguientes archivos principales:
*   **[index.html](file:///home/froca/git_projects/aperturaclientes/index.html)**: Contiene la estructura del formulario (maquetado con Bootstrap 5.3.3) y la lógica en JavaScript para la interactividad y la conversión a PDF.
*   **[style.css](file:///home/froca/git_projects/aperturaclientes/style.css)**: Define los estilos personalizados de los campos, tablas, bloques y las firmas, además de ajustar el comportamiento visual para simular una hoja física.
*   **Imágenes (`DIMAE.png` e `ISO.png`)**: Logotipos utilizados en la cabecera del documento.
*   **`CNAME`**: Archivo de configuración para el dominio personalizado de hosting estático (p. ej. GitHub Pages).

---

## ⚙️ ¿Cómo Funciona?

El núcleo de esta herramienta es la generación de un PDF fiel al diseño en pantalla. El proceso de exportación se realiza de la siguiente manera:

1.  **Validación**: Cuando el usuario hace clic en el botón **"Descargar PDF"**, la función `descargarPDF()` verifica que se hayan aceptado las dos cláusulas de protección de datos (RGPD) en la parte inferior.
2.  **Preparación del Formulario (Conversión Temporal)**: 
    *   Dado que los navegadores renderizan los elementos `<input>` y `<select>` con bordes y estilos nativos que no siempre se trasladan bien al PDF, el código **reemplaza temporalmente** cada campo de texto (`input.form-control`) y lista desplegable (`select.form-select`) por un elemento `<div>` plano con un borde negro de 1px.
    *   Esto asegura que el texto introducido se vea limpio, con el tamaño y fuente correctos en el PDF, sin bordes de foco ni flechas de selección.
3.  **Captura de Pantalla con `html2canvas`**:
    *   Se fuerza el ancho del contenedor principal `.formularioPDF` a exactamente `1200px` para asegurar una resolución consistente.
    *   Se realiza una captura de pantalla del contenedor usando la librería `html2canvas` con un factor de escala (`scale: 2`) para garantizar alta resolución al imprimir.
4.  **Restauración**: Inmediatamente después de la captura, los `<div>` temporales se eliminan, se muestran de nuevo los inputs originales y se devuelven los estilos de visualización de la página al estado inicial.
5.  **Generación de PDF con `jsPDF`**:
    *   Se crea un documento PDF A4 vertical (`portrait`).
    *   Se calcula la relación de aspecto para ajustar la imagen capturada al tamaño de la página A4 respetando un margen de 5mm.
    *   Se guarda el archivo resultante con el formato `Apertura_Cliente_[CÓDIGO_CLIENTE].pdf`.

---

## 🛠️ Cómo Editar el Documento

### 1. Modificar Textos o Estructura
Si deseas cambiar el nombre de una sección, añadir un nuevo campo o modificar una etiqueta:
1.  Abre el archivo **[index.html](file:///home/froca/git_projects/aperturaclientes/index.html)**.
2.  Localiza el bloque o fila correspondiente (están organizados utilizando la estructura de rejilla de Bootstrap, como `<div class="row">` y `<div class="col-md-X">`).
3.  Edita el texto del `<label>` o añade el nuevo campo siguiendo la misma sintaxis.

### 2. Añadir Delegaciones u Opciones de Menú
Si se abre una nueva delegación o se añade un nuevo grupo de clientes:
1.  Busca el elemento `<select>` correspondiente en **[index.html](file:///home/froca/git_projects/aperturaclientes/index.html)**.
    *   *Delegaciones*: Busca el `<select>` que contiene `CIUDAD REAL`, `TOLEDO`, etc.
    *   *Grupo Cliente*: Busca el `<select>` con las opciones de grupo.
2.  Añade una nueva etiqueta `<option>NUEVA_DELEGACIÓN</option>` dentro del elemento select respectivo.

### 3. Modificar Estilos Visuales
Si necesitas ajustar márgenes, tamaños de fuente o colores:
1.  Abre el archivo **[style.css](file:///home/froca/git_projects/aperturaclientes/style.css)**.
2.  Modifica las reglas css existentes. Ten en cuenta que la mayoría de los campos tienen estilos con `!important` para asegurar que Bootstrap no los sobrescriba y que la exportación sea lo más idéntica posible al diseño impreso.

---

## ⚠️ Reglas de Oro para Editar SIN Romper Nada

Para evitar que el PDF deje de funcionar o se renderice de forma incorrecta al hacer cambios, sigue estas pautas estrictas:

### 📐 1. Mantén las Clases CSS en Nuevos Inputs/Selects
El script de exportación busca elementos específicos usando selectores de clase:
*   Si añades un nuevo input de texto, **debe tener** la clase `form-control` (ej. `<input class="form-control campo">`).
*   Si añades un nuevo select, **debe tener** la clase `form-select` (ej. `<select class="form-select campo">`).
*   *¿Por qué?* Si no usas estas clases, la función `descargarPDF()` no los convertirá a `<div>` antes de la captura, provocando que se impriman con el diseño interactivo del navegador (o vacíos si no capturan su valor).

### 🏷️ 2. No elimines los IDs ni Clases Clave
*   El contenedor principal de la captura tiene la clase `formularioPDF`. No cambies su nombre ni lo elimines.
*   El input del código de cliente tiene el ID `Cod_CLiente` (ej. `<input class="form-control campo" id="Cod_CLiente">`). Este ID es utilizado para nombrar el archivo PDF generado. Si lo borras, el archivo siempre se guardará como `Apertura_Cliente_SIN_CODIGOCLIENTE.pdf`.
*   Los checkboxes de consentimiento de datos y comercial tienen los IDs `consentimientoDatos` y `consentimientoComercial`. Son necesarios para la validación previa a la descarga.

### 📏 3. Respetar el Ancho de Diseño (`1200px`)
El formulario se renderiza y captura a un ancho fijo de `1200px`. 
*   Evita usar anchos fijos mayores en tus elementos personalizados.
*   Usa siempre la rejilla de Bootstrap (`row` y `col-md-X`) para que los campos se distribuyan proporcionalmente dentro de ese ancho fijo.

### 🔍 4. Nota sobre el Código Huérfano en `index.html` (Líneas 176-212)
En el archivo **[index.html](file:///home/froca/git_projects/aperturaclientes/index.html)**, justo después del cierre de la función `descargarPDF()`, existe un bloque de código suelto:
```javascript
document.querySelectorAll("input").forEach(input => { ... });
```
Este código se ejecuta inmediatamente al cargar la página para vaciar todos los campos del formulario.
*   **Recomendación**: Si vas a reorganizar las funciones de JavaScript, ten cuidado con este fragmento. Actualmente realiza una función similar a `limpiarFormulario()` pero de forma automática en la carga. Si quieres que el formulario cargue con valores preestablecidos desde el HTML, deberás eliminar o comentar este bloque suelto.

---

## 💻 Cómo Probar el Proyecto Localmente

> [!NOTE]
> Las imágenes del logotipo (`ISO.png` y `DIMAE.png`) han sido **incrustadas directamente como cadenas Base64** dentro de [index.html](file:///home/froca/git_projects/aperturaclientes/index.html). Esto elimina las restricciones de seguridad por CORS (Tainted Canvas), permitiendo que el botón **Descargar PDF** funcione perfectamente al abrir el archivo haciendo doble clic (usando el protocolo `file://`).

Aunque ya funciona abriéndolo directamente, si en algún momento necesitas servir la aplicación web en una red local o realizar desarrollos más complejos, puedes iniciar un servidor web local:
1.  Abre una terminal en el directorio del proyecto.
2.  Ejecuta un servidor web local sencillo. Por ejemplo:
    *   Si tienes **Node.js** instalado:
        ```bash
        npx live-server
        ```
        o bien:
        ```bash
        npx http-server
        ```
    *   Si prefieres usar **Python**:
        ```bash
        python3 -m http.server 8000
        ```
3.  Accede a la dirección indicada (usualmente `http://localhost:8000` o `http://127.0.0.1:8080`) desde tu navegador.
4.  Rellena los datos, acepta los checks y prueba el botón **Descargar PDF** para comprobar que todo se alinee correctamente.
