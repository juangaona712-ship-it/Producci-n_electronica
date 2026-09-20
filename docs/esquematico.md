# 1. Diseño del Esquemático y Lógica del Circuito

El diseño del diagrama esquemático representa la primera y más crucial fase en el desarrollo de nuestra placa de circuito impreso (PCB). En esta etapa de abstracción, el objetivo principal no es definir la geometría física ni la ubicación espacial de los componentes, sino establecer de manera rigurosa la interconectividad lógica (Netlist), definir los flujos de corriente, estructurar las topologías de control y garantizar que se cumplan las normativas de diseño eléctrico (ERC). Para llevar a cabo este proceso, emplearemos la suite de automatización de diseño electrónico (EDA) de código abierto, **KiCad**.

---

## Fase 1: Preparación del Entorno y Estandarización de Librerías

El éxito de la manufactura de una PCB depende directamente de la correcta configuración inicial del entorno de trabajo. Es imperativo que el software y las librerías estén sincronizados con las capacidades de fabricación y el inventario de componentes físicos de nuestro laboratorio.

### 1.1 Descarga e Instalación del Software KiCad
El primer paso consiste en obtener la versión más estable de la suite de diseño. Nos dirigimos al portal web oficial de KiCad. En la página de inicio, localizaremos el panel principal que nos ofrece la documentación, las notas de la versión y, en el centro, el botón principal para la descarga del instalador.

![Opciones de descarga KiCad](img/fotos_esquematico/Imagen 1.png)
*Figura 1.1: Interfaz principal del portal oficial de descargas de KiCad.*

Al ingresar a la sección de descargas, el sistema nos requerirá especificar la arquitectura y el sistema operativo de nuestra estación de trabajo. Dado que nuestras computadoras de diseño operan bajo este entorno, seleccionaremos la versión correspondiente para **Windows**.

![Selección de Sistema Operativo](img/fotos_esquematico/Imagen 2.png)
*Figura 1.2: Selección del sistema operativo Windows para garantizar la compatibilidad de los controladores.*

Para optimizar los tiempos de descarga y asegurar la integridad del paquete de instalación, el portal ofrece múltiples servidores espejo (mirrors) distribuidos geográficamente. Seleccionaremos el servidor alojado en **GitHub** ubicado en la región de Norteamérica, lo que nos proporcionará el ejecutable estable. Una vez descargado, procedemos con la instalación dejando los parámetros de variables de entorno por defecto.

![Servidor de descarga GitHub](img/fotos_esquematico/Imagen 3.png)
*Figura 1.3: Selección del servidor espejo en Norteamérica.*

### 1.2 Estructuración y Creación del Proyecto
Al ejecutar KiCad por primera vez, nos recibe el panel de control unificado. Este gestor central administra todos los archivos vinculados a nuestra placa (esquemas, rutados, modelos 3D y archivos de manufactura Gerber). Para iniciar, crearemos un proyecto nuevo seleccionando el primer icono de la barra lateral izquierda o ejecutando el atajo de teclado `Ctrl + N`.

![Panel principal de KiCad](img/fotos_esquematico/Imagen 4.png)
*Figura 1.4: Panel de control unificado y herramienta de creación de nuevos proyectos.*

El software nos solicitará elegir una configuración base. En el selector de plantillas, mantendremos seleccionada la opción **`default`** (Plantilla por defecto de KiCad) y confirmamos haciendo clic en Aceptar. Esta plantilla configura automáticamente las cuadrículas y las reglas de diseño estándar de la industria.

![Plantilla por defecto](img/fotos_esquematico/Imagen 5.png)
*Figura 1.5: Selección de la plantilla de proyecto predeterminada.*

A continuación, debemos asignar un nombre estructurado y definir el directorio de trabajo. Es vital mantener un orden jerárquico en las carpetas. El sistema generará un archivo maestro con la extensión `.kicad_pro`, el cual actuará como el núcleo que enlaza el esquemático lógico con el diseño físico de la placa.

