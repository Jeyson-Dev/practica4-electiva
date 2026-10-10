# Práctica 4 – Integración continua con GitHub Actions

Esta práctica configura una integración continua con GitHub Actions. Cada vez que se sube código a la rama `main`, un workflow ejecuta un programa Hola Mundo en JavaScript y envía una notificación al canal [ntfy.sh/devops-itla](https://ntfy.sh/devops-itla).

## Archivos

| Archivo | Descripción |
| --- | --- |
| `index.js` | Programa Hola Mundo en JavaScript. |
| `.github/workflows/alerta.yml` | Workflow de GitHub Actions que se ejecuta con cada push a `main`. |

## Programa Hola Mundo

```js
console.log("Hola Mundo");
```

Para ejecutarlo en local (requiere [Node.js](https://nodejs.org/)):

```bash
node index.js
```

Salida:

```
Hola Mundo
```

## Workflow `alerta.yml`

El workflow se activa con cada `push` a la rama `main` y ejecuta el job `notificar` en `ubuntu-latest` con estos pasos:

1. **Descargar el código** con `actions/checkout@v4`.
2. **Configurar Node.js 20** con `actions/setup-node@v4`.
3. **Ejecutar Hola Mundo** con `node index.js`.
4. **Enviar la notificación** a `https://ntfy.sh/devops-itla` con `curl`.

La notificación incluye:

- **Título:** `Push a main en <usuario>/<repositorio>`
- **Mensaje:** quién subió el código, los primeros 7 caracteres del commit y la primera línea del mensaje del commit.
- **Etiqueta:** 🚀 (`rocket`)

Ejemplo:

> **Push a main en Jeyson-Dev/practica4-electiva**
> Jeyson-Dev subió código a main. Commit: 85ff7bf - Mostrar solo la primera línea del commit en la alerta

Los datos del commit se pasan al paso de `curl` mediante variables de entorno, en lugar de escribirse directamente en el comando. Así, un mensaje de commit con comillas u otros caracteres especiales no rompe el comando.

## Cómo probarlo

1. Haz un cambio y súbelo a `main`:

   ```bash
   git add .
   git commit -m "Mi cambio"
   git push origin main
   ```

2. Revisa la ejecución en la pestaña **Actions** del repositorio.
3. Abre [ntfy.sh/devops-itla](https://ntfy.sh/devops-itla) (o suscríbete al tema `devops-itla` en la app de ntfy) para ver la notificación.
