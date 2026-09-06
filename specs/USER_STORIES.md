# User Stories — Hermes Landing Studio

## US-1: Registro de organización
**COMO** nuevo usuario
**QUIERO** registrarme con mi correo y crear una organización
**PARA** acceder al panel de landing pages de mi equipo

**Criterios de aceptación:**
- [ ] Formulario de registro con nombre, email, contraseña y nombre de organización
- [ ] Validación de email único (error 409 si ya existe)
- [ ] Validación de contraseña (mín. 8 caracteres, 1 mayúscula, 1 número)
- [ ] Al registrarse se crea automáticamente la organización y se asigna el usuario como admin
- [ ] Redirección al dashboard tras registro exitoso
- [ ] Error "Email ya registrado" si el correo existe

## US-2: Inicio de sesión
**COMO** usuario registrado
**QUIERO** iniciar sesión con mi email y contraseña
**PARA** acceder a mis páginas y paneles

**Criterios de aceptación:**
- [ ] Formulario de login con email y contraseña
- [ ] Validación de credenciales incorrectas (error 401)
- [ ] Token JWT almacenado en localStorage
- [ ] Redirección al dashboard tras login exitoso
- [ ] Cerrar sesión elimina el token y redirige al login
- [ ] Protección de rutas: redirección al login si no hay token

## US-3: Crear landing page desde template
**COMO** usuario de marketing
**QUIERO** ver una galería de templates y seleccionar uno para crear mi página
**PARA** empezar rápidamente con un diseño profesional

**Criterios de aceptación:**
- [ ] Galería de templates con vista previa visual (miniatura)
- [ ] Cada template muestra nombre, categoría y descripción breve
- [ ] Al seleccionar un template se crea una nueva página con sus secciones predefinidas
- [ ] La nueva página aparece en el dashboard con estado "borrador"
- [ ] El contenido de las secciones se copia del template (no es referencia)

## US-4: Editar secciones de página con drag-and-drop
**COMO** usuario de marketing
**QUIERO** reorganizar, añadir y eliminar secciones de mi página arrastrándolas
**PARA** personalizar el flujo y contenido sin tocar código

**Criterios de aceptación:**
- [ ] Panel lateral con lista de secciones disponibles (hero, features, pricing, CTA, testimonios, FAQ, footer)
- [ ] Las secciones se pueden arrastrar para reordenar
- [ ] Botón "Añadir sección" inserta una nueva sección al final
- [ ] Botón "Eliminar sección" con confirmación
- [ ] Los cambios se reflejan inmediatamente en la vista previa
- [ ] Estados: loading (skeleton), empty (sin secciones, "Añade tu primera sección"), error (banner + retry)

## US-5: Editar contenido de sección inline
**COMO** usuario de marketing
**QUIERO** hacer clic en cualquier sección y editar su contenido (título, texto, colores, imágenes)
**PARA** personalizar el mensaje de mi página

**Criterios de aceptación:**
- [ ] Al hacer clic en una sección se abre un panel de edición lateral
- [ ] Cada tipo de sección tiene campos específicos (hero: headline, subtext, CTA text, background image; features: lista de features con icono/título/descripción)
- [ ] Los cambios se ven en tiempo real en la vista previa
- [ ] Botón "Guardar cambios" persiste en backend
- [ ] Indicador de auto-guardado (último guardado: hace X segundos)
- [ ] Validación: campos requeridos marcados

## US-6: Publicar y despublicar landing page
**COMO** usuario de marketing
**QUIERO** publicar mi página para que esté disponible públicamente y despublicarla cuando sea necesario
**PARA** controlar el ciclo de vida de mis campañas

**Criterios de aceptación:**
- [ ] Botón "Publicar" visible solo en páginas en estado borrador
- [ ] Al publicar, la página obtiene una URL pública: `/{slug}`
- [ ] Botón "Despublicar" visible solo en páginas publicadas
- [ ] Confirmación antes de publicar/despublicar
- [ ] Las páginas publicadas muestran badge verde "Publicada", las borrador badge gris "Borrador"
- [ ] La página pública muestra el contenido renderizado sin barra de herramientas

## US-7: Ver dashboard con métricas de página
**COMO** usuario de marketing
**QUIERO** ver un dashboard con el listado de mis páginas, su estado y métricas básicas
**PARA** monitorear el rendimiento de mis campañas

**Criterios de aceptación:**
- [ ] Tabla/listado de páginas con: título, slug, estado, fecha de publicación, visitas totales
- [ ] KPIs en la parte superior: páginas totales, publicadas, visitas totales del mes
- [ ] Botón "Nueva página" que lleva a la galería de templates
- [ ] Cada página tiene acciones: editar, previsualizar, publicar/despublicar, eliminar
- [ ] Filtro por estado (todas, borrador, publicadas)
- [ ] Búsqueda por título
- [ ] Estados: loading (skeleton cards), empty ("Crea tu primera página" + CTA), error (banner + retry)

## US-8: Vista previa responsive
**COMO** usuario de marketing
**QUIERO** previsualizar mi página en distintos tamaños de pantalla (desktop, tablet, móvil)
**PARA** asegurarme de que se ve bien en todos los dispositivos

**Criterios de aceptación:**
- [ ] Botones de toggle: Desktop (1920×1080), Tablet (768×1024), Mobile (375×812)
- [ ] La vista previa se escala dentro de un contenedor con scroll
- [ ] Los cambios en el editor se reflejan en tiempo real en la vista previa
- [ ] El toggle de vista previa no afecta al modo de edición

## US-9: SEO metadata por página
**COMO** usuario de marketing
**QUIERO** editar el título SEO, meta description y OG tags de cada página
**PARA** mejorar el posicionamiento en buscadores y el compartir en redes sociales

**Criterios de aceptación:**
- [ ] Panel de SEO en el editor de página (sección colapsable)
- [ ] Campos: SEO Title (preview en resultados de búsqueda), Meta Description, OG Image URL
- [ ] Contador de caracteres para title (máx. 60) y description (máx. 160)
- [ ] Vista previa de cómo se ve el resultado en Google Search
- [ ] Los meta tags se renderizan en el `<head>` de la página pública
- [ ] Validación: title y description no vacíos si se activa SEO personalizado

## US-10: Analíticas de página
**COMO** usuario de marketing
**QUIERO** ver estadísticas de visitas e interacciones de mis páginas publicadas
**PARA** medir el rendimiento de mis campañas de marketing

**Criterios de aceptación:**
- [ ] Al cargar una página pública, se registra un evento "view" (anónimo, sin auth)
- [ ] Dashboard de página muestra: visitas totales, visitas por día (últimos 7 días), clicks en CTAs
- [ ] Gráfico SVG de visitas diarias (últimos 7/30 días)
- [ ] Los datos se agregan por página y por día
- [ ] Las analíticas son de solo lectura para el usuario
