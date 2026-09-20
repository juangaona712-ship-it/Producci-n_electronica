# Editor de Placas (PCB Layout)

Una vez que el diseño lógico ha sido validado en el esquemático, el siguiente paso crítico en nuestro flujo de trabajo es la traducción de este circuito a su forma física. En esta sección documentamos la importación de huellas (footprints), la definición del área de trabajo y la preparación para el ruteo.

## 1. Transición al Entorno de PCB

Para comenzar con el diseño físico, utilizamos el botón **Abrir editor de placas** ubicado en la barra de herramientas superior del Eeschema.

![Botón de cambio al editor de placas](img/Imagen 20.png)
*Icono de acceso al editor de PCB (Pcbnew).*

!!! warning "Importante: Gestión del Proyecto"
    Es fundamental realizar este paso con la ventana principal del **Proyecto de KiCad** abierta en segundo plano. Si se abre el editor de placas de manera independiente, el software perderá el enlace con el esquemático, imposibilitando la sincronización de componentes y arrojando errores de conectividad al intentar actualizar.

## 2. Sincronización de Componentes

Con el editor de placas abierto, procedemos a importar los componentes lógicos a su representación física. Esto se logra ejecutando la herramienta **Actualizar placa desde esquema** (atajo de teclado `F8`).

![Ventana de actualización de componentes](img/Imagen 21.png)
*Gestor de actualización. Aquí verificamos que se procesen los símbolos sin errores.*

Al aplicar los cambios, las huellas de los componentes aparecerán agrupadas. En este punto, es visible una red de líneas delgadas que interconectan los pines; a este conjunto de guías se le conoce como **Ratsnest** (nido de ratas), el cual nos indica el orden, la topología y el destino exacto de cada conexión.

![Componentes importados con ratsnest](img/Imagen 23.png)
*Disposición inicial de los componentes interconectados por el ratsnest.*

## 3. Configuración de Capas de Trabajo

Antes de iniciar el trazado de pistas o contornos, debemos comprender el apilamiento de capas (Layer Stackup). Dado que el diseño actual es una placa de cara simple (una sola capa de cobre), todo nuestro trabajo conductivo se realizará en la capa **F.Cu** (Front Copper / Cobre Frontal).

![Selección de capa frontal de cobre](img/Imagen 24.png)
*Gestor de capas posicionado en la capa principal de cobre (F.Cu).*

## 4. Delimitación del Contorno (Edge.Cuts)

Toda PCB necesita un límite físico definido para que la máquina CNC o el fabricante sepa por dónde cortar la placa terminada. Para trazar este límite, debemos cambiar nuestra capa activa a **Edge.Cuts** (Cortes de borde).

![Capa Edge.Cuts seleccionada](img/Imagen 27.png)
*Capa dedicada exclusivamente a la geometría del corte.*

Utilizando la herramienta de formas (Cuadrado/Rectángulo), trazamos un perímetro que encapsule todos nuestros componentes. Para darle mayor robustez visual al diseño y evitar problemas de tolerancias durante el corte, ajustamos las propiedades de la forma asignándole un **ancho de línea de 2 mm**.

![Propiedades del rectángulo de corte](img/Imagen 28.png)
*Ajuste de parámetros geométricos y grosor de línea del borde.*

![Contorno de la placa finalizado](img/Imagen 29.png)
*Contorno de la PCB delimitando exitosamente el área de trabajo física.*

## 5. Herramientas Esenciales para la Siguiente Fase

Para las etapas de distribución y enrutamiento, nos apoyaremos fuertemente en dos herramientas de la barra lateral izquierda:

*   **Enrutar pistas (X):** La herramienta principal que nos permitirá convertir las líneas virtuales del ratsnest en pistas reales de cobre.
![Herramienta de enrutamiento](img/Imagen 25.png)

*   **Herramienta de medida (Ctrl+Shift+M):** Crucial para verificar tolerancias, separaciones mínimas entre componentes y confirmar las dimensiones físicas reales de la placa.
![Herramienta de medición](img/Imagen 26.png)

---

## 6. Reglas de Diseño (DRC) y Dimensiones

Antes de comenzar a trazar pistas, es imperativo establecer las reglas de diseño basadas en las capacidades de nuestro método de fabricación. 

En este proyecto, la manufactura se realizará mediante fresado CNC utilizando una **Roland Monofab SRM-20**. Por lo tanto, debemos configurar los anchos mínimos y tolerancias para evitar que la broca rompa pistas delgadas o fusione conexiones cercanas.

![Menú rápido de ancho de pistas](img/Imagen 30.png)
*Selección rápida de anchos de pista predefinidos.*

Accedemos a la **Configuración de la placa** para definir estos parámetros:

