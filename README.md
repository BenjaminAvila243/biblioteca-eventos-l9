# biblioteca-eventos

| Qué falla | Qué hace el consumidor | Por qué |
|---|---|---|
| JSON roto, falta `x-evento-id` o un campo obligatorio, o Postgres rechaza el dato (códigos `22...` y `23...`) | `nack(mensaje, false, false)` de inmediato | Reintentar no lo arregla nunca |
| El `insert` choca con `23505` | `ack` | Ya estaba guardado: es la reentrega del 2.3 |
| Cualquier otro error de la base (`ECONNREFUSED`, conexión cortada) | Hasta tres intentos, esperando 2 s y después 4 s; si se agotan, `nack(mensaje, false, false)` | El mensaje está bien; el que está mal es el entorno |
