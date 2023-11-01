# Capitulo 2. Tipos de datos y sentencias de alto nivel
## 2.1.1. Modos de direccionamiento del ARM
En la arquitectura ARM los accesos a memoria se hacen mediante instrucciones
específicas ldr y str. El resto de instrucciones toman operandos desde registros o valores inmediatos, sin excepciones. En este caso la arquitectura nos fuerza a que trabajemos de
un modo determinado: primero cargamos los registros desde memoria, luego procesamos el valor de estos registros con el amplio abanico de instrucciones del ARM,
para finalmente volcar los resultados desde registros a memoria.
Ningún método es mejor que otro, todo es cuestión de diseño. Normalmente se opta por direccionamiento a memoria en instrucciones de procesado en arquitecturas con un número reducido de registros, donde se emplea la memoria como almacén temporal.
#### Direccionamiento inmediato. 
El operando fuente es una constante, formando parte de la instrucción.