![Guardado del proyecto](img/fotos_esquematico/Imagen 6.png)
*Figura 1.6: Creación del directorio de trabajo y guardado del archivo maestro del proyecto.*

### 1.3 Integración de Librerías de Manufactura (FabLib)
Uno de los errores más comunes en la ingeniería de PCBs es diseñar utilizando componentes genéricos que luego no coinciden físicamente con los adquiridos. Para evitar discrepancias de empaquetado (footprints), instalaremos la librería estándar de fabricación. Navegamos al menú superior y seleccionamos **`Herramientas > Administrador de complementos y contenido`**.

![Menú Herramientas](img/fotos_esquematico/Imagen 7.png)
*Figura 1.7: Ruta de acceso al gestor de paquetes y librerías de KiCad.*

Dentro del administrador, utilizaremos la barra de búsqueda ingresando el término **"fab"**. Localizaremos el paquete denominado **`KiCad FabLib`** (fácilmente identificable por su logotipo de un dinosaurio morado). Procedemos a instalar y actualizar este paquete. Su uso garantiza que las dimensiones de las pistas y los pads coincidan milimétricamente con los estándares requeridos por equipos de ruteo CNC.

![Instalación de FabLib](img/fotos_esquematico/Imagen 8.png)
*Figura 1.8: Localización e instalación de la librería estandarizada FabLib.*

---

## Fase 2: Exploración de la Interfaz del Editor de Esquemas

Con el entorno debidamente configurado y estandarizado, procedemos a abrir el **Editor de Esquemas** dando clic en su respectivo icono en el panel de control principal. Aquí es donde realizaremos la captura lógica de los componentes.

![Abrir Editor de Esquemas](img/fotos_esquematico/Imagen 9.png)
*Figura 2.1: Acceso al entorno de diseño del diagrama lógico.*

### 2.1 Análisis de las Herramientas de Inserción
Dentro del lienzo de diseño, prestaremos especial atención a la barra de herramientas lateral derecha, la cual contiene los accesos rápidos para la inserción de elementos eléctricos:
* 🔴 **Añadir Símbolo:** Herramienta principal (atajo `A`) utilizada para buscar e insertar componentes activos y pasivos (resistencias, capacitores, semiconductores).
* 🔵 **Añadir Símbolo de Alimentación:** Herramienta dedicada (atajo `P`) para establecer los nodos globales de voltaje y planos de tierra (VCC, GND).

![Barra de herramientas](img/fotos_esquematico/Imagen 10.png)
*Figura 2.2: Detalle de la barra de herramientas. Recuadro rojo para símbolos generales y recuadro azul para alimentación.*

Al activar la herramienta de añadir símbolo y buscar un componente (por ejemplo, el diodo emisor de luz `LED_1206`), se despliega una interfaz que nos muestra información crítica. La flecha verde indica el motor de búsqueda, el recuadro rojo muestra la representación lógica según las normativas internacionales, y el recuadro azul nos muestra el **Footprint** (Huella). Es imperativo confirmar que el componente posea una huella asignada antes de dar Aceptar (flecha roja), de lo contrario, no podrá ser ruteado en la PCB.

![Buscador de Símbolos - LED](img/fotos_esquematico/Imagen 11.png)
*Figura 2.3: Interfaz de validación de componentes verificando la asociación lógica-física (Footprint).*

Aplicamos exactamente la misma metodología utilizando la herramienta de símbolos de alimentación. Al buscar el término "GND" (Ground), seleccionamos el nodo que servirá como nuestra referencia de 0 voltios para el retorno de las corrientes.

![Buscador de Alimentación - GND](img/fotos_esquematico/Imagen 12.png)
*Figura 2.4: Búsqueda y selección del nodo de retorno común (Tierra).*

Para los componentes pasivos encargados de limitar la corriente, buscamos el término "res" y seleccionamos la resistencia en formato de montaje superficial (SMD) tamaño 1206 (`R_1206`).

