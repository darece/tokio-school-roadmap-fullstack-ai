# Brief

Vas a hacer en tu propio ordenador el viaje completo que recorre el submódulo: lanzar un programa que dé lugar a un proceso, identificar ese proceso con sus herramientas (PID, puerto), cerrarlo limpiamente y dejar documentado cada paso. No hace falta servidor remoto, red corporativa ni nada externo: todo se hace en tu máquina con un servidor estático mínimo que ya viene incluido en cualquier instalación moderna.

Al terminar tendrás un mini-informe que puedes pegar tal cual en el README del proyecto del Roadmap como una entrada de glosario reforzada con ejemplo real.

# Objetivos de aprendizaje

* Distinguir en la práctica entre **archivo ejecutable** (lo que invocas) y **proceso** (lo que aparece vivo en el monitor del sistema).
* Identificar un proceso por nombre, por PID y por el puerto que ocupa.
* Cerrar un proceso de forma limpia con `kill` (SIGTERM) y, si no responde, forzarlo con `kill -9` (SIGKILL).
* Leer mensajes como `EADDRINUSE` o "puerto en uso" y traducirlos a la pregunta correcta: "¿qué PID lo está usando?"

# Enunciado

Sigue los pasos en este orden. Cada paso pide una captura o un texto copiado del terminal que vas a juntar al final en un único documento.

> 📌 Si trabajas en Windows, los comandos cambian un poco. La sección "Equivalentes en Windows" al final del enunciado te da las variantes de cada paso.

## 1. Lanzar un proceso a propósito

Abre una terminal y elige UNO de los siguientes lanzadores. Los dos levantan un mini servidor web estático en el puerto 8000. No requieren instalar nada nuevo en macOS o Linux modernos.

```bash
# Opción A: si tienes Python 3 instalado (la mayoría de macOS y Linux)
python3 -m http.server 8000
```

```bash
# Opción B: si prefieres Node.js (instala con `brew install node` si hace falta)
npx --yes http-server -p 8000
```

Verás que la terminal queda "ocupada" mostrando algo como `Serving HTTP on :: port 8000 ...`. **No cierres** esta terminal: abre una segunda terminal para los pasos siguientes.

**Entregable del paso 1**: copia el comando que has usado y la primera línea de salida que ha emitido.

## 2. Confirmar que el proceso está vivo

En la segunda terminal, lista los procesos cuyo nombre contiene python (si elegiste la opción A) o node (si elegiste la opción B).

```bash
# Si lanzaste con Python
pgrep -l python
```

```bash
# Si lanzaste con Node
pgrep -l node
```

Apunta el **PID** que ha aparecido. Va a ser tu identificador del proceso durante todo el ejercicio.

**Entregable del paso 2**: salida del comando con el PID resaltado.

## 3. Descubrir quién usa el puerto 8000
Comprueba qué proceso está escuchando el puerto 8000. Esto te confirma que el PID que has apuntado en el paso 2 es realmente el dueño del puerto (y te entrena en la pregunta clave del clásico Error: `EADDRINUSE`).

```bash
lsof -i :8000
```

La salida debe incluir el nombre del proceso (`python3` o `node`), el PID, el usuario y el estado `LISTEN`. Si ves el PID que apuntaste antes, todo cuadra.

**Entregable del paso 3**: pega la salida de `lsof`.

## 4. Mirar el proceso en el monitor gráfico (opcional pero recomendado)

Abre el monitor del sistema (Monitor de Actividad en macOS, Administrador de tareas en Windows, System Monitor en Linux), busca el proceso por nombre o por PID y mira cuánta CPU y RAM consume mientras está dormido esperando peticiones. Verás un consumo casi nulo: es un proceso vivo, pero en estado sleeping.

**Entregable del paso 4 (opcional)**: captura de pantalla del monitor con tu proceso destacado.

## 5. Cierre limpio con SIGTERM

Pide al proceso que termine de forma educada. Sustituye <PID> por el número que apuntaste en el paso 2.

```bash
kill <PID>
```

Vuelve a la primera terminal (donde lo lanzaste): verás que el servidor se cierra y la terminal queda libre. Repite `pgrep -l python` (o `node`): ya no debe aparecer el proceso.

**Entregable del paso 5**: el comando que ejecutaste y la salida (vacía) del `pgrep` posterior.

## 6. Forzar el cierre con SIGKILL

Vuelve a lanzar el servidor (paso 1). Esta vez, en lugar de cerrarlo educadamente, fuérzalo
con SIGKILL.

```bash
kill -9 <PID>
```

Observa que la terminal del servidor se cierra de manera más abrupta (en algunos casos sin imprimir ningún mensaje de despedida). Esto te deja claro la diferencia: SIGTERM es "por favor termina", SIGKILL es "mueres ya".

**Entregable del paso 6**: comando ejecutado y comentario en una línea sobre la diferencia que has notado respecto al paso 5.

## 7. Diagnóstico del "puerto ocupado"
Lanza el servidor por tercera vez. Sin cerrarlo, abre una tercera terminal y prueba a lanzar OTRO servidor en el mismo puerto 8000:

```bash
python3 -m http.server 8000
```

Vas a recibir un error parecido a:

```bash
OSError: [Errno 48] Address already in use
```

o, con Node:
```bash
Error: listen EADDRINUSE: address already in use :::8000
```

Identifica qué proceso ocupa el puerto con `lsof -i :8000`, mátalo con `kill`, y vuelve a lanzar el servidor: ahora arranca sin problemas.

**Entregable del paso 7**: el error que recibiste, el comando de diagnóstico y la secuencia de cierre que te dejó el puerto libre.

## 8. Mini-glosario reforzado

Con todo lo anterior, redacta en un párrafo de 4-6 líneas tu propia definición de **proceso** y **PID**, citando el ejemplo concreto que acabas de vivir (el servidor web local). Este párrafo va al README del proyecto Roadmap como entrada del glosario.

# Equivalentes en Windows

Si trabajas con Windows, los comandos cambian. Los equivalentes son:

| Operación  | macOS / Linux | Windows (PowerShell) |
+------------+---------------+----------------------+
|Listar procesos por nombre| `pgrep -l python` | `Get-Process python`  |
|Ver puerto en uso | `lsof -i :8000` | `Get-NetTCPConnection -LocalPort 8000` |
|Cerrar proceso por PID | kill <PID> | `Stop-Process -Id <PID>+  |
| Forzar cierre | `kill -9 <PID>` | `Stop-Process -Id <PID> -Force` |

Los pasos 1-8 se mantienen igual; sólo cambia la sintaxis de los comandos.


# glosario

* **PID**: Process Identifier. Es un número único y aleatorio que asigna el propio SO a un proceso en ejecución, para identificarlo por un número entero de forma única e inequívoca, no tiene por qué ocupar el mismo PID un proceso que se ejecute una y otra vez.
* **proceso**: Es el nombre del proceso en sí, que no tiene por qué coincidir con el archivo ejecutable. Y puede tener varias instancias, por ejemplo **Google chrome** si se está ejecutando en multihilo en distintos cores, pero cada hijo tiene su PID distinto.