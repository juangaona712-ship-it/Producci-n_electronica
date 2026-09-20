<div style="display: flex; align-items: center; justify-content: center; gap: 20px; margin-bottom: 30px;">
  <!-- Logo de tu equipo -->
  <img src="img/logo.png" alt="Logo del Equipo" width="100">
  
  <!-- Logo de KiCad actualizado a tu archivo local -->
  <img src="img/KiCad-Logo.svg.webp" alt="Logo KiCad" width="100">
</div>

# Documentación de Diseño de PCB

<!-- BARRA LATERAL FLOTANTE DE ICONOS -->
<div class="sidebar-glass">
  
  <a href="https://discord.com/invite/FANuKv8sZn" target="_blank" title="Discord Oficial de KiCad">
    <img src="https://assets-global.website-files.com/6257adef93867e50d84d30e2/636e0a6a49cf127bf92de1e2_icon_clyde_blurple_RGB.png" width="40" style="border-radius: 8px;">
  </a>
  
  <a href="https://www.kicad.org/blog/" target="_blank" title="Blog de KiCad">
    <img src="img/kicad-icon.png" width="40" style="border-radius: 8px; background: white; padding: 4px;">
  </a>
  
  <a href="https://downloadcenter.rolanddg.com/SRM-20" target="_blank" title="Descargar VPanel SRM-20">
    <img src="img/icono-monofab.jpg" width="40" style="border-radius: 8px;">
  </a>

</div>
<!-- FIN BARRA LATERAL -->

Bienvenido a la bitácora del proyecto de diseño de nuestra placa de circuito impreso (PCB). Este sitio documenta el flujo de trabajo realizado en **KiCad**.

<div align="center" style="margin-top: 20px; margin-bottom: 40px;">
  <a href="https://www.kicad.org/download/" target="_blank" class="btn-descarga">
    📥 Descargar KiCad Oficial
  </a>
</div>

## 🎬 Introducción al Entorno (Curso)

Si apenas estás comenzando, te recomendamos ver el primer episodio del curso **KiCad desde Cero** (por *Easy Learning*). En esta vista previa podrás familiarizarte con el entorno de trabajo antes de replicar nuestra documentación:

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); margin-bottom: 40px; margin-top: 20px;">
  <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" src="https://www.youtube.com/embed/d3H3tfU4zBI" title="KiCad desde Cero - Entorno" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

---

## Fases del Proyecto

<div class="grid cards" markdown>

-   <img src="img/icono-esquematico.jpg" width="35" align="left" style="margin-right: 12px; border-radius: 6px;"> **[1. Esquemático](esquematico.md)**
    
    ---
    
    Documentación del diagrama lógico, selección de componentes y cableado.
    
    [Ver documentación ➔](esquematico.md)

-   <img src="img/icono-placa.png" width="35" align="left" style="margin-right: 12px; border-radius: 6px;"> **[2. Editor de Placas (Layout)](editor-placas.md)**
    
    ---
    
    Proceso de ruteo, distribución de huellas y diseño físico de la PCB.
    
    [Ver documentación ➔](editor-placas.md)

-   ⚙️ **[3. Mods CE](mods-ce.md)**
    
    ---
    
    Modificaciones aplicadas, correcciones y consideraciones técnicas.
    
    [Ver documentación ➔](mods-ce.md)

-   📚 **[4. Recursos](recursos.md)**
    
    ---
    
    Material de referencia, hojas de datos y guías para el manejo de KiCad.
    
    [Ver documentación ➔](recursos.md)

</div>

---

## Nuestro Equipo: KiCad Squad
<p style="text-align: center; color: #888; margin-top: -10px;">Proyecto de Electrónica - IBERO Puebla</p>

<!-- Contenedor del equipo ajustado para 2 personas con descripciones -->
<div style="display: flex; justify-content: center; gap: 40px; text-align: center; margin-top: 30px; flex-wrap: wrap;">
  
  <!-- Carlos -->
  <div style="width: 300px; margin-bottom: 20px; background: rgba(0,0,0,0.2); padding: 20px; border-radius: 12px;">
    <img src="img/carlos.jpg" style="width: 150px; height: 150px; object-fit: cover; border-radius: 50%; border: 4px solid #e53935; box-shadow: 0 4px 15px rgba(229, 57, 53, 0.4); margin: 0 auto;">
    <h3 style="margin-bottom: 5px; margin-top: 15px; font-size: 1.2em;">Carlos Alberto Vázquez Peraza</h3>
    <p style="color: #e53935; font-weight: bold; margin: 0; font-size: 0.95em;">Ingeniero Mecatrónico</p>
    <p style="color: #888; font-size: 0.85em; margin-top: 2px; margin-bottom: 15px;">18 años | Xalapa, Veracruz</p>
    <blockquote style="font-size: 0.9em; color: #bbb; border-left: 3px solid #e53935; text-align: left; padding-left: 15px; margin: 0; font-style: italic;">
      "Me metí a ingeniería mecatrónica porque siempre me ha gustado todo lo relacionado con la electrónica."
    </blockquote>
  </div>

  <!-- Luis -->
  <div style="width: 300px; margin-bottom: 20px; background: rgba(0,0,0,0.2); padding: 20px; border-radius: 12px;">
    <img src="img/luis.jpg" style="width: 150px; height: 150px; object-fit: cover; border-radius: 50%; border: 4px solid #e53935; box-shadow: 0 4px 15px rgba(229, 57, 53, 0.4); margin: 0 auto;">
    <h3 style="margin-bottom: 5px; margin-top: 15px; font-size: 1.2em;">Luis Ernesto Tamez Velásquez</h3>
    <p style="color: #e53935; font-weight: bold; margin: 0; font-size: 0.95em;">Ingeniero Mecatrónico</p>
    <p style="color: #888; font-size: 0.85em; margin-top: 2px; margin-bottom: 15px;">19 años | Tampico, Tamaulipas</p>
    <blockquote style="font-size: 0.9em; color: #bbb; border-left: 3px solid #e53935; text-align: left; padding-left: 15px; margin: 0; font-style: italic;">
      "Vine a esta carrera por una razón muy clara: seré el próximo Tony Stark."
    </blockquote>
  </div>

</div>
