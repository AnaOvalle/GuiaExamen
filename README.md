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
## 
