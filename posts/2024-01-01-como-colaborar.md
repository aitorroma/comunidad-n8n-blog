---
title: Cómo colaborar en el Blog de la Comunidad N8N
date: 2025-04-02
author: aitorroma
description: Guía para contribuir con tus artículos al blog de la comunidad N8N
tags: [guía, colaboración, github]
featured_image: https://raw.githubusercontent.com/aitorroma/comunidad-n8n-blog/main/assets/mimi-thian-vdXMSiX-n6M-unsplash.jpg
---

¡Bienvenido al Blog de la Comunidad N8N! Este espacio está diseñado para que todos podamos compartir nuestros conocimientos, experiencias y tutoriales sobre N8N. Aquí te explico cómo puedes contribuir con tus artículos.

## Estructura del Blog

El blog está alojado en GitHub y utiliza archivos markdown para los posts. Esto significa que puedes escribir tus artículos usando formato markdown, que es muy sencillo de aprender y usar.

## Pasos para Colaborar

1. **Fork del Repositorio**
   ```bash
   # Haz un fork del repositorio en GitHub
   https://github.com/aitorroma/comunidad-n8n-blog
   ```

2. **Clona tu Fork**
   ```bash
   git clone https://github.com/TU-USUARIO/comunidad-n8n-blog.git
   cd comunidad-n8n-blog
   ```

3. **Crea tu Artículo**
   - Los posts se encuentran en la carpeta `posts/`
   - Nombra tu archivo con el formato: `YYYY-MM-DD-titulo-del-post.md`
   - Incluye el frontmatter al inicio del archivo:
   ```yaml
   ---
   title: Título de tu Artículo
   date: 2024-01-01
   author: tu-usuario-github
   description: Breve descripción de tu artículo
   tags: [tag1, tag2, tag3]
   featured_image: https://url-de-tu-imagen.png
   ---
   ```

4. **Añade Imágenes**
   - Coloca tus imágenes en la carpeta `assets/`
   - Usa rutas relativas en tu markdown:
   ```markdown
   ![Descripción](../assets/nombre-imagen.png)
   ```

5. **Envía tu Contribución**
   ```bash
   git add .
   git commit -m "Añadir: Nuevo artículo sobre [tema]"
   git push origin main
   ```

6. **Crea un Pull Request**
   - Ve a GitHub y crea un Pull Request desde tu fork
   - Describe brevemente tu artículo
   - Espera la revisión y aprobación

## Guías de Estilo

### Formato del Contenido

- Usa encabezados de manera jerárquica (h2 para secciones principales, h3 para subsecciones)
- Incluye ejemplos de código cuando sea relevante
- Añade imágenes para ilustrar conceptos complejos
- Mantén un tono profesional pero amigable

### Código

- Usa bloques de código con el lenguaje especificado:
  ````markdown
  ```javascript
  // Tu código aquí
  ```
  ````

### Imágenes

- Optimiza las imágenes antes de subirlas
- Usa nombres descriptivos para los archivos
- Incluye texto alternativo en las imágenes

## Temas Sugeridos

- Tutoriales de configuración
- Casos de uso específicos
- Integraciones interesantes
- Tips y trucos
- Soluciones a problemas comunes
- Mejores prácticas

## Proceso de Revisión

1. El equipo revisará tu PR
2. Podremos sugerir cambios si es necesario
3. Una vez aprobado, tu artículo se publicará en el blog

## Ayuda y Soporte

Si tienes dudas o necesitas ayuda:
1. Abre un issue en el repositorio
2. Únete a nuestra [Comunidad de Telegram](https://t.me/comunidadn8n)
3. Comenta en el PR

¡Esperamos tu contribución!
