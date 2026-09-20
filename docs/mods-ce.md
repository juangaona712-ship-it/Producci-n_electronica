# 3. Manufactura CAM y Generación de Trayectorias (Mods CE)

!!! abstract "Objetivo de esta fase"
    Una vez validados los diseños y generados los archivos de las capas de nuestra placa en KiCad, es necesario "traducirlos" a un formato de trayectorias espaciales (G-Code/Toolpaths) que la fresadora CNC **Roland Monofab (SRM-20)** pueda interpretar para realizar el corte y desgaste físico del cobre.
    Para realizar esta conversión utilizaremos **Mods CE** (Community Edition), una plataforma basada en navegador e ideal para la generación de trayectorias de fresado de PCBs mediante un sistema de nodos.

---

## 3.1 Exportación Vectorial desde KiCad (Archivos Base)

El proceso comienza aislando y exportando las capas necesarias desde el editor de KiCad en formato vectorial (`.svg`). 

Para que la máquina pueda saber qué hacer, es necesario seguir estos pasos: primero, una vez que tenemos nuestra placa terminada, nos dirigimos al menú superior y seleccionamos la ruta **`Archivo > Salidas de fabricación > Gerbers...`**. Esta herramienta nos permite compilar la geometría de la PCB.

![Menú Salidas de Fabricación](img/fotos_mods/Imagen42.jpg)
*Figura 3.1: Acceso al módulo de exportación de archivos de fabricación.*

En la ventana de trazado, el formato predeterminado suele ser Gerber. Debemos abrir el menú desplegable en la parte superior izquierda.

![Formato Gerber por defecto](img/fotos_mods/Imagen43.jpg)
*Figura 3.2: Selección del menú de formatos de trazado.*

Cambiamos el formato de trazado estrictamente a **SVG**. A diferencia de los archivos Gerber tradicionales, el formato SVG (Scalable Vector Graphics) genera imágenes monocromáticas de alto contraste que Mods CE utiliza para calcular las operaciones de desgaste de cobre.

Ahora en la flecha verde (columna izquierda) seleccionamos solo las capas que utilizaremos (pistas y cortes de borde). La opción que está señalada por la flecha azul (**"Comprobar relleno de zonas antes de trazar"**) es necesario activarla obligatoriamente para garantizar la integridad de los planos de tierra. Al final le picamos "Trazar".

![Configuración de trazado SVG](img/fotos_mods/Imagen44.jpg)
*Figura 3.3: Ajuste de parámetros vectoriales y selección de capas a exportar.*

Los archivos estarán guardados en formato Microsoft Edge documents (o el navegador por defecto), con la extensión en SVG. Debemos tener listos los archivos correspondientes a las **pistas**, **perforaciones**, **bordes** y **etiquetas** (opcional).

| Archivos Exportados Localmente | Archivos Base Listos para Mods CE |
| :---: | :---: |
| ![Archivos exportados](img/fotos_mods/Imagen45.jpg) | ![Archivos SVG exportados](img/Imagen%2054.png) |

---

## 3.2 Acceso y Configuración del Entorno Mods CE

Para configurar nuestro espacio de trabajo CAM, seguimos esta ruta de inicialización:

