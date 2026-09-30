# Tema 1. Monitor del sistema y mapa mental del ordenador

## Tarea 1 — Reconocer tu monitor del sistema

Abre el monitor del sistema correspondiente a tu sistema operativo:
* **macOS**: Monitor de Actividad (Aplicaciones → Utilidades).
* **Windows**: Administrador de tareas (Ctrl + Shift + Esc).
* **Linux**: System Monitor gráfico o htop en la terminal.

Identifica las columnas que muestran:
* Uso de **CPU** (suele estar en porcentaje).
* Consumo de **RAM** o memoria (en MB o GB).
* Estado o consumo de **disco** (a veces como pestaña aparte).

Anota los **tres procesos** que más CPU consumen en este momento y los **tres que más RAM** consumen. Pueden coincidir o no.

## Tarea 2 — Provocar un pico observable

Vamos a provocar, de forma segura, una situación en la que una pieza concreta se ponga a trabajar. Elige UNA de las opciones:
* **Pico de CPU**: abre el navegador, ve a una página interactiva exigente (un juego HTML, una visualización con muchas animaciones) y déjala correr 30 segundos mientras miras el monitor.
* **Pico de RAM**: abre 20 pestañas distintas en el navegador con páginas pesadas (vídeos en pausa, redes sociales, mapas). Mira cómo sube el consumo de memoria del navegador.
* **Pico de disco**: copia una carpeta grande (varios GB) de un sitio a otro del disco, o duplica un archivo grande. Observa la actividad de disco mientras la copia está en curso.

Describe en 3-5 líneas qué cambió en el monitor durante el experimento (porcentajes antes y después, qué proceso destacaba).

> No hace falta dejar el ordenador al límite durante mucho rato. Con ver el pico ya basta. Si en algún momento el ordenador empieza a ir incómodo, cierra lo que abriste y termina.

## Tarea 3 — Lectura de un mensaje hipotético

Lee este escenario y escribe en 4-6 líneas qué pieza está sufriendo y qué harías a continuación:

> Te escribe un compañero por chat: "Mi ordenador va lentísimo desde hace 10 minutos. Sólo tengo Chrome con 40 pestañas, VS Code abierto y un proyecto de vídeo que no he terminado. El ventilador hace un ruido tremendo y la CPU está al 95 % en el monitor según me dice. El disco está al 70 %. Tengo 8 GB de RAM."

Pista: piensa en cuál de las cuatro piezas (CPU, RAM, disco, SO) tiene el síntoma más claro, y qué dos acciones concretas le sugerirías.

## Entrega final — Glosario del README

Añade al README inicial del repo del módulo (o crea un archivo `glosario.md` si todavía no tienes README) **cuatro entradas**, una por cada pieza estudiada. Cada entrada debe tener:
* **Nombre técnico** en negrita (ej. **CPU**).
* **Una frase** con la definición en tus propias palabras (no copies y pegues de internet).
* **Una analogía cotidiana** que te ayude a recordarla.
* **Un ejemplo** real de algo que has visto hoy en el monitor del sistema relacionado con esa pieza.

Ejemplo de cómo debe quedar una entrada (para que veas el formato):

> **RAM**: la memoria de trabajo del ordenador, donde viven los datos que están usando los programas ahora mismo. Es como la mesa de un cocinero: caben pocas cosas, pero todas a mano. Hoy he visto Chrome consumiendo 1,8 GB de RAM en mi monitor del sistema.

----

# Tema 2. Qué es un progrmaa, un proceso y un archivo ejecutable

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

----

# Tema 3. Tipos de software: aplicación, libería, frmaework, servicio

## Paso 1 — Clasifica las 15 piezas de la lista

A continuación tienes 15 piezas de software muy comunes. Para cada una, asigna **una sola categoría** (aplicación , librería , framework o servicio) y escribe una justificación de **1-2 líneas**. La justificación tiene que mencionar al menos uno de estos tres criterios:

* Quién la usa (humano vs programador vs otra app).
* Quién lleva el control (tú la llamas vs ella te llama).
* Dónde corre (en tu proyecto vs en otra máquina por red).

Lista de piezas a clasificar:

1. Spotity
2. React
3. lodash
4. api.stripe.com
5. Django
6. Visual Studio Code
7. Google Chrome
8. NumPy
9. Express
10. WhatsApp
11. api.openai.com
12. date-fns
13. Next.js
14. Excel
15. api.github.com

## Paso 2 — Caso frontera: React

React se autodenomina "librería de UI" pero la mayoría de los desarrolladores lo trata como framework. Explica en **3-4 líneas** por qué tiene sentido considerarlo como framework cuando piensas en quién dirige el flujo (pista: piensa en cuándo se ejecuta tu componente y quién decide cuándo se renderiza).

## Paso 3 — Dibuja tu propio stack

Elige un escenario familiar de los siguientes (el que más te suene):
* **Opción A — Tu ordenador hoy**: cinco programas que tienes abiertos en este momento.
* **Opción B — Una app que usas a diario**: por ejemplo Instagram, Google Maps o tu app de banca. Imagínate por dentro las piezas que podría tener.
* **Opción C — Un proyecto en el que has tocado código** (si ya has tocado alguno)

Para el escenario elegido, dibuja un esquema sencillo (papel, Excalidraw, Miro, lo que prefieras) con:
* La **aplicación** en el centro.
* Una flecha hacia abajo a las **librerías** y **frameworks** que imaginas o sabes que usa por dentro.
* Una flecha hacia fuera (típicamente hacia un dibujo de "internet") a los **servicios** remotos con los que se comunica.

Tiene que caber en una sola pantalla o folio. El objetivo no es ser exacto sino entrenar la mirada: para cualquier app que uses, ya empiezas a identificar las cuatro capas.

## Paso 4 — Cuatro entradas para el glosario

Redacta cuatro entradas para el glosario del README del proyecto. Cada entrada sigue este formato (3-4 líneas máximo):
```
**Categoría** (Nombre humano): explicación con tus palabras.
Ejemplo real: una pieza de la lista del paso 1 que pertenezca a esta
categoría, con una línea de por qué.
```

Las cuatro entradas son: aplicación, librería, framework y servicio.

----

# Tema 4. Caza de mojibake - Bytes, codificaciones y tildes rotas

## Brief

Vas a provocar tú mismo el clásico **mojibake** (texto roto tipo `maÃ±ana`) en tu ordenador, observar los bytes reales que hay dentro de un archivo de texto y entender exactamente por qué a veces ves `Ã±` donde debería poner `ñ` . El ejercicio no tiene programación: se hace con tu editor de código y un comando de terminal. Termina con cuatro entradas para el glosario del README del proyecto del Roadmap (bit, byte, ASCII, UTF-8).

## Objetivos de aprendizaje

* Comprobar con tus ojos que una `ñ` ocupa 2 bytes en UTF-8 y un emoji ocupa 4.
* Provocar a propósito un mojibake guardando un archivo en una codificación y abriéndolo con otra.
* Leer los bytes reales de un archivo con xxd , hexdump o el equivalente Windows.
* Redactar entradas concisas para el glosario del proyecto.

## Enunciado

### 1. Crea el archivo de prueba

Abre tu editor de código (VS Code, Sublime, Notepad++, lo que uses) y crea un archivo llamado prueba.txt con esta única línea:

```
Hola mañana 😀
```

Asegúrate de que el editor está guardando en **UTF-8**. En VS Code lo ves en la barra inferior; haz clic ahí y selecciona "Save with Encoding → UTF-8" si no lo estaba.

**Entregable del paso 1**: una captura del archivo guardado y la codificación visible en la barra del editor.

### 2. Mira los bytes reales del archivo

Abre una terminal en la carpeta donde guardaste `prueba.txt` y ejecuta el comando adecuado para tu sistema:
```
# macOS o Linux
xxd prueba.txt
```

```
# Windows (PowerShell)
Format-Hex prueba.txt
```

Vas a ver algo parecido a esto (los bytes pueden cambiar según finales de línea y el emoji exacto que uses):
```
00000000: 486f 6c61 206d 61c3 b16e 6120 f09f 9880 Hola ma..na ....
```

