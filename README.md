# Capitulo 2. Tipos de datos y sentencias de alto nivel
## 2.1.1. Modos de direccionamiento del ARM
La arquitectura ARM ofrece varios modos de direccionamiento para acceder a los operandos en las instrucciones. Estos modos de direccionamiento determinan

#### Direccionamiento inmediato: 
El operando fuente es una constante que forma parte de la instrucción. Por ejemplo:

*mov r0, #1: Mueve el valor 1 al registro r0.
*add r2, r3, #4: Suma el valor 4 al registro r3 y guarda el resultado en r2.

#### Direccionamiento inmediato con desplazamiento o rotación: 
Es una variante del direccionamiento inmediato en la cual se permiten operaciones intermedias sobre los registros. Por ejemplo:

*mov r1, r2, LSL #1: Mueve el valor del registro r2 multiplicado por 2 al registro r1.
*mov r1, r3, ASR #3: Mueve el valor del registro r3 dividido por 8 al registro r1.

#### Direccionamiento por registro: 
El operando fuente se obtiene de un registro. Por ejemplo:

*ldr r0, [r1]: Cargue el valor de la dirección almacenada en el registro r1 en el registro r0.
*str r2, [r3]: Guarde el valor del registro r2 en la dirección almacenada en el registro r3.

#### Direccionamiento por registro con desplazamiento: 
Es una variante del direccionamiento por registro en la cual se permite un desplazamiento adicional sobre el registro. Por ejemplo:

*ldr r0, [r1, #4]: Carga el valor de la dirección almacenada en el registro r1 más un desplazamiento de 4 en el registro r0.
*str r2, [r3, -r4]: Guarde el valor del registro r2 en la dirección almacenada en el registro r3 menos el valor del registro r4.

