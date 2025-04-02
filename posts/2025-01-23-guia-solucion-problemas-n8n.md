---
title: Guía de Solución de Problemas Comunes en N8N
date: 2025-01-23
author: aitorroma
description: Una guía completa de los errores más frecuentes en N8N y cómo solucionarlos, basada en la experiencia de la comunidad
tags: [guía, soluciones, errores, troubleshooting]
featured_image: https://plus.unsplash.com/premium_photo-1683120972789-f2a14477e6b3\?w\=800\&auto\=format\&fit\=crop
---

# Guía de Solución de Problemas Comunes en N8N

Esta guía recopila los problemas más frecuentes reportados por la comunidad de N8N y sus soluciones verificadas. Cada problema incluye una descripción detallada, su impacto y los pasos específicos para resolverlo.

## 1. Bad Request - Problemas con Parámetros

### Error
`Bad request - please check your parameters`

### Causa
- Problemas con los parámetros de entrada en nodos HTTP Request
- Demasiadas peticiones al servidor en el caso del nodo Email Trigger

### Solución ✅
1. Revisar los parámetros de entrada en el nodo
2. Para Email Trigger: usar el nodo con carpetas y filtros

### Configuración 🔍
- Verificar URL y método en nodos HTTP
- Chequear credenciales y autenticación
- Validar esquema JSON y datos de entrada

## 2. Webhooks de Telegram

### Error
`Bad Request: bad webhook: An HTTPS URL must be provided for webhook`

### Causa
La URL del webhook debe ser HTTPS y accesible desde internet

### Solución ✅
1. Asegurar que N8N sea accesible mediante HTTPS
2. Configurar correctamente el túnel o proxy inverso
3. Usar la URL correcta del webhook de producción

### Configuración 🔍
```yaml
# En docker-compose.yml
command: "n8n start --tunnel"

# Variables de entorno necesarias
N8N_HOST=tu-dominio.com
N8N_PROTOCOL=https
WEBHOOK_URL=https://tu-dominio.com/
```

## 3. Problemas con HTTP Request y Code

### Error
`TypeError: Cannot convert undefined or null to object`

### Causa
Estructura de datos incorrecta o falta de datos en nodos anteriores

### Solución ✅
Usar la notación correcta para acceder a los datos:
```javascript
// Acceder a un campo específico
$node["nombre_nodo"].json["campo"]

// Acceder a todos los items
$node["nombre_nodo"].item

// Obtener primer valor
{{ $node["nombre_nodo"].json["campo"].first() }}
```

## 4. Email Trigger (IMAP)

### Error
`mail_max_userip_connections`

### Causa
Demasiadas conexiones al servidor SMTP

### Solución ✅
1. Usar carpetas y filtros específicos
2. Ajustar configuraciones del servidor de correo

### Configuración 🔍
- Configurar filtros para reducir datos innecesarios
- Revisar loops en el workflow
- Aumentar límites del servidor SMTP si es posible

## 5. Switch y Code

### Error
El nodo Code solo lee la primera ruta del Switch

### Solución ✅
1. Usar nodo Loop antes del Switch
2. Alternativamente, usar nodos IF en lugar de Switch

### Configuración 🔍
Desactivar "Always output data" en el Switch

## 6. Debug con Subdominios

### Error
No se visualiza la ejecución paso a paso

### Solución ✅
1. Acceder usando IP pública y puerto
2. Configurar correctamente el proxy reverso

### Configuración ��
```yaml
# Ejemplo de configuración Traefik v3
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.n8n.rule=Host(`n8n.tudominio.com`)"
  - "traefik.http.services.n8n.loadbalancer.server.port=5678"
```

## 7. Telegram e Imágenes

### Error
No se pueden enviar imágenes locales

### Solución ✅
1. Usar Write Binary File para guardar la imagen
2. Referenciar la imagen en el mensaje de Telegram

### Configuración 🔍
```javascript
// Ejemplo de configuración
{
  "message": "Imagen adjunta",
  "photo": "data/imagen.jpg"
}
```

## 8. Credenciales N8N

### Error
`error:1C800064:Provider routines::bad decrypt`

### Solución ✅
1. Respaldar credenciales antes de actualizaciones
2. Reconfigurar credenciales si es necesario

### Configuración 🔍
```bash
# Respaldar configuración
cp ~/.n8n/config backup/
```

## Consejos Generales

1. **Siempre hacer respaldos** antes de actualizaciones mayores
2. **Usar entornos de prueba** para validar workflows complejos
3. **Mantener logs** de errores para diagnóstico
4. **Verificar versiones** de N8N y nodos utilizados
5. **Documentar configuraciones** especiales

## Recursos Adicionales

- [Documentación oficial de N8N](https://docs.n8n.io/)
- [Foro de la comunidad](https://community.n8n.io/)
- [Canal de Telegram de la Comunidad](https://t.me/aitorroma)

> Esta guía se actualiza regularmente con nuevos problemas y soluciones reportados por la comunidad. Si encuentras un problema no listado aquí, no dudes en compartirlo en nuestro canal de Telegram.