Identifica:
* Los bytes que representan la palabra `Hola` (4 bytes, uno por letra ASCII).
* Los **dos** bytes que representan la `ñ` (típicamente `c3 b1`).
* Los **cuatro** bytes que representan el emoji 😀 (típicamente `f0 9f 98 80`).

**Entregable del paso 2**: pega la salida del comando y rodea manualmente (con un comentario, una captura editada o lo que prefieras) los bytes que corresponden a la `ñ` y al emoji.

### 3. Provoca el mojibake en directo

Reabre `prueba.txt` en tu editor **indicando que lo abra como Latin-1** (en VS Code: "Reopen with Encoding → Western (ISO 8859-1)"; en Notepad++: menú "Encoding → Character sets → Western European").

Si todo va bien deberías ver algo como:
```
Hola maÃ±ana ð😀
```

Lo importante es que **el archivo en disco no ha cambiado**: los bytes siguen siendo los mismos. Lo único que cambia es la interpretación.

> 📌 Si tu editor no permite reabrir con otra codificación, abre el mismo archivo con un editor distinto (por ejemplo Notepad++ en Windows o `iconv` en terminal) forzando Latin-1.

**Entregable del paso 3**: captura del archivo abierto con la codificación equivocada, mostrando el mojibake.

### 4. Vuelve a UTF-8 sin perder datos

Cierra el archivo **sin guardar** (esto es importante: si lo guardas mientras estás viendo el mojibake, fijarás los bytes en su forma incorrecta) y reábrelo indicando UTF-8.

Debes volver a ver:
```
Hola mañana 😀
```

**Entregable del paso 4**: captura mostrando que el archivo se ve bien de nuevo, sin haber tenido que modificarlo.

### 5. Cuatro preguntas cortas

Contesta cada pregunta en 1-2 líneas como máximo:
1. ¿Cuántos bytes ocupó la `ñ` en tu archivo? ¿Y el emoji 😀?
2. Cuando viste `Ã±` en el paso 3, ¿el archivo en disco había cambiado? ¿Por qué entonces se veía mal?
3. Si tu compañera te envía un .txt y lo abres en Excel y los acentos aparecen como `Ã©`, ¿qué codificación está asumiendo Excel y qué tendría que cambiar?
4. ¿Por qué la mayoría de proyectos web añaden `<meta charset="utf-8">` en el `<head>`?

### 6. Glosario del proyecto

Redacta cuatro entradas para el glosario del README del Roadmap. Cada entrada tiene este formato (3-4 líneas máximo):

> **Término** — definición con tus palabras + ejemplo concreto del ejercicio (por ejemplo: "la `ñ` ocupó 2 bytes: c3 b1").

Los cuatro términos son: **bit**, **byte**, **ASCII** y **UTF-8**.

---

# Tema 5. Compilar, interprestar y runtime en vivo.

## Brief

Vas a escribir el mismo "Hola, mundo" en tres lenguajes distintos — **C** (compilado), **Python** (interpretado) y **JavaScript** sobre Node.js (interpretado con JIT) — y ejecutarlos en tu portátil para observar con tus propios ojos las diferencias. Vas a ver qué herramienta hace falta tener instalada en cada caso, en qué momento aparecen los errores y por qué un binario compilado se puede correr sin tener el compilador presente. Termina con cinco respuestas cortas que conectan esto con el resto del módulo y entradas para el glosario del README del Roadmap.

## Objetivos de aprendizaje

* Comprobar en vivo la diferencia entre un lenguaje compilado y dos interpretados.
* Identificar qué runtime/compilador necesita cada lenguaje.
* Observar en qué fase (compilación vs. ejecución) aparece un error introducido a propósito.
* Saber consultar la versión instalada del runtime con `--version`.

## Enunciado