![Buscador de Símbolos - Resistencia](img/fotos_esquematico/Imagen 13.png)
*Figura 2.5: Selección de resistores pasivos para limitación de corriente.*

El objetivo de esta fase de familiarización es prepararnos para construir una topología base de control: un circuito modular diseñado para procesar la señal de un pulsador mecánico y accionar un indicador luminoso (LED). Arquitectónicamente, buscamos alcanzar un diseño análogo al siguiente esquema de referencia:

![Circuito Objetivo](img/fotos_esquematico/Imagen 14.png)
*Figura 2.6: Topología objetivo del circuito de control lógico y potencia.*

---

## Fase 3: Construcción y Ruteo Lógico del Módulo Base

Procederemos a ensamblar nuestro primer módulo funcional. Mantener un orden ortogonal (líneas rectas y componentes alineados) es una convención estricta en el dibujo de esquemas mecatrónicos.

### 3.1 Emplazamiento de Nodos y Componentes
Iniciamos definiendo nuestros planos de retorno. Insertamos dos referencias de tierra globales utilizando la etiqueta estandarizada **`PWR_GND`**.

![Búsqueda de PWR_GND](img/fotos_esquematico/Imagen 15.png)
*Figura 3.1: Selección de la etiqueta de tierra de potencia.*

![Colocación de tierras](img/fotos_esquematico/Imagen 16.png)
*Figura 3.2: Nodos de tierra posicionados en la base del esquema, siguiendo la convención de diseño.*

A continuación, añadimos dos resistores al lienzo. Para facilitar la lectura del flujo de señal, seleccionamos uno de ellos y presionamos la tecla **`R`** para rotarlo 90 grados, dejándolo en posición horizontal. Estos resistores actuarán como limitadores de corriente (Ohm) para proteger a los semiconductores.

![Colocación y rotación de resistencias](img/fotos_esquematico/Imagen 17.png)
*Figura 3.3: Posicionamiento y rotación ortogonal de los resistores pasivos.*

Buscamos en nuestra librería el diodo emisor de luz ingresando el parámetro `led`.

![Búsqueda de LED](img/fotos_esquematico/Imagen 18.png)
*Figura 3.4: Búsqueda del semiconductor optoelectrónico.*

Posicionamos el LED y lo rotamos adecuadamente. Es crítico que el cátodo (la línea recta en el símbolo del diodo) apunte hacia el nodo de tierra (`PWR_GND`) para garantizar la correcta polarización directa del componente y permitir el flujo de electrones.

![LED posicionado](img/fotos_esquematico/Imagen 19.png)
*Figura 3.5: Diodo orientado respetando la polaridad de trabajo.*

Acto seguido, buscamos nuestro actuador de entrada: un botón táctil de montaje superficial (`Switch_Tactile_Omron`). Este componente mecánico cerrará el circuito al ser presionado, permitiendo el paso del voltaje.

![Búsqueda de Switch](img/fotos_esquematico/Imagen 20.png)
*Figura 3.6: Selección del microinterruptor táctil.*

Posicionamos el switch mecánicamente sobre la resistencia vertical. Para proveer de energía al sistema, insertamos un nodo de voltaje positivo. Buscamos la referencia **`PWR_3V3`** (3.3 Voltios) y la rotamos para que apunte hacia arriba, cumpliendo con la norma que dicta que los voltajes positivos siempre deben fluir desde la parte superior del esquema hacia la inferior.

![Switch posicionado](img/fotos_esquematico/Imagen 21.png)
*Figura 3.7: Microinterruptor colocado en línea con la red principal.*

![Búsqueda de PWR_3V3](img/fotos_esquematico/Imagen 22.png)
*Figura 3.8: Selección del nodo lógico de alimentación de 3.3V.*

![Voltaje posicionado](img/fotos_esquematico/Imagen 23.png)
*Figura 3.9: Nodo de potencia emplazado en el extremo superior del módulo.*

