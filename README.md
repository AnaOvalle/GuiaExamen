# Capitulo 2. Tipos de datos y sentencias de alto nivel
## 2.1.1. Modos de direccionamiento del ARM
La arquitectura ARM ofrece varios modos de direccionamiento para acceder a los operandos en las instrucciones. Estos modos de direccionamiento determinan

#### Direccionamiento inmediato: 
El operando fuente es una constante que forma parte de la instrucción. Por ejemplo:

- mov r0, #1: Mueve el valor 1 al registro r0.
- add r2, r3, #4: Suma el valor 4 al registro r3 y guarda el resultado en r2.

#### Direccionamiento inmediato con desplazamiento o rotación: 
Es una variante del direccionamiento inmediato en la cual se permiten operaciones intermedias sobre los registros. Por ejemplo:

- mov r1, r2, LSL #1: Mueve el valor del registro r2 multiplicado por 2 al registro r1.
- mov r1, r3, ASR #3: Mueve el valor del registro r3 dividido por 8 al registro r1.

#### Direccionamiento por registro: 
El operando fuente se obtiene de un registro. Por ejemplo:

- ldr r0, [r1]: Cargue el valor de la dirección almacenada en el registro r1 en el registro r0.
- str r2, [r3]: Guarde el valor del registro r2 en la dirección almacenada en el registro r3.

#### Direccionamiento por registro con desplazamiento: 
Es una variante del direccionamiento por registro en la cual se permite un desplazamiento adicional sobre el registro. Por ejemplo:

- ldr r0, [r1, #4]: Carga el valor de la dirección almacenada en el registro r1 más un desplazamiento de 4 en el registro r0.
- str r2, [r3, -r4]: Guarde el valor del registro r2 en la dirección almacenada en el registro r3 menos el valor del registro r4.

## 2.1.2 Tipos de datos
Los tipos de datos incluyen enteros, caracteres, flotantes y punteros. Los enteros pueden ser de diferentes tamaños, como int, short y long. Los caracteres representan caracteres individuales y se pueden utilizar para almacenar texto. Los flotantes representan números decimales y se pueden utilizar para realizar cálculos precisos. Los punteros son variables que almacenan direcciones de memoria y se utilizan para acceder y manipular datos almacenados en esas direcciones.
## 2.1.3 Instrucciones de salto
Estas instrucciones permiten controlar el flujo del programa y realizar saltos condicionales o incondicionales a diferentes partes del código.

Se menciona que existen dos tipos de instrucciones de salto: by bx. La instrucción b se utiliza para saltar a una etiqueta específica, mientras que la instrucción bx se utiliza para saltar a un registro.

Además, se explican las condiciones que se pueden añadir a las instrucciones de salto condicional. Estas condiciones permiten controlar si se realiza o no el salto dependiendo del estado de las banderas. Esto es útil para ejecutar o no un grupo de instrucciones en función del resultado de una operación.

También se menciona que las instrucciones de salto en la arquitectura ARM tienen un rango de salto de hasta 64 Mb. Sin embargo, si se necesita realizar un salto mayor, se puede utilizar la instrucción ldr pc, =etiqueta.
## 2.1.4. Estructuras de control de alto nivel
Se aborda el tema de las estructuras de control de alto nivel. Se explica cómo se traducen estas estructuras a lenguaje ensamblador. Se mencionan las estructuras de control for y while, que pueden ejecutarse un mínimo de 0 iteraciones. Se muestra la traducción de estas estructuras utilizando instrucciones de salto condicional y se explica que es necesario evaluar la condición del bucle o de la sentencia si antes de la instrucción de salto. También se proporciona la traducción de la estructura si.
## 2.1.5. Compilación a ensamblador
Explica que los compiladores de C, como gcc, realizan este proceso en dos fases: la primera fase convierte el código C en lenguaje ensamblador, y la segunda fase convierte el lenguaje ensamblador en código compilado (código máquina). Se menciona que es posible interrumpir el proceso después de la compilación y examinar el código ensamblador generado a partir del código fuente en C.
# Capítulo 3 Subrutinas y paso de parámetros
## 3.1.1. La pila y las instrucciones ldm y stm
Se refiere a los modos de direccionamiento del ARM en la arquitectura ARM. En este contexto, se explican las instrucciones específicas ldr y str que se utilizan para acceder a la memoria. También se mencionan las variantes ldm, stm y las preprocesadas push. Estas instrucciones permiten cargar y almacenar datos en la memoria, y se utilizan en la programación en ensamblador para acceder a variables y estructuras de datos almacenadas en la memoria del computador.
## 3.1.2. Convención AAPCS
La convención AAPCS (Procedure Call Standard for the ARM Architecture) es un conjunto de reglas que definen cómo se deben pasar los parámetros y devolver los resultados en las funciones en la arquitectura ARM. Algunas de las reglas de la convención AAPCS son:

- Se pueden usar hasta cuatro registros (r0-r3) para pasar parámetros y hasta dos registros (r0 y r1) para devolver el resultado.
- No es obligatorio utilizar todos los registros para pasar parámetros o devolver resultados. Se pueden utilizar solo los registros necesarios.
- Los registros r0-r3 se utilizan en orden ascendente para pasar los primeros cuatro parámetros.
- Si una función devuelve un valor de 64 bits, se utiliza el par de registros r1:r0 para devolver el resultado.
- Si una función no devuelve ningún valor, se utiliza el tipo "void".
- La convención también establece reglas para el manejo de la pila y las instrucciones ldm y stm.
  
En resumen, la convención AAPCS define cómo se deben pasar los parámetros y devolver los resultados en las funciones en la arquitectura ARM, utilizando registros específicos y siguiendo reglas específicas.
## 3.2.1. Funciones en ensamblador llamadas desde C
Se almacena la semilla en la variable estática seed. Podemos cambiar el valor de la semilla en cualquier momento con la función mysrand, y recibir un número pseudoaleatorio de 15 bits con la función myrand. En realidad myrand lo único que hace es aplicar una operación sencilla en la semilla (multiplicación y suma) y extraer 15 bits de esta. 
Empecemos con la función más sencilla, mysrand. Consta de 3 instrucciones. En la primera de ellas apuntamos con r1 a la dirección donde se encuentra la variable seed. En la segunda pasamos el primer y único parámetro de la función, r0, a la posición de memoria apuntada por r1, es decir, a la variable seed. Por último salimos de la función con la conocida instrucción bx lr. No hay más, no tenemos que devolver nada en r0 (la función devuelve el tipo void), ni tenemos que preservar registros, ni crear variables locales.
La otra función es un poco más compleja. Aparte de requerir más cálculos debemos devolver un valor. Aprovechamos que las 3 variables están almacenadas consecutivamente (en realidad las dos últimas son constantes) para no tener que cargar 3 veces la dirección de cada variable en un registro. Lo hacemos la primera vez con ldr r1, =seed, y accedemos a las variables con direccionamiento a registro con desplazamiento ( [r1], [r1, #4] y [r1, #8]). Como no hay parámetros de entrada empleamos los registros r0, r1, r2 y r3 como almacenamiento temporal, hacemos nuestros cálculos, escribimos el resultado en la variable seed y devolvemos el resultado en el registro r0.
## 3.2.2. Funciones en ensamblador llamadas desde ensamblador
En este tema se explica cómo llamar a funciones escritas en ensamblador desde otras funciones en ensamblador. También se menciona la convención AAPCS y cómo aplicarla para crear y llamar a funciones externas. Además, se aborda el tema de las funciones recursivas y cómo implementarlas en ensamblador. También se mencionan las funciones con muchos parámetros de entrada y se explican los pasos detallados de las llamadas a funciones.
## 3.2.3. Funciones recursivas
Se presentan ejemplos de aplicación de funciones recursivas en lenguaje ensamblador. Se muestra cómo implementar una función recursiva para calcular la serie de Fibonacci utilizando la pila y el paso de parámetros por pila. También se discute la importancia de nombrar las variables locales y la longitud de la pila para hacer el código más legible y fácil de mantener. Además, se menciona cómo salvar y restaurar los registros necesarios al llamar y salir de una función recursiva.
## 3.2.4. Funciones con muchos parámetros de entrada
Se exploran diferentes enfoques para manejar la transferencia de múltiples parámetros a una función, incluyendo el uso de registros y la convención AAPCS (Procedure Call Standard for the ARM Architecture). También se presentan ejemplos de cómo llamar a funciones con muchos parámetros desde C y desde ensamblador.
## 3.2.5. Pasos detallados de llamadas a funciones
1. Usando los registros r0-r3 como almacén temporal, el llamador pasa por pila los parámetros quinto, sexto, etc... hasta el último. Cuidado con el orden, especialmente si se emplea un push múltiple. Este paso es opcional y sólo necesario si nuestra función tiene más de 4 parámetros.
2. El llamador escribe los primeros 4 parámetros en r0-r3. Este paso es opcional, ya que nos lo podemos saltar si nuestra función no tiene parámetros.
3. El llamador invoca a la función con bl. Este paso es obligatorio.
4. Ya dentro de la función, lo primero que hace esta es salvaguardar los registros desde r4 que se empleen más adelante como registros temporales. En caso de no necesitar ningún registro temporal nos podemos saltar este paso.
5. Decrementar la pila para hacer hueco a las variables locales. La suma de bytes entre paso de parámetros por pila, salvaguarda y variables locales debe ser múltiplo de 8, rellenar aquí hasta completar. Como este paso es opcional, en caso de no hacerlo aquí el alineamiento se debe hacer en el paso 4.
6. La función realiza las operaciones que necesite para completar su objetivo, accediendo a parámetros y variables locales mediante constantes .equ para aportar mayor claridad al código. Se devuelve el valor resultado en r0 (ó en r1:r0 si es doble palabra).
7. Incrementar la pila para revertir el alojamiento de variables locales.
8. Recuperar con pop la lista de registros salvaguardados.
9. Retornar la función con bx lr volviendo al código llamador, exactamente a la instrucción que hay tras el bl.
10. El llamador equilibra la pila en caso de haber pasado parámetros por ella
# Capítulo 4 E/S a bajo nivel
## 4.1.1. Librerías y Kernel, las dos capas que queremos saltarnos
Una primera capa se encuentra en la librería runtime que acompaña al ejecutable, la cual incluye sólamente el fragmento de código de la función que necesitemos, en este caso en printf. El resto de funciones de la librería (stdio), si no las invocamos no aparecen en el ejecutable. El enlazador se encarga de todo esto, tanto de ubicar las funciones que llamemos desde ensamblador, como de poner la dirección numérica correcta que corresponda en la instrucción bl printf.
Este fragmento de código perteneciente a la primera capa sí que podemos depurarlo mediante gdb. Lo que hace es, a parte del formateo que realiza la propia función, trasladar al sistema operativo una determinada cadena para que éste lo muestre por pantalla. Es una especie de traductor intermedio que nos facilita las cosas.
La segunda capa va desde que hacemos la llamada al sistema (System Call o Syscall) hasta que se produce la transferencia de datos al periférico, retornando desde la llamada al sistema y volviendo a la primera capa, que a su vez retornará el control a la llamada a librería que hicimos en nuestro programa inicialmente.

En esta segunda capa se ejecuta código del kernel, el cual no podemos depurar.
Además el procesador entra en un modo privilegiado, ya que en modo usuario (el que se ejecuta en nuestro programa ensamblador y dentro de la librería) no tenemos privilegios suficientes como para acceder a la zona de memoria que mapea los periféricos.

En general se tiende a usar una lista reducida de posibles llamadas a sistema, y que éstas sean lo más polivalentes posibles. En este caso vemos que no existe una función específica para escribir en pantalla. Lo que hacemos es escribir bytes en un fichero, pero usando un manejador especial conocido como salida estándar, con lo cual todo lo que escribamos a este fichero especial aparecerá por pantalla.
Pero el propósito de este capítulo no es saltarnos una capa para comunicarnos directamente con el sistema operativo. Lo que queremos es saltarnos las dos capas y enviarle órdenes directamente a los periféricos. Para esto tenemos prescindir del sistema operativo, o lo que es lo mismo, hacer nosotros de sistema operativo para realizar las tareas que queramos.
## 4.1.2. Ejecutar código en Bare Metal
Explica que en un entorno Bare Metal, debemos omitir el sistema operativo y acceder directamente al hardware. Este modo de trabajo nos permite realizar tareas como controlar periféricos o incluso construir nuestro propio sistema operativo desde cero.

Para ejecutar código en Bare Metal, el proceso de ensamblaje y vinculación es diferente en comparación con la creación de ejecutables. En un programa Bare Metal, creamos un archivo binario simple sin encabezado, que contiene el código de máquina de nuestro programa. Este archivo binario, llamado kernel.img, se carga en la RAM en la dirección 0x8000. El proceso de ensamblar y vincular programas Bare Metal implica el uso del comando 'as' para ensamblar el código y el comando 'ld' para vincularlo.

En general, el tema proporciona una descripción general de cómo ejecutar código en un entorno Bare Metal y destaca las diferencias en el proceso de ensamblaje y vinculación en comparación con la creación de ejecutables.
## 4.2. Acceso a periféricos
Se mencionan dos periféricos específicos: GPIO (Entrada/Salida de Propósito General) y el temporizador del sistema. Se proporcionan ejemplos de programas Bare Metal que utilizan estos periféricos, como un LED parpadeante con un bucle de retardo y un LED parpadeante con un temporizador. También se menciona un ejemplo de sonido utilizando el temporizador. Se presentan ejercicios relacionados con la cadencia variable utilizando un bucle de retardo o un temporizador, así como un ejercicio relacionado con la escala musical.
## 4.2.1. GPIO (General-Purpose Input/Output)
El GPIO es un conjunto de señales mediante las cuales la CPU se comunica condistintas partes de la Rasberry tanto internamente (audio analógico, tarjeta SD o LEDs internos) como externamente a través de los conectores P1 y P5. Como la mayor parte de las señales se encuentran en el conector P1 (ver figura 4.3), normalmente este conector se denomina GPIO. Nosotros no vamos a trabajar con señales GPIO que no pertenezcan a dicho conector, por lo que no habrá confusiones.
El GPIO contiene en total 54 señales, de las cuales 17 están disponibles a través del conector GPIO (26 en los modelos A+/B+). Como nuestra placa auxiliar emplea la fila inferior del conector, sólo dispondremos de 9 señales.
##### GPFSELn
GPFSELn es un puerto en el GPIO (General Purpose Input/Output) de la Raspberry Pi que se utiliza para cambiar la funcionalidad de los pines GPIO. Hay varios puertos GPFSELn, donde "n" representa un número del 0 al 5. Cada puerto GPFSELn contiene grupos funcionales llamados FSELx (del 0 al 9) que se utilizan para configurar la función de los pines GPIO. Cada grupo FSELx tiene 3 bits para configurar la función del pin correspondiente. Las posibles configuraciones son: entrada, salida, función alternativa 0, función alternativa 1, función alternativa 2, función alternativa 3, función alternativa 4 y función alternativa 5. En el caso de querer utilizar un pin GPIO como salida genérica, se utiliza la configuración de salida.
##### GPSETn y GPCLRn
GPSETn y GPCLRn son registros utilizados para controlar los pines de entrada/salida de uso general (GPIO) en Raspberry Pi.

- GPSETn es un registro utilizado para configurar los pines GPIO en un estado alto (1), que activa la salida correspondiente o configura el pin como entrada con una resistencia pull-up habilitada.
- GPCLRn es un registro utilizado para borrar los pines GPIO a un estado bajo (0), lo que apaga la salida correspondiente o configura el pin como entrada con una resistencia desplegable habilitada.
Estos registros se utilizan para manipular el estado de los pines GPIO sin la necesidad de realizar operaciones complejas en bits individuales. Proporcionan una forma cómoda de controlar el estado de varios pines simultáneamente.
##### Otros registros
- GPLEVn. Estos puertos devuelven el valor del pin respectivo. Si dicho pin está en torno a 0V devolverá un cero, si está en torno a 3.3V devolverá un 1.
- GPEDSn. Sirven para detectar qué pin ha provocado una interrupción en caso de usarlo como lectura. Al escribir en ellos también podemos notificar que ya hemos procesado la interrupción y que por tanto estamos listos para que nos vuelvan a interrumpir sobre los pines que indiquemos.
- GPRENn. Con estos puertos enmascaramos los pines que queremos que provoquen una interrupción en flanco de subida, esto es cuando hay una transición de 0 a 1 en el pin de entrada.
- GPFENn. Lo mismo que el anterior pero en flanco de bajada.
- GPHENn. Enmascaramos los pines que provocarán una interrupción al detectar un nivel alto (3.3V) por dicho pin.
- GPLENn. Lo mismo que el anterior pero para un nivel bajo (0V).
- GPARENn y GPAFENn. Tienen funciones idénticas a GPRENn y GPFENn, pero permiten detectar flancos en pulsos de poca duración.
- GPPUD y GPPUDCLKn. Conectan resistencias de pull-up y de pull-down sobre los pines que deseemos. Para más información ver el último ejemplo del siguiente capítulo.
