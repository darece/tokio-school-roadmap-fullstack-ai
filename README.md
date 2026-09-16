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

---

# Glosario

* **Aplicación**: Es la parte que el humano interactúa con ella, fruto del desarrollo final de un proyecto y la parte bonita, visual e interactiva de esta hecha para los humanos, con botones, menús, imágenes, etc...
	- Ejemplo real: Spotify, es una aplicación porque ofrece únicamente una interfaz gráfica y es el humano el que interactúa con ella, aunque sea una página web su forma de aplicación (menús y botones interactuables).
* **CPU**: pieza fundamental de un ordenador, es el procesador principal que ejecuta instrucciones en lengaje máquina y tiene varios mecanismos de caché muy pequeños pero extremadamente rápidos. Es como el cocinero en una cocina que ejecuta los platos, un mismo cocinero puede trabajar en paralelo en varios platos hasta cierto límite y cierto número de platos. La forma de medirse es en % de trabajo, donde cada proceso ocupa una parte de % y si la suma de todo llega al 100% de ocupación es que está saturado de trabajo. He visto hoy algún proceso de CPU al 14% que era el administrador de tareas justo en el momento de abrirse.
* **Disco**: la memoria permanente donde residen los datos de usuario y el propio sistema operativo. Esto equivale en una cocina al almacén donde están los productos siempre disponibles y bien almacenados y la temperatura correcta. Actualmente existen de estado sólido y duros puros mecánicos (más lentos, en órdenes de magnitud). Es la dispositivo más lento de los componnentes físicos junto con la CPU y memoria, pero su capacidad es órdenes de magnitud más elevado que la memoria RAM. Se mide en velocidad de acceso lectura o escritura en MB/s. He visto en un momento dado 0.1 MB/s aunque cuando se está copiano un archivo esto crece a miles de MB/s.
* **Framework**: Código que proporciona un punto de partida inicial a un proyecto y una estructura sólida que se utiliza para llamar al código que se genera a posteriori de su implantación y permite un punto de partida mucho más avanzado obviando los detalles de más bajo nivel y que en definitiva sirve para agilizar en mucho tiempo la creación de proyectos respecto como se hacía con código nativo. Es el _framework_ el que llama al código, no al revés, y hay que seguir las normas y criterios establecidos para que la aplicación funcione. Un framework es algo bastante pesados con muchas piezas interconectadas y utiliza normalmente muchas o varias librerías para poder funcionar correctamente.
	- Ejemplo real: **Django**, siendo para Python es un popular framework que está tomando bastante fama para desarrollo web rápido con sus ventajas e inconvenientes respecto a **Flask**. Es un framework porque proporciona la base que llamará a nuestro código creando rutas web, controladores, vistas y modelos.
* **Librería**: Pieza o parte de código desarrollado para poderse llamar desde el código fuente que se está desarrollando que proporciona una ayuda a nuestra aplicación para aplicar funcionalidades bien establecidas, evitando así errores y que se pueden reutilizar en otros poryectos. Se llama desde el código fuente del proyecto a demanda, y no viceversa, y se pueden utilizar como y cuando se quiera. Para ello hay que importarlas y copiarlas primero o generarlas mediante herramientas automatizadas de consola de descarga de paquetes como `npm` (node), `composer` (php) o `maven` (java). Suelen estar bien depuradas y ser seguras para los desarrolladores si están en constante proceso de evolución y mejora.
	- Ejemplo real: **lodash**, es una librería porque lo indica el pripio fabricante, y su forma de trabajar es que tenemos que llamarla nosotros explícitamente e invocar a las funciones y métodos que contiene.
* **PID**: Process Identifier. Es un número único y aleatorio que asigna el propio SO a un proceso en ejecución, para identificarlo por un número entero de forma única e inequívoca, no tiene por qué ocupar el mismo PID un proceso que se ejecute una y otra vez.
* **Proceso**: Es el nombre del proceso en sí, que no tiene por qué coincidir con el archivo ejecutable. Y puede tener varias instancias, por ejemplo **Google chrome** si se está ejecutando en multihilo en distintos cores, pero cada hijo tiene su PID distinto.
* **RAM**: la memoria de trabajo del ordenador, donde viven los datos que están usando los programas ahora mismo. Es volátil y gestionada por el SO. Es como la mesa de un cocinero: caben pocas cosas, pero todas a mano. Hoy he visto Chrome consumiendo 1,8 GB de RAM en mi monitor del sistema.
* **Servicio**: Punto en red de llamada que proporciona una interfaz de comunicación de datos entre la aplicación y un servidor, pero no está pensado para que el usuario o humano interactúe con él. Se comunica mediante una **API** que los propios desarrolladores del servicio otorgan a los desarrolladores para que sepan como se utilizan.
	- Ejemplo real: **api.stripe.com**, por convenito, todas las url que empiezan con **api** vienen a denotar que es un servicio web que proporciona un punto de entrada de datos y se usa como **API**, mediante llamadas concretas cerradas, datos enviados y datos devueltos en remoto.
* **Sistema operativo**: Es el que maneja los dispositivos a bajo nivel y hace de puente entre el usuario y estos dispositivos. Cualquier llamada a un dispositivo de bajo nivel tiene que pasar por el sistema operativo previamente, no se puede acceder directamente a disco ni memoria RAM sin que el sistema de permiso previo porque es quien controla las zonas de bloqueo de memoria o qué parte del disco está libre u ocupado. Es el equivalente a un chef de cocina que orquestra todos los componentes y personal. No existe un medidor de esta parte, que mencione su estado de ocupación.