> 📌 Algunos pasos requieren tener instalados los compiladores e intérpretes. macOS y la mayoría de distribuciones Linux ya traen `gcc` o `clang` y `python3`. En Windows lo más rápido es instalar (WSL)[https://learn.microsoft.com/windows/wsl/] o usar Git Bash + un compilador como MinGW. Node.js se instala desde (nodejs.org)[https://nodejs.org/] (mejor con nvm/fnm).

### 1. Comprueba qué tienes instalado

Abre una terminal y ejecuta las tres comprobaciones. Cada herramienta devuelve su versión; si te falta alguna, instálala antes de seguir.
```
gcc --version
python3 --version
node --version
```

**Entregable del paso 1**: pega el output de las tres comprobaciones, mostrando que las tres versiones están disponibles.

### 2. Escribe los tres "Hola, mundo"

Crea una carpeta `tres-mundos/` y dentro tres archivos. Copia literalmente el contenido siguiente en cada uno. 

`hola.c` (lenguaje compilado):

```C
#include <stdio.h>
int main() {
 // Imprimimos un saludo desde C
 printf("Hola desde C\n");
 return 0;
}
```

`hola.py` (lenguaje interpretado):

```Python
# Imprimimos un saludo desde Python
print("Hola desde Python")
```

`hola.js` (interpretado por Node.js):

```javascript
// Imprimimos un saludo desde Node.js
console.log("Hola desde Node");
```

**Entregable del paso 2**: una captura del editor mostrando los tres archivos creados.

### 3. Ejecuta cada uno y compara

Ejecuta los tres y observa qué pasos requiere cada uno:
```
# C: paso 1 -> compilar; paso 2 -> ejecutar el binario resultante
gcc hola.c -o hola
./hola
# Python: un solo paso, pasamos el archivo al intérprete
python3 hola.py
# JavaScript: un solo paso, pasamos el archivo al runtime
node hola.js
```

Fíjate en una cosa importante: tras compilar `hola.c`, ha aparecido un archivo nuevo en la carpeta llamado `hola` (sin extensión en macOS/Linux) o `hola.exe` (en Windows). Es el **archivo ejecutable**. Si copias ese binario a otra máquina con el mismo SO, puedes correrlo sin tener gcc instalado. Los `.py` y `.js` no generan ningún archivo nuevo: necesitan siempre su runtime.

**Entregable del paso 3**: pega las salidas de los tres comandos y una línea explicando qué archivo NUEVO ha aparecido en la carpeta tras compilar `hola.c`.

### 4. Provoca un error a propósito en cada uno

Ahora vas a cambiar cada archivo introduciendo un error deliberado. Objetivo: ver **cuándo** salta cada error.

**4.1 Error en C: ejecuta con un printf mal escrito**

Cambia en `hola`.c la línea del `printf` por:
```c
prntf("Hola desde C\n");
```

Vuelve a compilar:
```bash
gcc hola.c -o hola
```

Anota: ¿el error aparece **al compilar o al ejecutar el binario**?

**4.2 Error en Python: ejecuta con función mal escrita**

Cambia en `hola.py`:
```python
prnt("Hola desde Python")
```

Ejecuta:
```cmd
python3 hola.py
```

Anota: ¿el error aparece antes de ejecutar nada, o al ejecutar?

**4.3 Error en JavaScript: ejecuta con función mal escrita**

Cambia en `hola.js`:
```javascript
consol.log("Hola desde Node");
```

Ejecuta:
```cmd
node hola.js
```

Anota: ¿en qué momento sale el error?

**Entregable del paso 4**: para cada lenguaje, pega el mensaje de error literal y di si aparece **antes de ejecutar** (fase de compilación/parseo) o **mientras ejecuta** (runtime).

### 5. Borra el runtime/compilador a ver qué pasa (mental)

No hace falta desinstalar nada: imagina por un momento que en una máquina nueva del equipo NO está instalado ni `gcc`, ni `python3`, ni `node`. Para cada uno de los tres ejemplos, responde:
* ¿Cuáles de los tres archivos podrías ejecutar tal cual?
* ¿Cuál necesitaría que instalaras algo antes?
* ¿En qué cambia la respuesta si en lugar del archivo .c tienes ya el binario hola compilado?

**Entregable del paso 5**: tres líneas con las respuestas.

### 6. Cinco preguntas cortas

Contesta cada una en 1-2 líneas como máximo:
1. ¿Por qué `gcc` aparece como "compilador" y `node` como "runtime", aunque ambos te permiten ejecutar código?
2. ¿Qué necesita una máquina para ejecutar el binario `hola` compilado en C? ¿Y para ejecutar `hola.py`?
3. ¿En qué se parece V8 (el motor JIT de Node.js) a un compilador? ¿En qué se diferencia?
4. ¿Por qué un proyecto Next.js requiere `pnpm build` antes de desplegarse, en lugar de copiar tal cual el código fuente al servidor?
5. Si un error de sintaxis aparece al ejecutar `node app.js` , ¿en qué fase está el problema según la clasificación del submódulo? ¿Y si aparece al lanzar `pnpm build`?

----

# Tema 6. Diseccionando una petición web - Del clic al servidor

## Tarea 1. Resuelve la IP del dominio

Elige un dominio público. Vamos a usar tokioschool.com como ejemplo; puedes cambiarlo por otro si quieres ( `github.com`, `wikipedia.org`).
```
# macOS / Linux
dig tokioschool.com +short
# Windows
nslookup tokioschool.com
```

Apunta la **IP** (o IPs) que recibes. Si te llegan varias, fíjate: los servicios grandes suelen tener varias IPs por dominio para repartir la carga (el llamado _0round robin_ DNS o un CDN).

**Entregable del paso 1**: el comando que ejecutaste y la IP resultante.

## Tarea 2. Mide la latencia con ping

```
# Envía 4 paquetes (en Windows, sin -c; usa -n 4)

ping -c 4 tokioschool.com
```

Observa cuántos milisegundos tarda cada paquete y si alguno se pierde. Una latencia de 10-30 ms es muy buena (servidor cercano); más de 200 ms empieza a ser lento (servidor lejano o saturado).

**Entregable del paso 2**: pega la salida del `ping` y comenta en una línea cuál fue la latencia
media.

## Tarea 3. Traza el recorrido del paquete

```
# macOS / Linux
traceroute tokioschool.com
# Windows
tracert tokioschool.com
```

Cada línea es un **salto** (un router intermedio) que el paquete atraviesa para llegar al destino. Verás varias direcciones hasta llegar al servidor final. Lo normal son entre 8 y 20 saltos.

**Entregable del paso 3**: pega la salida (puedes truncarla si es muy larga) y cuenta cuántos
saltos hubo en total.

## Tarea 4. Mira las cabeceras HTTP con curl

```
curl -I https://tokioschool.com
```

`-I` pide solo las cabeceras (`HEAD`), sin descargar el cuerpo. Vas a ver el código de estado (`HTTP/1.1 200 OK` o similar), el tipo de contenido, información sobre caché y a veces el servidor web que está respondiendo (`Server: nginx`, `Server: Apache`...).

**Entregable del paso 4**: pega la salida y resalta el código de estado y el servidor (si aparece).

## Tarea 5. Disección de una URL

Rellena la tabla siguiente analizando esta URL:

```
https://tokioschool.com:443/cursos/desarrollo-web?nivel=junior&modalidad=online#temario
```

+-------------------+--------------------------------------------------------------+
|	Pieza			|	Valor													   |
+-------------------+--------------------------------------------------------------+
|Esquema 			||
|Host 				||
|Puerto 			||
|Ruta (path)		||
|Query string		||
|Parámetros del query||
|Fragmento			||
+-------------------+--------------------------------------------------------------+

**Entregable del paso 5**: la tabla rellena.

## Tarea 6. Provoca dos errores a propósito

**6.1 Error DNS**

Escribe en tu navegador un dominio con un typo, por ejemplo `https://tokioscholl.com` (sin la o final). Anota el error exacto que sale. En navegadores Chromium suele ser `DNS_PROBE_FINISHED_NXDOMAIN`.

**6.2 Error de conexión rechazada**

Intenta conectarte con `curl` a un puerto en el que no hay nadie escuchando en tu propia máquina:

```
curl http://localhost:9999
```

(Asegúrate de que el puerto 9999 no tenga ningún servidor escuchando; si lo tiene, prueba con 9998, 19999 o cualquier número alto raro.)

Anota el error exacto. Será algo como `Failed to connect to localhost port 9999: Connection
refused`.

**Entregable del paso 6**: el mensaje literal de cada error y una línea indicando en qué eslabón del viaje está el problema (DNS, conexión TCP, ruta, servidor...).

## Tarea 7. Dibuja el diagrama del viaje

En una sola página (Excalidraw, Miro, papel, lo que prefieras), dibuja el viaje completo cuando un usuario teclea `https://tokioschool.com` y le da a Enter. El diagrama tiene que incluir:
* El **navegador** del usuario (cliente).
* La consulta al **DNS** y la IP resultante.
* La **conexión TCP** al servidor (IP + puerto 443).
* La **petición HTTP** con la ruta.
* La **respuesta** del servidor.

Etiqueta cada flecha con lo que está pasando ("pregunta IP", "devuelve IP", "abre conexión", "GET /", "200 OK + HTML"). El objetivo no es elegancia gráfica sino que cada pieza esté nombrada.

**Entregable del paso 7**: captura o foto del diagrama.


















----

# Glosario

* **Aplicación**: Es la parte que el humano interactúa con ella, fruto del desarrollo final de un proyecto y la parte bonita, visual e interactiva de esta hecha para los humanos, con botones, menús, imágenes, etc...
	- Ejemplo real: Spotify, es una aplicación porque ofrece únicamente una interfaz gráfica y es el humano el que interactúa con ella, aunque sea una página web su forma de aplicación (menús y botones interactuables).
* **ASCII**: American Stantardard Code for Information Interchange. Es una forma de hacer que se puedarn representar un conjunto reducido de caracteres ingleses (que no incluyen letas con acentos), número y algunos símbolos (interrogante, eclamación, almohadilla, signos...) y caracteres de control (salto de línea, pitido, vacío). Sirve para que una secuencia de bytes pueda simbolidar caracteres al ser leídos como texto, plano, pues al fin y al cabo todo son 0 y 1's en una computadora. Existe un ascii extendido por país que aprovecha que el 8o bit de la izquierda de convierta en un 1, y eso permite jugar con 127 caractres extra, así pues, podemos por ejemplo asumir que si la letra n es en ascii 01101110b en binario, 6Eh en hexa, pues al poner cambiar el 0 de delante a uno (**1**1101110b o CEh), esto que simbolice la `ñ` (aunque en realidad para ISO 8859-1 la `ñ` es F1h). **En resumen, en ascii cada caracter ocupa un byte siempre**.
* **Bit**: unidad mínima de información detectable en un ordenador, que solo puede tener dos valores: 0 o 1, o apagado y encendido.
* **Build**: Es una herramienta o proceso de traspilación que es una especie de compilador que adapta un código fuente a un destino con un propósito específico, por ejemplo producción. Y organiza el código, paquetiza o minimiza para al final hacerlo lo más óptimo posible para su ejecución. Un ejemplo de transpilador o build es next, que traduce typescript a javascript.
* **Byte**: secuencia de 8 bits que forman un único conjunto inseparable y es la unidad mínima de información que se permite hoy día en los ordenadores, por comidad y convenio, y porque en su forma hexadecimal lo hace muy fácil de representar, con solo dos caracteres del 0 a la F (del 0 al 15). Ej: F0h = 11110000b. Se pueden representar 255 valores posibles.
* **Cliente**: Es una aplicación o el que lanza una petición bajo demanda a un servidor, esperando obtener una respuesta. Literalmente "es el que llama". Puede ser tanto una app de móvil que se conecta a un servidor para obtener respuestas (**Spotify**) como un navegador (en este caso **Chorme**) que accede a una url para obtener una página web y renderizarla. Además hay clientes de consola, como **mysql** o comandos directos como **curl**.
* **Código fuente**: Es el programa completo o fragmento de programa con una sintaxis legible a nivel humano, normalmente en inglés, que agrupa las instrucciones, datos y estructuras que formarán un programa y cada uno tiene un lenguaje diferente que dependiendo de la herramienta que lo ejecute o interprete será un lenguaje u otro, cada uno con características peculiares y diferentes a tener en cuanta para su propósito final y cliente.
* **Compilador**: Herramienta que traduce un lenguaje de alto noviel o código fuente a lenguaje máquina interprestable por la CPU.
* **CPU**: pieza fundamental de un ordenador, es el procesador principal que ejecuta instrucciones en lengaje máquina y tiene varios mecanismos de caché muy pequeños pero extremadamente rápidos. Es como el cocinero en una cocina que ejecuta los platos, un mismo cocinero puede trabajar en paralelo en varios platos hasta cierto límite y cierto número de platos. La forma de medirse es en % de trabajo, donde cada proceso ocupa una parte de % y si la suma de todo llega al 100% de ocupación es que está saturado de trabajo. He visto hoy algún proceso de CPU al 14% que era el administrador de tareas justo en el momento de abrirse.
* **Disco**: la memoria permanente donde residen los datos de usuario y el propio sistema operativo. Esto equivale en una cocina al almacén donde están los productos siempre disponibles y bien almacenados y la temperatura correcta. Actualmente existen de estado sólido y duros puros mecánicos (más lentos, en órdenes de magnitud). Es la dispositivo más lento de los componnentes físicos junto con la CPU y memoria, pero su capacidad es órdenes de magnitud más elevado que la memoria RAM. Se mide en velocidad de acceso lectura o escritura en MB/s. He visto en un momento dado 0.1 MB/s aunque cuando se está copiano un archivo esto crece a miles de MB/s.
* **DNS**: **Domain Name Server**, es un servidor que traduce un nombre de dominio (en su forma `host.ext`) a la **dirección IP** que corresponda, habiendo muchos de una forma escalonada para agilizar esta búsqueda, formando cachés, servidores intermedios y demás para que no todo dependa de uno solo y en caso de cambio de este haya una propagación entre todos los servidores afectados por el cambio en un tiempo relativamente rápido.
* **Framework**: Código que proporciona un punto de partida inicial a un proyecto y una estructura sólida que se utiliza para llamar al código que se genera a posteriori de su implantación y permite un punto de partida mucho más avanzado obviando los detalles de más bajo nivel y que en definitiva sirve para agilizar en mucho tiempo la creación de proyectos respecto como se hacía con código nativo. Es el _framework_ el que llama al código, no al revés, y hay que seguir las normas y criterios establecidos para que la aplicación funcione. Un framework es algo bastante pesados con muchas piezas interconectadas y utiliza normalmente muchas o varias librerías para poder funcionar correctamente.
	- Ejemplo real: **Django**, siendo para Python es un popular framework que está tomando bastante fama para desarrollo web rápido con sus ventajas e inconvenientes respecto a **Flask**. Es un framework porque proporciona la base que llamará a nuestro código creando rutas web, controladores, vistas y modelos.
* **Intérprete**: Herramienta que ejecuta lenguaje de alto nivel o código fuente línea a línea en tiempo de ejecución para los lenguajes que así lo requieren. Son ejemplo de lengauejes interprestados: python, php, visual basic, etc...
* **IP**: **Internet Protocol** es la capa de red por debajo de la física que permite conexiones punto a punto mediante una **dirección IP**. Aunque IP en sí es el protocolo, suele abreviarse como que "una IP" es una dirección IP, porque se usa mucho más corrientemente. Así pues una IP (en nuestro caso del ejemplo era 99.84.9.3) es un punto en internet (o red local) que identifica un host inequívocamente y sería algo así como la dirección física de una casa o bloque de pisos en una localidad dada.
* **Librería**: Pieza o parte de código desarrollado para poderse llamar desde el código fuente que se está desarrollando que proporciona una ayuda a nuestra aplicación para aplicar funcionalidades bien establecidas, evitando así errores y que se pueden reutilizar en otros poryectos. Se llama desde el código fuente del proyecto a demanda, y no viceversa, y se pueden utilizar como y cuando se quiera. Para ello hay que importarlas y copiarlas primero o generarlas mediante herramientas automatizadas de consola de descarga de paquetes como `npm` (node), `composer` (php) o `maven` (java). Suelen estar bien depuradas y ser seguras para los desarrolladores si están en constante proceso de evolución y mejora.
	- Ejemplo real: **lodash**, es una librería porque lo indica el pripio fabricante, y su forma de trabajar es que tenemos que llamarla nosotros explícitamente e invocar a las funciones y métodos que contiene.
* **PID**: Process Identifier. Es un número único y aleatorio que asigna el propio SO a un proceso en ejecución, para identificarlo por un número entero de forma única e inequívoca, no tiene por qué ocupar el mismo PID un proceso que se ejecute una y otra vez.
* **Proceso**: Es el nombre del proceso en sí, que no tiene por qué coincidir con el archivo ejecutable. Y puede tener varias instancias, por ejemplo **Google chrome** si se está ejecutando en multihilo en distintos cores, pero cada hijo tiene su PID distinto.
* **RAM**: la memoria de trabajo del ordenador, donde viven los datos que están usando los programas ahora mismo. Es volátil y gestionada por el SO. Es como la mesa de un cocinero: caben pocas cosas, pero todas a mano. Hoy he visto Chrome consumiendo 1,8 GB de RAM en mi monitor del sistema.
* **Servicio**: Punto en red de llamada que proporciona una interfaz de comunicación de datos entre la aplicación y un servidor, pero no está pensado para que el usuario o humano interactúe con él. Se comunica mediante una **API** que los propios desarrolladores del servicio otorgan a los desarrolladores para que sepan como se utilizan.
	- Ejemplo real: **api.stripe.com**, por convenito, todas las url que empiezan con **api** vienen a denotar que es un servicio web que proporciona un punto de entrada de datos y se usa como **API**, mediante llamadas concretas cerradas, datos enviados y datos devueltos en remoto.
* **Runtime**: Entorno de ejecución que es capaz de interpretar, leer y ejecutar bytecode propios de su lenguaje y ejecutar los programas. Son ejemplo de runtime JVM (Java Virtual Machine) y Node.js.
* **Servidor**: Es un punto de acceso remoto o local que está esperando conexiones por un puesto. Es por tanto "el que escucha". Espera peticiones a través de un puerto TCP/UPD y en caso de conexión exitosa devuelve los datos de respuesta, interpretando la entrada y actuando en consecuencia. A su vez puede ser que el propio servidor tenga que ser cliente de por ejemplo una base de datos para obtener los datos de respuesta, así que actuaría a su vez como cliente (es lo más común). En caso de que los datos esperados no sean válidos puede devolver una salida con el error o en caso de ser servidor web un código de error HTTP y no informar de nada más. En el caso nuestro hemos accedido a `https://www.tokioschool.com`.
* **Sistema operativo**: Es el que maneja los dispositivos a bajo nivel y hace de puente entre el usuario y estos dispositivos. Cualquier llamada a un dispositivo de bajo nivel tiene que pasar por el sistema operativo previamente, no se puede acceder directamente a disco ni memoria RAM sin que el sistema de permiso previo porque es quien controla las zonas de bloqueo de memoria o qué parte del disco está libre u ocupado. Es el equivalente a un chef de cocina que orquestra todos los componentes y personal. No existe un medidor de esta parte, que mencione su estado de ocupación.
* **URL**: Uniform Resourece Identifier. Es la dirección web completa que se pone en la barra del navegador que identifica la web que se va a mostrar. Tiene sus partes, algunas que son opcionales pero suele incluir el formato `<protocolo>://<host y dominio>:<puerto>/<path o destino>?<consulta querystring>#<fragmento>`. Su forma más báscia es `<protocolo>://<host>` donde se asume que si es petición `http` se usa el puerto 80 (en desuso) y si est `https` el 443.
* **UTF-8**. Es una forma de intrepretar o codificar **Unicode**, permite compatibilidad con ASCII puro al 100%, de forma que un texto 100% en ASCII se verá igual en UTF-8 que en ASCII sin necesidad de recodificar. Incluye un formato tan elegante y sencillo que permite no solo que al abrirlo en cualquier otra codificación se intuya todos los caracteres de ASCII normal, sino que además permite cientos de miles de caractres extra incluyeno emoticonos, a costa de requerir algo de espacio extra. En concreto los caracteres de países, acentuados o especiales ocupan un byte extra (2 en total) y los emojis 4 (que no podrían ser representados en ASCII normal ni extendido). En el ejemplo del ejercicio la letra `ñ` ocupaba 2 bytes: `c3 b1`, mientras que el emoji de cara sonriente ocupaba 4.