![Configuración de tamaños predefinidos](img/Imagen 31.png)
*Agregamos anchos predefinidos (Ej. 0.4 mm y 0.8 mm).*

![Reglas de diseño y requerimientos](img/Imagen 32.png)
*Definición de márgenes mínimos, aislamiento y tolerancias de diseño.*

!!! info "Parámetros para CNC Monofab SRM-20"
    Se estableció un **ancho de pista de 0.4 mm** para las líneas de señal. Este grosor garantiza que la fresadora pueda aislar las pistas correctamente sin comprometer su integridad mecánica ni su conductividad.

## 7. Enrutamiento y Conexiones

Con las reglas establecidas, comenzamos el proceso de enrutado. Al seleccionar la herramienta de pistas y hacer clic sobre un pad, KiCad ilumina automáticamente los pads de destino correspondientes, facilitando la visualización del camino a seguir.

![Proceso de enrutamiento inicial](img/Imagen 33.png)
*El software resalta en color brillante los pads que deben interconectarse.*

![Conexión completada entre dos pads](img/Imagen 34.png)
*Pista trazada de punto a punto respetando las reglas de diseño.*

## 8. Trucos de Ruteo: Puentes (Jumpers)

Al trabajar exclusivamente en una cara (Capa `F.Cu`), es común encontrarnos con cruces inevitables donde una pista bloquea el paso de otra. 

Para solucionar esto sin requerir una placa de dos capas, implementamos el uso de una **resistencia de 0 ohmios (R_0)** que actúa físicamente como un puente o "jumper", permitiendo que una señal salte por encima de otras pistas.

![Puente implementado en el ruteo](img/Imagen 35.png)
*Uso del componente R_0 para resolver un cruce de pistas en capa simple.*

!!! tip "Consideración en el DRC"
    El uso de este puente puede generar advertencias en el chequeo de reglas de diseño (DRC), ya que lógicamente el software lo ve como una interrupción. Sin embargo, en la manufactura física, esta es una técnica completamente válida y la advertencia puede ser ignorada.

## 9. Perforaciones Manuales (Headers)

Para la colocación de pines (pin headers), necesitamos definir perforaciones precisas. Para no interferir con las capas estándar, utilizamos una capa de usuario (`User.1` o `User.2`).

Con la herramienta de círculo, dibujamos guías con la propiedad de **Rellenar con Sólido** y un radio de aproximadamente **0.72 mm a 0.8 mm** (dependiendo del calibre del pin). Es vital asegurarse de que estos círculos queden perfectamente centrados en el pad para guiar correctamente la broca de la CNC.

![Propiedades de círculo de perforación](img/Imagen 36.png)
*Ajuste manual del radio y relleno en una capa de usuario.*

## 10. Creación de Zonas de Cobre (Copper Pour)

El último paso del diseño físico consiste en generar una zona de relleno (Copper Pour). Esto delimita exactamente el área de cobre que la CNC debe procesar y optimiza el tiempo de fresado al evitar remover material innecesario.

![Herramienta de zonas rellenas](img/Imagen 37.png)
*Selección de la herramienta para dibujar zonas.*

Ubicados nuevamente en la capa **F.Cu**, trazamos un polígono por el interior de nuestro contorno (`Edge.Cuts`) y configuramos las propiedades de la zona, ajustando los parámetros térmicos y de aislamiento térmico necesarios.

![Propiedades de la zona de cobre](img/Imagen 39.png)
*Configuración del polígono de cobre sin asignación de red (`<sin red>`).*

## 11. Ejecución del Relleno de Cobre (Copper Pour)

Al trazar la zona de cobre, existe una regla fundamental para que el software procese el área correctamente: **el polígono debe ser un lazo completamente cerrado**. El punto final del trazado debe hacer clic exactamente sobre el punto inicial.

![Trazado del polígono de la zona de cobre](img/Imagen 40.png)
*El contorno del área de relleno delimitando todo el circuito.*

Una vez que el polígono está cerrado, presionamos la tecla **`B`** en nuestro teclado. Este comando obliga a KiCad a recalcular y renderizar el relleno de cobre, esquivando automáticamente las pistas y los pads según las reglas de aislamiento (clearance) que configuramos previamente.

![Detalle de las pistas aisladas del relleno](img/Imagen 41.png)
*Vista de cerca mostrando cómo el relleno de cobre respeta el margen de las pistas.*

![Placa con el relleno de cobre ejecutado](img/Imagen 42.png)
*Vista superior de la PCB con la capa F.Cu completamente rellenada.*

![Placa finalizada (Modo contraste)](img/Imagen 43.jpeg)
*Visualización general de la placa terminada y lista para verificación.*

## 12. Inspección en el Visor 3D

