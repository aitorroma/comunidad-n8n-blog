---
title: N elementos a 1
date: 2025-04-02
author: MrCaringi
description: Un snippet para convertir N cantidad de elementos en 1 solo
tags: [snippet, nodo-code]
featured_image: https://raw.githubusercontent.com/MrCaringi/assets/refs/heads/main/images/others/comunidad-n8n/20250402_featured.png
---
Este es un pequeño snippet que te va a ayudar a "consolidar" N cantidad de elementos en uno solo,
Esto puede ser muy util cuando necesitas que indiferentemente de la cantidad de elementos que pueda tener como salida el nodo anterior, quieres que convierta a un único elemento de salida.

## Índice
1. [Creación del Nodo](#1-creacion-del-nodo)

## 1. Creación del Nodo
- Añade el nodo "code"

![nuevo nodo](https://raw.githubusercontent.com/aitorroma/comunidad-n8n-blog/main/assets/2025-04-02_154640.jpg)

![seleccion code](https://raw.githubusercontent.com/aitorroma/comunidad-n8n-blog/main/assets/2025-04-02_155110.jpg)

- Indica el "modo" `Run Once for All Items`
- Añade el código:

![Ingresar el codigo](https://raw.githubusercontent.com/aitorroma/comunidad-n8n-blog/main/assets/2025-04-02_155239.jpg)

### Codigo
- Este es el codigo que debes pegar:

```javascript
return [
  {
    items: items.map(function(item) {
      return item.json;
    })
  }
]
```

- Y con esto ya lo puedes usar!

