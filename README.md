# Repositorio Página Web de Simuladores de Estadística en R de DEIOAC 

Este repositorio almacena el contenido de la página web de simuladores estadísticos, mientras que además, sirve como template para el desarrollo de nuevos simuladores.

Este repositorio de ha creado en en R usando **Shiny**, compilación a **ShinyLive (WebAssembly)** y su integración en un documento estilo **Quarto**.

Cada simulador vive en su propia carpeta dentro de `simuladores/`.

---

## 🧰 Requisitos

Los siguientes programas deben estar instalados:

- **Git** 
  https://docs.github.com/en/get-started/git-basics/set-up-git  
- **Quarto** 
  https://quarto.org/docs/download/  
- **R 4.5.x**  
  https://cran.r-project.org/bin/windows/base/  
- **RStudio**

---

## 🚀 Crear un nuevo proyecto desde GitHub

1. Abrir RStudio → **File → New Project → Version Control**  

2. Si Git está correctamente instalado, aparecerá la opción *Git*.
<img width="546" height="392" alt="git_r" src="https://github.com/user-attachments/assets/61d6cc43-e26d-4213-8407-234fba6056e3" />


3. En **Repository URL**, introducir la dirección de este repositorio:

```
https://github.com/AlbertoAltozano/SimuladoresEstadistica
```

4. Elegir el nombre de la carpeta que queremos crear para el proyecto (**Project Directory Name**) y su ubicación (**Browse…**).

Tras crearse, el proyecto tendrá una estructura similar a esta:  
```
.
|-- styles.css
|-- _quarto.yml
|-- about.qmd
|-- index.qmd
|-- renv.lock
|-- ...etc
|
|-- _site/
|-- categorias/
|-- renv/
|-- simuladores/
```


## 📂 Estructura del proyecto

### Archivos `.qmd`
Son archivos de **Quarto**, usados para generar la web estilo blog.

- `index.qmd` → página principal  
- `about.qmd` → información general
  
### Carpetas importantes

- **`_site/`**  
  Sitio web renderizado por Quarto. *No editar aquí.*

- **`categorias/`**  
  Contiene las categorías de las apps.  
  Si se añade una categoría, también debe añadirse en `index.qmd`.

- **`renv/`**  
  Entorno de R del proyecto (no tocar manualmente).

- **`simuladores/`**  
  **Carpeta de trabajo real.**  
  Cada simulador vive dentro de su propia carpeta.

---

## 🧱 Crear un nuevo simulador

1. Duplicar la carpeta `template/` dentro de `simuladores/`  
2. Renombrar la carpeta por `/nombre_de_tu_carpeta/` (ej.: `ttest`)

### Editar la información de la app

Dentro de tu carpeta renombrada:

- Abrir `index.qmd`
- Cambiar:
  - Título
  - Descripción
  - Categoría
  - Imagen
  - En el `iframe`.
    Reemplazar la Ruta donde aparece `/template/` → reemplazar por `/nombre_de_tu_carpeta/`
    Reemplazar el título del iframe
    
---

## 🖥 Crear la aplicación Shiny

1. Entrar en la carpeta `appr/`  
2. Editar el archivo `app.R` (ya incluye una plantilla básica de Shiny)

### Instalar dependencias antes de desarrollar

1. Abrir `app.R`
2. En la **consola de R** (no en terminal), ejecutar:

```r
renv::restore()
renv::activate()
```

3. Instalar paquetes faltantes cuando RStudio lo pida.
4. Instalar manualmente los siguientes:

```r
install.packages("shinylive")
install.packages("S7")
install.packages("munsell")
```

5. Verificar que Shiny funciona ejecutando la aplicación:  
   RStudio → **Run App**

Si falla, instalar Shiny manualmente:

```r
install.packages("shiny")
```

Ahora ya podemos desarrollar nuestra app realizando cambios en app.R y la podremos probar usando **Run App**.
También podríamos ir desarrollando nuestra app.R desde otro proyecto y después seguir el proceso descrito en este documento para añadir esa nueva app a la web.

---

## 🌐 Compilar la app a ShinyLive (WebAssembly)

Cuando uno quiera compilar la app de shiny para la web ha de utilizar el siguiente comando:

```r
shinylive::export(
  "./simuladores/nombre_carpeta_mi_app/appr",
  "./simuladores/nombre_carpeta_mi_app/appsite"
)
```

Ejemplo:

```r
shinylive::export("./simuladores/template/appr", "./simuladores/template/appsite")
```

Si la exportación es exitosa, R sugerirá ejecutar:

```r
httpuv::runStaticServer("./simuladores/nombre_carpeta_mi_app/appsite")
```

Esto abrirá un HTML local con la app compilada en WebAssembly.

---

## 🔄 Flujo de trabajo recomendado

Mientras desarrollas:

- Usa **Run App** para probar la app en Shiny.
- Compila a **ShinyLive** cada vez que añadas librerías nuevas o funciones complejas.

---

## 🧱 Compilar la web de Quarto

Una vez hayamos acabado de desarrollar nuestra nueva shiny app y la hayamos compilado en shinylive, hemos de regenerar la web:

### Ejecutar en el **Terminal** (no en consola R):

```bash
quarto render
```

---

## 🧼 Uso de Git: cómo evitar pisarse cambios

### Antes de trabajar en una nueva app

En el **Terminal de R**:

```bash
git pull
```

Así aseguras que tienes los cambios de los demás.

---

## ⬆️ Subir tus cambios a GitHub

Finalmente, si ya hemos creado la app, compilado a shinylive y regenerado la web quarto, podemos subir los cambios a Github.

### 1. Comprobar que Git está instalado

En el **Terminal de R**:

```bash
git
```

### 2. Ver la rama actual

```bash
git branch
```

Debe ser `main`.

### 3. Añadir los cambios

#### a) Si has creado/modificado solo tu app:

```bash
git add .
```

#### b) Si has añadido librerías nuevas en R:

```r
renv::snapshot()
```

Luego:

```bash
git add .
```

### 4. Crear el commit

```bash
git commit -m "Descripción clara de lo que se hizo"
```

### 5. Comprobar si otros han subido cambios

```bash
git pull
```

- Si hay conflictos, Git lo indicará.
- Si no, continúa.

### 6. Subir tus cambios

```bash
git push
```

---

## 📌 Resumen rápido de comandos útiles

```
# Antes de trabajar
git pull

# Duplicar la carpeta template en ./simuladores/ para empezar a trabajar a desarrollar shinyapp en app.R
# Editar index.qmd
```
```
# Al acabar de trabajar con la shinyapp
shinylive::export("./simuladores/template/appr", "./simuladores/template/appsite")
```
```
# Al ir a subir cambios a git
quarto render

# Añadir cambios
git add .

# Guardar cambios
git commit -m "Descripción"

# Asegurar que no hay cambios nuevos en remoto
git pull

# Subir cambios a GitHub
git push
```
