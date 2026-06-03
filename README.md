# Validación de integridad con SHA256

Un hash como **SHA256** funciona como una huella digital del archivo.

Si el archivo cambia, aunque conserve el mismo nombre, el hash también cambia.
Por eso se usa para validar que un archivo descargado no fue modificado, dañado o reemplazado.

En esta prueba se valida un paquete RPM descargado desde GitHub:

```text
otelcol-contrib_0.153.0_linux_amd64.rpm
```

## Comandos

Guardar el SHA256 publicado en GitHub:

```bash
HASH_ORIGINAL="PEGAR_AQUI_EL_SHA256_DE_GITHUB"
```
Validar el archivo descargado:

```bash
echo "${HASH_ORIGINAL} otelcol-contrib_0.153.0_linux_amd64.rpm" | sha256sum -c -
```

Este comando hace lo siguiente:

* `echo`: imprime una línea con el valor de la variable y el nombre del archivo.
* `${HASH_ORIGINAL}`: se reemplaza por el SHA256 oficial copiado desde GitHub.
* `otelcol-contrib_0.153.0_linux_amd64.rpm`: es el archivo que se va a validar.
* `|`: envía esa línea al comando `sha256sum`.
* `sha256sum`: usa el algoritmo SHA256.
* `-c`: check, compara el hash esperado contra el archivo indicado.
* `-`: toma el input recibido desde el pipe.

En resumen, el comando envía a `sha256sum` esta estructura:

```text
SHA256  archivo_a_validar
```

Ejemplo:

```text
9f3021901...  otelcol-contrib_0.153.0_linux_amd64.rpm
```

Otras formas de validar:

```bash
sha256sum otelcol-contrib_0.153.0_linux_amd64.rpm
```

Despues modificaremos el archivo para cambiar el la huella intencionalmente:

```bash
echo "cambio de prueba" >> otelcol-contrib_0.153.0_linux_amd64.rpm
```


Validar nuevamente:

```bash
echo "${HASH_ORIGINAL} otelcol-contrib_0.153.0_linux_amd64.rpm" | sha256sum -c -
```

Como el archivo fue modificado, el hash ya no coincide.

Resultado esperado:

```text
otelcol-contrib_0.153.0_linux_amd64.rpm: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
```

## Conclusión

El archivo conserva el mismo nombre, pero su contenido cambió.

Por eso el SHA256 ya no coincide.

Esto demuestra que el hash sirve para validar la integridad de archivos descargados desde repositorios como GitHub.
