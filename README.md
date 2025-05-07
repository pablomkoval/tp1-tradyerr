# TP1 Fases de traducción y errores
##1.Preprocesador
*a.```hello2.c```
```
#include <stdio.h>
int/*medio*/main(void){
 int i=42;
 prontf("La respuesta es %d\n");
```
*b. En esta ocasión se usó el siguiente comando:
```gcc -E .\hello2.c -o hello2.i```
El parámetro ```-E``` le indica al compilador detenerse justo despues del preprocesamiento y ```-o``` le indica sobre que archivo escribir el output.
En el archivo ```hello2.i``` se ve que al preprocesar, el compilador elimina los comentarios del código, como ```/*medio*/``` y procesa el ```#include``` (añadiendo cientos de lineas nuevas), sin embargo no hace nada respecto al error de tipeo de ```prontf```.
*c.```hello3.c```
```
int printf(const char * restrict s, ...);
int main(void){
 int i=42;
 prontf("La respuesta es %d\n");
```
*d. ```int printf(const char * restrict s, ...);```
Esta línea llama a la funcion ```printf``` y se añade ```int``` para indicar que va a devolver un valor numérico, ya que esta función devuelve el número de caracteres a imprimir, como primer argumento se le pasa un puntero a una cadena de caracteres que tiene la particularidad de tener el calificador ```restrict```, lo que este hace es hacerle saber al compilador que el espacio de memoria del puntero debe permanecer fijo, es decir, que ninguna otra variable se almacene en ese mismo espacio de memoria. Por último el ```...``` significa que la función puede llegar a tomar varios argumentos.
*e. Repetimos el comando usado anteriormente pero con este nuevo archivo
```gcc -E .\hello3.c -o hello3.i```
Las diferencias que se notan al preprocesar este nuevo archivo es que en ```hello3.i``` se añadieron lineas como ```# 0 ".//hello3.c"```, las cuales ayudan al compilador a hallar el origen del código.
##2.Compilación
*a. Comando para compilar el resultado: ```gcc -S .\hello3.i```. Con esta línea intentamos compilar el archivo ```hello3.i``` para generar el archivo de assembler.
*b. Estos son los errores al intentar generar el archivo ```hello3.s```
```
.\hello3.c: In function 'main':
.\hello3.c:4:2: error: implicit declaration of function 'prontf'; did you mean 'printf'? [-Wimplicit-function-declaration]
    4 |  prontf("La respuesta es %d\n");
      |  ^~~~~~
      |  printf
.\hello3.c:4:2: error: expected declaration or statement at end of input
```
Luego de corregir la } faltante, se repiten los comandos para preprocesar y compilar el nuevo archivo ```hello4.s```
*c. Ensamblador es un lenguaje de programación de bajo nivel, que representa las instrucciones de código de máquina.
El código de ```hello4.s``` es la traducción de ```hello4.c``` pero en assembler, es decir que su función es la de imprimir en terminal una cadena de texto seguida de un numero.
*d. Para ensamblar ```hello4.s``` usamos ```gcc -c hello4.s``` y nos genera un archivo ```hello4.o```
##3.Vinculación
*a.Usamos el comando ```gcc hello4.o -o hello4``` para intentar generar el ejecutable ```hello4.exe``` y no devuelve error alguno.
*b.Ya que no hubo error alguno, ```hello5.exe``` queda igual que ```hello4.exe```
*c.Al ejecutar nos da como salida "La respuesta es " seguido de un numero aleatorio, probablemente residual que haya quedado en memoria ya que no le pasamos en ningun momento como argumento la variable ```i```.
##4.Corrección de bug
*a. Corregimos en un nuevo archivo ```hello6.c``` con el siguiente codigo:
```
int printf(const char * restrict s, ...);
int main(void){
 int i=42;
 printf("La respuesta es %d\n", i);
}
```
La salida es la esperada
##5.Remoción de prototipo
*a.Usamos el código
```
int main(void){
    int i=42;
    printf("La respuesta es %d\n", i);
}
```
*b.El código arroja un error al tratar de compilar ya que la versión del compilador no es muy permisiva y al no haber importado la biblioteca estándar ni declarar la función ```printf``` no puede invocarla, y nos devuelve un error de "declaración implicita" es decir que se llama a la función sin haberla declarado previamente.