1. Ingresamos a nuestro navegador y realizamos la búsqueda de **"mod ce"** o entramos a la página oficial [Mods CE](https://modsproject.org).

![Búsqueda Mods CE](img/fotos_mods/Imagen46.jpg)
*Figura 3.4: Búsqueda del entorno de procesamiento modular.*

2. En la interfaz principal (identificable por el logo de la carita feliz en la pestaña), hacemos clic derecho o buscamos el menú de opciones en la esquina superior izquierda para abrir los **Programs**.
3. En la barra de búsqueda tecleamos "sr" para filtrar las máquinas, navegamos hacia la sección de máquinas Roland y seleccionamos la opción **`mill 2D PCB`** (señalada por la flecha roja).

![Selección de programa CAM](img/fotos_mods/Imagen47.jpg)
*Figura 3.5: Selección del algoritmo de ruteo 2D para la máquina Roland SRM-20.*

<div style="display: flex; gap: 10px; justify-content: center; margin-top: 15px;" markdown="1">
![Paso 55](img/Imagen%2055.png){ width="30%" }
![Paso 56](img/Imagen%2056.png){ width="30%" }
![Paso 57](img/Imagen%2057.png){ width="30%" }
</div>

Al cargar el programa, se desplegará una red de nodos interconectados (diagrama de flujo de datos) que procesarán nuestro archivo desde el SVG hasta el archivo de corte de la máquina. El paso siguiente es venir al primer apartado (nodo raíz) para seleccionar el archivo.

| Nodo Raíz de Inserción | Entorno Completo de Nodos |
| :---: | :---: |
| ![Nodo inicial de Mods CE](img/fotos_mods/Imagen48.jpg) | ![Entorno de nodos de Mods CE](img/Imagen%2058.png) |

---

## 3.3 Configuración de Pistas (Traces)

Comenzaremos procesando el archivo de las pistas (`Pistas.svg`). En el nodo de entrada `Roland Monofab PCB`, seleccionamos y cargamos nuestro archivo SVG.

![Archivo SVG cargado](img/fotos_mods/Imagen49.jpg)
*Figura 3.6: Geometría de las pistas importada exitosamente al entorno CAM.*

Ahora damos clic en el botón **`invert`**. Esto es un paso técnico crítico: le indica a la máquina que el objetivo es realizar un fresado de aislamiento (remover el cobre *alrededor* de los vectores) y no taladrar sobre las líneas de la pista, lo cual destruiría nuestro circuito.

![Inversión de vectores](img/fotos_mods/Imagen50.jpg)
*Figura 3.7: Proceso de inversión lógica para ruteo de aislamiento.*

A continuación, configuramos los parámetros de la herramienta física:

!!! tip "Parámetros de la Broca"
    Para el fresado de las pistas utilizaremos una broca plana estándar. En el nodo de configuración (*set PCB defaults*), bajamos un poco en la página, damos clic donde señala la flecha azul (para cambiar a mm) y escogemos la opción **`0.40mm flat`** señalada por la flecha roja (lo que equivale aproximadamente a 1/64 de pulgada).

| Selección de Herramienta | Parámetros del Nodo |
| :---: | :---: |
| ![Selección de fresa de ruteo](img/fotos_mods/Imagen51.jpg) | ![Configuración de herramienta](img/Imagen%2059.png) |

### Ajuste de Pasadas (Offsets)
En el nodo **mill raster 2D**, definiremos cuánto material queremos remover alrededor de cada pista:
*   **Offset number:** Lo configuramos en `2`. Esto indica que el taladro realizará dos pasadas concéntricas alrededor de las pistas para asegurar un aislamiento adecuado, ajustado a este valor para evitar problemas técnicos de ruteo.
*   Una vez configurado, hacemos clic en el botón **Calculate**.

| Nodo General | Cálculo de Trayectorias |
| :---: | :---: |
| ![Configuración general](img/fotos_mods/Imagen52.jpg) | ![Cálculo de trayectorias](img/Imagen%2060.png) |

---

## 3.4 Visualización y Renderizado

Al presionar *Calculate*, Mods CE generará visualmente el trazado de las rutas de corte de la herramienta (toolpath). 

![Plano de trayectorias 2D](img/Imagen%2061.png)

Podemos hacer clic en el botón **View** para obtener un renderizado 3D de cómo quedará la placa físicamente. 

!!! warning "Interpretación del Renderizado"
    En el renderizado 3D, **el área oscura representa el cobre que será removido** por la fresadora, mientras que el área clara e intacta representa nuestras pistas y pads eléctricos. 

![Renderizado 3D de la placa](img/Imagen%2062.png)

---

## 3.5 Origen y Velocidad (Nodo SRM-20)

El paso final antes de exportar el archivo es configurar los parámetros físicos y la cinemática de la máquina en el nodo final **Roland SRM-20 milling machine**:

*   **Speed (Velocidad):** Ajustamos la velocidad de fresado a **4 mm/s** para evitar rupturas en la broca y asegurar cortes limpios en las pistas.
*   **Origin (Origen de coordenadas):** El siguiente paso es definir los ejes y escribiremos **0** en los 3 ejes (**X: 0, Y: 0, Z: 0**) donde se señala con las flechas rojas para la fabricación de una placa individual principal.

!!! tip "Optimización de Material (Multipanel)"
    Es importante saber que si queremos agregar otra placa para imprimir (panelización), solo modificamos el eje de las X, con un margen de 2 mm extra para que se hagan adecuadamente los bordes perimetrales sin chocar.

| Origen 0,0,0 (Detalle) | Configuración final de máquina |
| :---: | :---: |
| ![Configuración de ejes y velocidades](img/fotos_mods/Imagen53.jpg) | ![Configuración final de máquina](img/Imagen%2063.png) |

---

## 3.6 Guardado y Organización de Archivos

Para finalizar con el primer documento, nos dirigimos a este apartado de cálculo, confirmamos que todo esté en orden, nos vamos al final de la página (al último nodo) y le picamos al botón **`save file`**. El navegador descargará automáticamente un archivo con la extensión `.rml` (Roland Machine Language). 

| Confirmación de Cálculo | Botón Save File |
| :---: | :---: |
| ![Cálculo de rutas de herramienta](img/fotos_mods/Imagen54.jpg) | ![Guardado del archivo de manufactura](img/fotos_mods/Imagen55.jpg) |

!!! danger "Importante: Renombrar los archivos"
    Por defecto, Mods CE guarda todos los archivos bajo el nombre genérico `SVG image.rml`. Es **crucial** ubicar el archivo descargado inmediatamente y renombrarlo (por ejemplo, a `1_Pistas.rml`) para mantener una organización estricta y evitar confusiones fatales al momento de operar la fresadora.

| Botón Save File (Detalle) | Archivo Descargado |
| :---: | :---: |
| ![Paso 64](img/Imagen%2064.png) | ![Paso 65](img/Imagen%2065.png) |

![Archivos RML exportados](img/fotos_mods/Imagen56.jpg)
*Figura 3.8: Archivos RML listos y renombrados correctamente en el directorio.*

---

## 🛠️ Solución de Problemas Frecuentes

Durante el procesamiento de las pistas, pueden surgir un par de complicaciones comunes que tienen solución rápida:

1.  **Áreas de corte invertidas:** Si al ver el renderizado 3D notas que la máquina cortará el cobre que querías conservar (dejando expuesto lo que querías quitar), dirígete al nodo **convert SVG image** y haz clic en el botón **invert**. Esto corregirá la polaridad de la imagen.
2.  **Errores en el contorno:** Si la placa presenta bordes irregulares o el SVG no fue interpretado correctamente desde KiCad, la mejor práctica es abrir el archivo original en **Inkscape** para corregir y unificar los vectores antes de subirlo a Mods CE.

![Error contorno](img/Imagen%2066.png)

---

## 3.7 Configuración de Perforaciones (Drills)

Una vez asegurado el archivo de las pistas, repetiremos el proceso para las perforaciones cargando el archivo `Perforaciones.svg`. La lógica de los nodos es idéntica, pero los parámetros de corte cambian.

1.  **Herramienta:** En el nodo *set PCB defaults*, la punta depende de la capa. Para perforación usamos **0.79mm drill** (que corresponde a nuestra broca de 0.8 mm).
2.  **Pasadas (Offsets):** En el nodo de cálculo (*mill raster 2D*), configuramos el **offset number** en `1`. A diferencia de las pistas, aquí solo necesitamos que la broca baje exactamente en el centro una sola vez por cada agujero.
3.  Hacemos clic en **Calculate** y luego en **View** para verificar que la posición de los agujeros coincida perfectamente con los pads de nuestro diseño.

| Carga de Perforaciones | Selección de Herramienta |
| :---: | :---: |
| ![Paso 67](img/Imagen%2067.png) | ![Paso 68](img/Imagen%2068.png) |

| Ajuste de Offsets | Cálculo de Rutas |
| :---: | :---: |
| ![Paso 69](img/Imagen%2069.png) | ![Paso 70](img/Imagen%2070.png) |

![Render Perforaciones](img/Imagen%2071.png)
*Renderizado 3D de las perforaciones calculadas.*

### Parámetros Críticos y Guardado (Perforaciones)

!!! danger "Velocidad de Corte Crítica (Speed)"
    En el nodo final **Roland SRM-20 milling machine**, la velocidad aparte cambia en perforaciones. La punta es sumamente delicada; es **obligatorio reducir la velocidad a `0.3 mm/s`**. Las brocas de perforación de 0.8 mm son extremadamente frágiles; si la máquina intenta taladrar muy rápido o entra al material de forma inestable, la broca se romperá instantáneamente.

En este mismo nodo, podemos observar el tiempo estimado de trabajo (*Estimated time*) en la parte inferior, lo cual es muy útil para planificar el uso de la máquina en el laboratorio. Finalmente, nos dirigimos al nodo **save file**, hacemos clic para descargar, y renombramos inmediatamente este nuevo archivo (por ejemplo, a `2_Perforaciones.rml`).

| Ajuste a 0.3 mm/s | Guardado de Perforaciones |
| :---: | :---: |
| ![Paso 72](img/Imagen%2072.png) | ![Paso 73](img/Imagen%2073.png) |

---

## 3.8 Configuración del Corte de Contorno (Cutout / Edge)

Para realizar el corte perimetral que separará la placa del material base, cargamos el archivo vectorizado del contorno (por ejemplo, `Bordes.svg`).

1.  **Carga del archivo:** En el nodo **read SVG**, seleccionamos el archivo `Bordes.svg`.

| Carga de SVG de Bordes |
| :---: |
| ![Carga del archivo de bordes](img/Imagen%2074.png) |

2.  **Ajuste del diámetro de herramienta (Manual):** En el nodo **set PCB defaults**, observaremos que en la sección *Cutout* la opción predeterminada de mayor diámetro es `1.59mm cutout`. Dado que utilizaremos una fresa más gruesa, seleccionamos la opción *1.59 custom* y modificamos manualmente el campo **diameter (mm)** escribiendo **`1.9`** o **`2.0`** (o ajustando el parámetro equivalente dentro del nodo de cálculo).
3.  **Pasadas (Offsets):** Mantenemos el parámetro **offsets** en `1` para realizar un único trazo perimetral alrededor de la placa. La velocidad la regresamos a **4 mm/s**.

| Parámetros PCB y Ajuste Manual | Cálculo de Trayectoria de Borde |
| :---: | :---: |
| ![Configuración de diámetro y pasadas](img/Imagen%2075.png) | ![Parámetros de cálculo del contorno](img/Imagen%2076.png) |

### Verificación y Exportación del Contorno

1.  **Cálculo de la trayectoria:** En el nodo **mill raster 2D**, hacemos clic en **Calculate** para generar el código de corte.
2.  **Visualización y Renderizado:** Presionamos **View** para inspeccionar la trayectoria en 2D y verificar la simulación 3D de la placa recortada.

| Vista 2D del Contorno | Renderizado 3D de Recorte |
| :---: | :---: |
| ![Trayectoria 2D del contorno](img/Imagen%2077.png) | ![Renderizado 3D del corte de contorno](img/Imagen%2078.png) |

3.  **Parámetros de máquina y guardado:** Verificamos el tiempo estimado de trabajo en el nodo **Roland SRM-20 milling machine** y procedemos a descargar el archivo generado desde el nodo **save file**.
4.  **Organización:** Renombramos inmediatamente el archivo descargado a `3_Contorno.rml`. Para finalizar, aplicamos los mismos cambios y revisiones a los 3 documentos generados.

| Ajustes de Máquina | Archivo Generado RML |
| :---: | :---: |
| ![Parámetros finales y tiempo estimado](img/Imagen%2079.png) | ![Descarga del archivo RML de contorno](img/Imagen%2080.png) |

---

## 3.9 Instalación del Software de Control (VPanel para SRM-20)

Una vez generados los tres archivos de trabajo (`.rml`), es necesario instalar el software del fabricante para controlar la fresadora Roland SRM-20 y enviar las instrucciones de mecanizado.

1.  **Enlace de descarga:** Dirígete a la página de inicio del proyecto/documentación y haz clic en el icono con la imagen de la fresadora SRM-20.

| Acceso Directo |
| :---: |
| ![Acceso a descarga de software](img/Imagen%2082.png) |

2.  **Centro de descargas:** Al abrirse el *Download Center* de Roland / DGSHAPE, selecciona el modelo **monoFab SRM-20** y dirígete a la pestaña de **Software**. Busca en la lista el programa **VPanel for SRM-20**.
3.  **Licencia y descarga:** Acepta los términos del contrato de licencia haciendo clic en el botón **Agree** para iniciar la descarga del instalador ejecutable.

| Portal de Descargas Oficial | Contrato de Licencia |
| :---: | :---: |
| ![Centro de descargas de Roland SRM-20](img/Imagen%2083.png) | ![Aceptación de la licencia de VPanel](img/Imagen%2084.png) |