### 3.2 Interconexión de Redes Eléctricas (Nets)
Con los componentes emplazados, procedemos a unirlos lógicamente. Al acercar el cursor a los pines de un componente, aparecerá un pequeño círculo. Al hacer clic, iniciaremos el trazado de una red eléctrica (Net).

![Inicio de cableado](img/fotos_esquematico/Imagen 24.png)
*Figura 3.10: Iniciación de un trazo de conexión (Net).*

Este proceso genera una línea verde que representa un trazo de cobre virtual. Arrastramos esta línea hacia el pin del componente de destino para establecer la continuidad eléctrica.

![Arrastre de cable](img/fotos_esquematico/Imagen 25.png)
*Figura 3.11: Arrastre ortogonal de la conexión lógica.*

![Cableado vertical](img/fotos_esquematico/Imagen 26.png)
*Figura 3.12: Cierre del circuito vertical entre el switch y la resistencia.*

Cuando necesitamos bifurcar una señal (por ejemplo, tomar voltaje tanto para la resistencia base como para la sección del LED), trazamos la línea perpendicularmente hacia un cable existente. Al hacer clic sobre el cable verde, KiCad generará automáticamente un nodo (representado por un punto verde más grueso), confirmando que hay una unión eléctrica y no solo un cruce visual de líneas.

![Creación de nodo](img/fotos_esquematico/Imagen 27.png)
*Figura 3.13: Generación de un nodo de derivación para dividir el flujo de corriente.*

Repitiendo este proceso de cableado para todos los elementos, finalizamos la arquitectura de nuestro módulo base de control e indicación.

![Módulo base terminado](img/fotos_esquematico/Imagen 28.png)
*Figura 3.14: Topología del módulo base interconectada y lista para escalabilidad.*

---

## Fase 4: Escalabilidad, Topologías de Control y Verificación (ERC)

Como el diseño general requiere la monitorización de múltiples entradas, aprovecharemos el diseño modular que acabamos de crear. Seleccionamos la totalidad del circuito base, lo copiamos (`Ctrl + C`) y lo pegamos (`Ctrl + V`) hasta obtener **4 módulos independientes**.

![Cuatro módulos replicados](img/fotos_esquematico/Imagen 29.png)
*Figura 4.1: Escalabilidad del diseño mediante la replicación del módulo funcional.*

### 4.1 Alteración de Topologías (Pull-Up y Pull-Down)
En el diseño de sistemas mecatrónicos, es común necesitar diferentes lógicas de disparo (activos en ALTO o activos en BAJO). Para evaluar este comportamiento, modificaremos la arquitectura de módulos específicos:
* En los dos módulos superiores (indicados con **flechas rojas**), intercambiaremos físicamente la posición de la resistencia y el switch. Esto invierte la lógica de lectura respecto a la referencia de tierra.
* En los módulos inferiores (indicados con **flechas azul y verde**) prepararemos el entorno para la inyección de banderas de validación de potencia.

![Indicadores de modificación](img/fotos_esquematico/Imagen 30.png)
*Figura 4.2: Señalización de los módulos a modificar para crear variaciones topológicas.*

### 4.2 Resolución del Control de Reglas Eléctricas (PWR_FLAG)
KiCad incorpora una herramienta de validación matemática llamada ERC (Electrical Rules Checker). Si el ERC detecta componentes consumiendo energía en una red donde no se ha definido explícitamente un componente que *genere* dicha energía (como un regulador o un conector de batería), arrojará errores críticos de red no alimentada.

Para solventar esto a nivel lógico sin alterar el circuito físico, insertamos el símbolo **`PWR_FLAG`** (Bandera de Poder). Esta bandera le comunica al compilador: *"Esta red recibirá energía desde una fuente externa conectada más adelante"*.

![Búsqueda de PWR_FLAG](img/fotos_esquematico/Imagen 31.png)
*Figura 4.3: Inserción de banderas lógicas para validación de potencia.*

