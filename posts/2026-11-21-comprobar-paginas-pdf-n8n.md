---
title: Comprueba en n8n qué páginas lleva un PDF antes de entregarlo
date: 2026-11-21
author: zenoven
description: Valida el número, la identidad y el orden de las páginas de un PDF con cuatro archivos de prueba y un flujo importable de n8n.
tags: [n8n, pdf, automatización, validación]
---

Dos PDF pueden tener el mismo número de páginas y llevar información distinta. Imagina que preparas un anexo de una incidencia y tienes que entregar las páginas 2 y 4 de un informe. Si eliges la 3 por error, o dejas la 4 delante de la 2, contar páginas no detectará el fallo. Con n8n puedes comprobar el recuento y el orden de unos identificadores que ya figuran en el informe.

## Material de prueba

En el [repositorio del blog](https://github.com/aitorroma/comunidad-n8n-blog/tree/main/assets/validar-paginas-pdf) están el flujo `workflow.json` y cinco PDF ficticios. El original, `entrada.pdf`, tiene cuatro páginas y lleva una marca de texto por página, de `QA-001` a `QA-004`. La portada cuenta como primera página, así que el número impreso en las páginas siguientes queda desplazado. Usa el número que ves en el visor al seleccionar páginas.

Para preparar otro archivo de prueba, selecciona la segunda y la cuarta página del original con tu editor de PDF y guárdalas juntas. Una opción es [extraer páginas con PDF Fan](https://pdf.fan/es/extraer-paginas-pdf/), una herramienta en la que trabajo. Coloca el archivo resultante en la carpeta de prueba. La selección es manual. El flujo de n8n empieza cuando el PDF ya está guardado.

## Monta el flujo

Este ejemplo usa n8n 2.36.8 autohospedado. Si lo ejecutas en Docker, monta la carpeta de los archivos en `/fixture`, en modo de solo lectura, y habilita esa ruta con `N8N_RESTRICT_FILE_ACCESS_TO=/fixture`. Si usas otra ruta, cambia el selector del nodo de lectura. El ejemplo funciona sin credenciales y sin enviar archivos a servicios externos desde n8n.

1. **Manual Trigger.** Ejecuta la prueba desde el editor de n8n.
2. **Read/Write Files from Disk.** Llámalo `Leer PDF`. En **Read File(s) From Disk** usa `/fixture/{correcto,incompleto,invertido,otra-pagina}.pdf`. Cada archivo llega como un ítem con el binario `data`. Añade el nombre de tu archivo al selector si quieres comprobarlo en la misma ejecución.
3. **Extract From File.** Elige **Extract From PDF** y usa el campo binario `data`. Para estos archivos, la salida incluye `numpages` y `text`. Ejecuta el nodo una vez para comprobar que aparecen las marcas completas en líneas separadas.
4. **Code.** Elige JavaScript y **Run Once for Each Item**. Usa el siguiente código. El nombre del archivo se recupera del ítem correspondiente en el nodo de lectura.

```javascript
const expected = ['QA-002', 'QA-004'];
const pages = Number($json.numpages);
const text = typeof $json.text === 'string' ? $json.text : '';
const found = text.split(/\r?\n/).map(line => line.trim())
  .filter(line => /^QA-\d{3}$/.test(line));
const countOk = pages === expected.length;
const orderOk = found.length === expected.length
  && found.every((marker, index) => marker === expected[index]);

return { json: {
  file: $('Leer PDF').item.binary.data.fileName,
  status: countOk && orderOk ? 'revisado' : 'revisar',
  pages, expected, found
} };
```

## Comprueba los cuatro resultados

Abrir o importar el flujo deja todo listo para ejecutar las cuatro pruebas. El orden de los ítems puede variar según la lectura del directorio. Comprueba cada resultado por su nombre.

| Archivo | Páginas | Marcas encontradas | Estado |
| --- | ---: | --- | --- |
| `correcto.pdf` | 2 | `QA-002`, `QA-004` | `revisado` |
| `incompleto.pdf` | 1 | `QA-002` | `revisar` |
| `invertido.pdf` | 2 | `QA-004`, `QA-002` | `revisar` |
| `otra-pagina.pdf` | 2 | `QA-002`, `QA-003` | `revisar` |

Para encadenar este control con otro paso, añade un nodo **If** y compara `status` con `revisado`. Deja la rama de `revisar` para inspección manual. Para una comprobación por lotes, decide además cómo tratar un PDF ilegible o protegido con contraseña. El flujo de ejemplo deja que un error de lectura detenga la ejecución.

La comprobación presupone que tu plantilla deja una sola marca de texto por página. Para un PDF escaneado, revisa primero la capa de texto o exige revisión manual. Si las marcas pueden repetirse en una misma página, hace falta extraer y validar cada página por separado. Si el destinatario necesita que compruebes cada palabra, revisa también el contenido. El ejemplo solo coteja las marcas y el número de páginas.

Documentación de n8n: [leer archivos del disco](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.readwritefile/) y [extraer texto de PDF](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.extractfromfile/).
