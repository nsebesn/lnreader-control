# Freno remoto de fuentes de LNReader

La app LNReader lee `control.json` cada 12 horas (preguntando antes si cambió).
Sirve para frenar una fuente para todos los usuarios a la vez, sin sacar una
actualización de la app: por ejemplo, si una web se queja del tráfico o empieza
a bloquear.

**Sólo puede frenar.** No activa nada, no ejecuta código y no lee datos de nadie.
La app aplica sus propios límites aunque el archivo diga otra cosa.

## Formato

```json
{
  "version": 1,
  "sources": {
    "nova": { "pause": true, "message": "NOVA está en pausa unos días a petición de la web." },
    "ranobes": { "minGapSeconds": 5, "novedades": false }
  }
}
```

Cada clave de `sources` es el identificador de la extensión. Campos, todos opcionales:

| Campo | Qué hace | Límite en la app |
|---|---|---|
| `pause` | `true`: la fuente no se usa y se enseña `message` | — |
| `message` | Texto que ve el usuario | 200 caracteres |
| `minGapSeconds` | Espera mínima entre peticiones de fondo (Novedades, descargas) | 0 a 120 s |
| `novedades` | `false`: Novedades no revisa la fuente | — |

- Para quitar un freno, se borra la fuente de `sources` (o se deja `"sources": {}`).
- Si el archivo no se puede leer o está mal escrito, la app sigue con lo último
  que leyó bien; si nunca leyó nada, no hay restricciones.
- Los cambios llegan a los usuarios en unas 12 horas.

## Cuidado

Quien pueda editar este repositorio puede frenar fuentes para todos. La cuenta
debe tener la verificación en dos pasos activada.