El diagrama final, aplicando las inversiones topológicas en los módulos superiores y las banderas de poder en los inferiores, debe lucir estructuralmente como se muestra a continuación, garantizando que el diseño compilará sin errores en el ERC.

![Módulos con PWR_FLAG](img/fotos_esquematico/Imagen 32.png)
*Figura 4.4: Arquitectura final con modificaciones de estado y validación de reglas superada.*

---

## Fase 5: Conectividad Externa y Etiquetas de Red (Net Labels)

Nuestro circuito lógico necesita conectarse con dispositivos externos (como microcontroladores o fuentes de alimentación). Para ello, utilizaremos conectores y organizaremos el esquemático mediante etiquetas de red.

### 5.1 Documentación y Nomenclatura
Para mantener la legibilidad profesional del plano, utilizaremos la herramienta de texto de la barra lateral derecha para agregar descriptores a nuestras zonas de conexión.

![Herramienta de texto](img/fotos_esquematico/Imagen 33.png)
*Figura 5.1: Selección de la herramienta de rotulación de texto.*

El texto en KiCad puede comportarse visualmente como un componente anclado a un nodo.

![Etiqueta conectada](img/fotos_esquematico/Imagen 34.png)
*Figura 5.2: Inserción de rotulación descriptiva en el área de trabajo.*

### 5.2 Inserción de Conectores (Pin Headers)
Procedemos a buscar los terminales físicos de conexión tipo *Through-Hole* (THT). Utilizaremos la familia `PinHeader_01x...`. Requerimos insertar dos de estos componentes:
* Uno de 2 pines (`1x02`) destinado al ingreso del voltaje de alimentación general.
* Uno de 4 pines (`1x04`) destinado a exportar o importar las señales lógicas de nuestros 4 circuitos.

![Búsqueda de Headers](img/fotos_esquematico/Imagen 35.png)
*Figura 5.3: Selección de terminales (Pin Headers) en la librería de componentes.*

![Headers colocados](img/fotos_esquematico/Imagen 36.png)
*Figura 5.4: Conectores J3 y J4 posicionados en el lienzo de diseño.*

Para facilitar la interpretación por parte de terceros, damos doble clic sobre los identificadores azules de los conectores y los renombramos explícitamente como "Entradas" y "Salidas".

![Headers renombrados](img/fotos_esquematico/Imagen 37.png)
*Figura 5.5: Nomenclatura técnica aplicada a los puertos de interconexión.*

### 5.3 Implementación de Etiquetas de Red (Netlabels)
En el diseño avanzado de PCBs, extender cables a lo largo de todo el diagrama cruzando otros componentes es una práctica deficiente que genera diagramas ilegibles (comúnmente llamado "espagueti"). La solución profesional es el uso de **Etiquetas de Red** (Net Labels).

Asignamos etiquetas lógicas específicas a los pines de nuestros conectores (por ejemplo, `Led 1`, `Led 2`, `V 3.3`, `GND`). El motor lógico de KiCad buscará en todo el diagrama y unirá internamente cualquier pin que comparta exactamente el mismo nombre de etiqueta, estableciendo una conexión virtual perfecta sin ensuciar visualmente el plano.

![Net labels en conectores](img/fotos_esquematico/Imagen 38.png)
*Figura 5.6: Asignación de variables de red para establecer conexiones inalámbricas lógicas.*

Con la correcta asignación de los puertos, la integración de módulos de lectura/actuación, y la validación de las reglas eléctricas, damos por concluido de manera exitosa el diseño del diagrama esquemático. La Netlist generada a partir de este documento servirá como el mapa fundacional para la etapa de ruteo físico de la PCB.

![Esquemático Final Completo](img/fotos_esquematico/Imagen 39.png)
*Figura 5.7: Diagrama esquemático mecatrónico finalizado y listo para exportación al entorno PCB.*