Antes de exportar los archivos para fabricación, es una excelente práctica revisar el aspecto físico de la placa. Accedemos al **Visor 3D** desde el menú superior o utilizando el atajo de teclado **`Alt + 3`**.

![Acceso al Visor 3D](img/Imagen 44.png)
*Menú desplegable para iniciar la previsualización 3D.*

Esta herramienta nos permite rotar la placa, inspeccionar la colocación de los componentes SMD/THT y verificar que las perforaciones mecánicas (headers) no colisionen con las pistas.

![Vista 3D de la PCB](img/Imagen 45.png)
*Renderizado 3D de la placa mostrando los componentes y el cobre desnudo.*

![Vista isométrica de la placa terminada](img/Imagen 46.jpeg)
*Inspección final de la distribución física.*

## 13. Verificación de Reglas de Diseño (DRC)

El último filtro de seguridad antes de fabricar es ejecutar el **DRC (Design Rule Checker)**. A diferencia del ERC en el esquemático, el DRC verifica errores físicos: pistas demasiado juntas, cruces no intencionados o zonas sin conexión.

![Botón Ejecutar DRC](img/Imagen 49.png)
*Lanzamiento de la herramienta de inspección de reglas de diseño.*

!!! tip "Gestión de Avisos (Warnings)"
    Es posible que el DRC arroje "Avisos" menores. Dependiendo del contexto, algunos pueden ser ignorados de forma segura. Si existe duda sobre un error específico, es altamente recomendable consultar la documentación oficial (Blog de KiCad) o su comunidad en Discord antes de proceder a maquinar.

## 14. Salidas de Fabricación (Exportación SVG)

Para procesar nuestra placa en la fresadora CNC Monofab SRM-20, necesitamos exportar el diseño en un formato vectorial. Nos dirigimos a **`Archivo > Salidas de fabricación`** y seleccionamos la herramienta de trazado.

![Menú de Salidas de Fabricación](img/Imagen 47.png)
*Acceso al menú de exportación de archivos.*

En la ventana de Trazar, configuramos los parámetros exactos para nuestro método de manufactura:

1.  **Formato de trazado:** Seleccionamos `SVG`.
2.  **Opciones SVG:** Marcamos la casilla **"Ajustar página a la placa"** para evitar exportar coordenadas vacías.
3.  **Incluir capas:** Seleccionamos estrictamente las capas necesarias para la máquina:
    *   `F.Cu` (Pistas y zonas de cobre)
    *   `Edge.Cuts` (Contorno exterior)
    *   `User.1` (Perforaciones manuales, si aplican).

![Ventana de configuración de trazado SVG](img/Imagen 48.png)
*Parámetros de exportación configurados para la CNC Monofab.*

Finalmente, hacemos clic en el botón **Trazar**. Los archivos `.svg` se generarán y guardarán automáticamente dentro de la carpeta raíz de nuestro proyecto.

!!! info "Post-procesamiento Vectorial"
    En caso de que el software de la CNC presente conflictos de lectura con el archivo SVG del contorno (`Edge.Cuts`), podemos importar el archivo generado a **Inkscape** para unificar los vectores, limpiar los trazos y volver a exportarlo asegurando la compatibilidad absoluta con la máquina.

    ### Resultados de Verificación y Exportación

Una vez ejecutado el DRC y validadas las conexiones, el sistema nos muestra el estado de la placa. 

![Resultados del DRC](img/Imagen 50.png)
*Ventana del DRC. Nota: Las advertencias de "Cobre aislado" (islas) son normales en este método de fabricación cuando el polígono de relleno no está conectado a GND, y no afectan el desempeño de la máquina CNC.*

Con el diseño validado y los parámetros de exportación configurados, procedemos a generar los archivos definitivos haciendo clic en **Trazar**.

![Botón Trazar](img/Imagen 51.png)
*Ejecución de la herramienta de trazado vectorial.*

### Archivos de Salida (Arte de Manufactura)

Como resultado, KiCad generará un conjunto de archivos `.svg` independientes. Es vital revisar esta carpeta, ya que cada archivo representa una etapa distinta de la manufactura (grabado de pistas, perforaciones mecánicas y el corte del contorno de la placa).

![Archivos SVG exportados](img/Imagen 52.png)
*Listado de archivos vectoriales listos para el maquinado.*

A continuación, se muestra una previsualización del archivo de pistas principales. Este formato monocromático de alto contraste es el estándar requerido, ya que permite al software CAM de la fresadora calcular las rutas de corte (*toolpaths*) de manera impecable para aislar el cobre.

![Previsualización de Pistas SVG](img/Imagen 53.png)
*Vista de alto contraste del arte generado para la capa de cobre frontal.*
