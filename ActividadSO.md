# ACTIVIDAD SISTEMAS OPERATIVOS
## Diego Ignacio Moreno Zapien

**Actividades**  
Eduardo Alcaraz  
November 27, 2024  

---

## Contents

El objetivo de estas actividades es fortalecer tu comprensión de dos áreas fundamentales en sistemas operativos: la administración de memoria y la administración de entrada/salida (E/S). Estos conceptos son esenciales para entender cómo un sistema operativo gestiona los recursos de hardware y software para garantizar el funcionamiento eficiente de un equipo de cómputo.

### ¿Qué se espera de ti?

1. **Lectura y análisis crítico:**
   - Antes de comenzar las actividades, revisa los temas relacionados en tu material de estudio o en recursos confiables. Asegúrate de comprender los conceptos clave, como memoria virtual, paginación, dispositivos de bloque y planificación de discos.
   - Reflexiona sobre cómo estos conceptos son aplicados en sistemas operativos modernos como Linux o Windows.

2. **Desarrollo práctico:**
   - Implementarás programas en lenguajes como C o Python para simular y profundizar en los mecanismos estudiados. Esto incluye la creación de algoritmos de administración de memoria y simuladores de entrada/salida.
   - Al realizar estas tareas, te familiarizarás con técnicas de programación orientadas a sistemas, como el manejo de memoria dinámica, la gestión de interrupciones y la planificación de recursos.

3. **Resolución de problemas:**
   - Las actividades incluyen preguntas teóricas y prácticas diseñadas para retarte a analizar problemas reales y proponer soluciones innovadoras.
   - Se espera que simules entornos de sistemas operativos y explores cómo interactúan sus componentes clave.

## 3. Administración de Memoria

### 3.1 Política y filosofía

#### 1. ¿Cuál es la diferencia entre fragmentación interna y externa? Explica cómo cada una afecta el rendimiento de la memoria.

La **fragmentación interna** ocurre cuando hay un espacio de memoria no utilizado dentro de una partición de memoria asignada a un proceso, debido a que el proceso no necesita toda la memoria de esa partición. Por ejemplo, si se asigna una partición de 100 KB a un proceso que solo necesita 70 KB, los 30 KB restantes son desperdiciados, causando fragmentación interna. Esto reduce la eficiencia de la memoria, ya que la memoria libre no se puede usar para otros procesos.

La **fragmentación externa** ocurre cuando hay suficiente memoria libre total, pero está distribuida en varias partes pequeñas, lo que impide que se asigne un bloque contiguo grande necesario para un proceso. Esto puede llevar a que, aunque la cantidad total de memoria libre sea suficiente, no se pueda usar para nuevos procesos, afectando el rendimiento al limitar la utilización de la memoria.

#### 2. Investiga y explica las políticas de reemplazo de páginas en sistemas operativos. ¿Cuál consideras más eficiente y por qué?

Las políticas de reemplazo de páginas se utilizan cuando la memoria principal se llena y se debe seleccionar una página para ser reemplazada. Algunas de las políticas más comunes son:

- **FIFO (First In, First Out)**: La página que ha estado más tiempo en la memoria se reemplaza primero.
- **LRU (Least Recently Used)**: Se reemplaza la página que ha sido usada menos recientemente.
- **OPT (Optimal)**: Reemplaza la página que no se usará por más tiempo en el futuro.
- **Clock (Reloj)**: Utiliza un puntero circular y reemplaza la página a la que apunta, si está marcada como no usada.

La política **LRU** es considerada más eficiente porque intenta mantener en la memoria las páginas que se han utilizado más recientemente, lo que tiene más probabilidad de ser útil en el futuro. Sin embargo, es más costosa en términos de implementación que FIFO.

### 3.2 Memoria real

#### 1. Programa en C que simule la administración de memoria mediante particiones fijas.

```c
#include <stdio.h>

#define NUM_PARTICIONES 5

void mostrar_particiones(int particiones[], int n) {
    printf("Particiones de memoria:\n");
    for (int i = 0; i < n; i++) {
        printf("Partición %d: %d KB\n", i + 1, particiones[i]);
    }
}

int main() {
    int particiones[NUM_PARTICIONES] = {100, 200, 150, 300, 250}; // Tamaño de cada partición
    int procesos[NUM_PARTICIONES] = {0}; // 0 indica que la partición está libre

    // Mostrar particiones
    mostrar_particiones(particiones, NUM_PARTICIONES);

    // Asignar procesos a las particiones
    for (int i = 0; i < NUM_PARTICIONES; i++) {
        if (procesos[i] == 0) {
            printf("\nAsignando proceso a la partición %d de %d KB\n", i + 1, particiones[i]);
            procesos[i] = 1; // Marca la partición como ocupada
        }
    }

    // Mostrar el estado de las particiones
    printf("\nEstado de las particiones:\n");
    for (int i = 0; i < NUM_PARTICIONES; i++) {
        printf("Partición %d: %s\n", i + 1, procesos[i] ? "Ocupada" : "Libre");
    }

    return 0;
}
```
### 2. Diseña un algoritmo para calcular qué procesos pueden ser asignados a un sistema con memoria real limitada utilizando el algoritmo de "primera cabida".


#### Descripción:
El algoritmo de "Primera Cabida" asigna cada proceso a la primera partición de memoria disponible que sea lo suficientemente grande para contenerlo.

#### Pseudocódigo:
```
Entrada: 
    - Lista de procesos con sus tamaños requeridos (procesos[])
    - Lista de particiones de memoria con sus tamaños disponibles (particiones[])

Salida: 
    - Lista de asignaciones de procesos a particiones.

Algoritmo:
    Para cada proceso en procesos:
        Para cada partición en particiones:
            Si la partición está libre y su tamaño >= tamaño del proceso:
                Asignar el proceso a la partición.
                Reducir el tamaño disponible de la partición.
                Marcar la partición como ocupada si es necesario.
                Romper el bucle interno.
        Si no se encuentra una partición:
            Indicar que el proceso no puede ser asignado.
    Fin Para
```

# 3.3 Organización de memoria virtual

## 1. Concepto de "paginación" y "segmentación"
### Paginación
La paginación es una técnica de administración de memoria que divide la memoria física en bloques de tamaño fijo llamados **marcos** y la memoria lógica de los procesos en bloques del mismo tamaño llamados **páginas**.

#### Ventajas:
- Elimina la fragmentación externa.
- Simplifica la asignación y liberación de memoria.

#### Desventajas:
- Puede generar fragmentación interna si las páginas no están completamente llenas.
- Introduce sobrecarga debido al uso de tablas de páginas.

### Segmentación
La segmentación divide la memoria lógica en segmentos de tamaño variable, cada uno con un propósito específico, como código, datos o pila.

#### Ventajas:
- Permite una estructura más lógica para los programas.
- Reduce la fragmentación interna.

#### Desventajas:
- Puede generar fragmentación externa.
- La administración es más compleja que en la paginación.

---

## 2. Simulación de una tabla de páginas para procesos con acceso aleatorio
```c
#include <stdio.h>
#include <stdlib.h>

#define TAMANO_MEMORIA 16
#define TAMANO_PAGINA 4

typedef struct {
    int pagina;
    int marco;
} EntradaTabla;

void inicializar_tabla(EntradaTabla tabla[], int paginas) {
    for (int i = 0; i < paginas; i++) {
        tabla[i].pagina = i;
        tabla[i].marco = rand() % (TAMANO_MEMORIA / TAMANO_PAGINA);
    }
}

void mostrar_tabla(EntradaTabla tabla[], int paginas) {
    printf("Tabla de páginas:\n");
    printf("Página\tMarco\n");
    for (int i = 0; i < paginas; i++) {
        printf("%d\t%d\n", tabla[i].pagina, tabla[i].marco);
    }
}

int main() {
    int paginas = TAMANO_MEMORIA / TAMANO_PAGINA;
    EntradaTabla tabla[paginas];
    
    inicializar_tabla(tabla, paginas);
    mostrar_tabla(tabla, paginas);
    
    return 0;
}
```
# 3.4 Administración de memoria virtual

## 1. Código que implementa el algoritmo de reemplazo de página "Least Recently Used" (LRU)
```c
#include <stdio.h>

#define MAX_PAGINAS 3

void mostrar_marcos(int marcos[], int n) {
    for (int i = 0; i < n; i++) {
        if (marcos[i] == -1) {
            printf(" _ ");
        } else {
            printf(" %d ", marcos[i]);
        }
    }
    printf("\n");
}

int encontrar_LRU(int tiempos[], int n) {
    int lru = 0;
    for (int i = 1; i < n; i++) {
        if (tiempos[i] < tiempos[lru]) {
            lru = i;
        }
    }
    return lru;
}

void LRU(int paginas[], int num_paginas) {
    int marcos[MAX_PAGINAS] = {-1, -1, -1};
    int tiempos[MAX_PAGINAS] = {0, 0, 0};
    int tiempo = 0;
    int fallos = 0;

    for (int i = 0; i < num_paginas; i++) {
        int pagina_actual = paginas[i];
        int encontrado = 0;

        for (int j = 0; j < MAX_PAGINAS; j++) {
            if (marcos[j] == pagina_actual) {
                tiempos[j] = ++tiempo;
                encontrado = 1;
                break;
            }
        }

        if (!encontrado) {
            int reemplazar = encontrar_LRU(tiempos, MAX_PAGINAS);
            marcos[reemplazar] = pagina_actual;
            tiempos[reemplazar] = ++tiempo;
            fallos++;
        }

        mostrar_marcos(marcos, MAX_PAGINAS);
    }

    printf("Fallos de página: %d\n", fallos);
}

int main() {
    int paginas[] = {7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3};
    int num_paginas = sizeof(paginas) / sizeof(paginas[0]);

    printf("Simulación del algoritmo LRU:\n");
    LRU(paginas, num_paginas);

    return 0;
}
```
## 2. Diagrama del proceso de traducción de direcciones virtuales a físicas

![Diagrama](Diagramaa1.jpg "Diagrama1")

Explicación:
1. La dirección virtual se divide en número de página y desplazamiento.
2. Se busca el marco correspondiente al número de página en la tabla de páginas.
3. Se concatena el marco con el desplazamiento para formar la dirección física.

# Integración

## 1. Análisis de la Gestión de Memoria Virtual en Linux

### Introducción
La memoria virtual es un mecanismo que permite que los programas perciban una cantidad de memoria mayor a la física disponible. Los sistemas operativos modernos, como Linux, administran la memoria virtual utilizando estrategias como paginación y segmentación.

### Gestión de Memoria Virtual en Linux
Linux utiliza un modelo basado en paginación, donde:
- **Páginas:** La memoria se divide en bloques de tamaño fijo llamados páginas (normalmente de 4 KB).
- **Espacio de direcciones:** Cada proceso tiene su propio espacio de direcciones virtuales, independiente del resto.
- **MMU (Unidad de Gestión de Memoria):** Traduce las direcciones virtuales a físicas mediante tablas de páginas.
- **Swapping:** Cuando no hay suficiente memoria física, las páginas inactivas se almacenan temporalmente en el disco (swap).

El sistema gestiona estas operaciones mediante algoritmos de reemplazo de páginas, como LRU (Least Recently Used), para decidir qué páginas mover al área de intercambio.

---

## 2. Simulación: Swapping en Memoria Virtual

A continuación, se presenta un programa en C que simula el intercambio de procesos en memoria virtual. Utiliza estructuras simples para representar procesos y su ubicación en memoria.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TAMANO_MEMORIA 5 // Tamaño de la memoria en "páginas"
#define TAMANO_SWAP 10   // Tamaño del área de intercambio

typedef struct {
    int id;
    char estado[10]; // "MEMORIA" o "SWAP"
} Proceso;

void mostrar(Proceso memoria[], int cant_memoria, Proceso swap[], int cant_swap) {
    printf("\n--- Memoria ---\n");
    for (int i = 0; i < cant_memoria; i++) {
        printf("Proceso %d [%s]\n", memoria[i].id, memoria[i].estado);
    }
    printf("\n--- Swap ---\n");
    for (int i = 0; i < cant_swap; i++) {
        printf("Proceso %d [%s]\n", swap[i].id, swap[i].estado);
    }
}

void intercambiar_proceso(Proceso memoria[], int *cant_memoria, Proceso swap[], int *cant_swap) {
    if (*cant_memoria == 0) {
        printf("No hay procesos en memoria para intercambiar.\n");
        return;
    }

    // Mover el primer proceso de memoria al swap
    Proceso a_intercambiar = memoria[0];
    strcpy(a_intercambiar.estado, "SWAP");
    swap[(*cant_swap)++] = a_intercambiar;

    // Compactar la memoria
    for (int i = 1; i < *cant_memoria; i++) {
        memoria[i - 1] = memoria[i];
    }
    (*cant_memoria)--;

    printf("Proceso %d movido a swap.\n", a_intercambiar.id);
}

void agregar_proceso(Proceso memoria[], int *cant_memoria, int id) {
    if (*cant_memoria >= TAMANO_MEMORIA) {
        printf("La memoria está llena, no se puede agregar el proceso %d.\n", id);
        return;
    }

    memoria[(*cant_memoria)].id = id;
    strcpy(memoria[(*cant_memoria)].estado, "MEMORIA");
    (*cant_memoria)++;
    printf("Proceso %d agregado a memoria.\n", id);
}

int main() {
    Proceso memoria[TAMANO_MEMORIA];
    Proceso swap[TAMANO_SWAP];
    int cant_memoria = 0, cant_swap = 0;

    agregar_proceso(memoria, &cant_memoria, 1);
    agregar_proceso(memoria, &cant_memoria, 2);
    agregar_proceso(memoria, &cant_memoria, 3);
    agregar_proceso(memoria, &cant_memoria, 4);
    agregar_proceso(memoria, &cant_memoria, 5);

    mostrar(memoria, cant_memoria, swap, cant_swap);

    printf("\nIntercambiando proceso...\n");
    intercambiar_proceso(memoria, &cant_memoria, swap, &cant_swap);

    mostrar(memoria, cant_memoria, swap, cant_swap);

    return 0;
}
```
# Administración de Entrada/Salida

## 4.1 Dispositivos y Manejadores de Dispositivos

### 1. Diferencia entre Dispositivos de Bloque y Dispositivos de Carácter

#### Dispositivos de Bloque
Los dispositivos de bloque son aquellos que pueden leer o escribir datos en bloques de tamaño fijo. Estos dispositivos permiten acceso aleatorio a los datos, lo que significa que se puede leer o escribir en cualquier parte del dispositivo sin necesidad de recorrer los datos secuencialmente. Ejemplos de dispositivos de bloque son los discos duros y las unidades de estado sólido (SSD).

**Ejemplo:** Un disco duro (HDD) es un dispositivo de bloque que puede almacenar datos en sectores que se acceden de manera independiente.

#### Dispositivos de Carácter
Los dispositivos de carácter son aquellos que transmiten datos de manera secuencial, es decir, un byte a la vez. No permiten acceso aleatorio a los datos, ya que la lectura o escritura se realiza de forma continua. Estos dispositivos son ideales para flujos de datos continuos como la comunicación por red o la entrada/salida de texto.

**Ejemplo:** Un teclado es un dispositivo de carácter, ya que los datos se envían de forma secuencial, un carácter a la vez.

---

### 2. Programa para un Manejador de Dispositivos de Entrada Virtual

A continuación, se presenta un programa en C que implementa un manejador de dispositivos sencillo para un dispositivo virtual de entrada. Este ejemplo simula la lectura de datos de un dispositivo de entrada y muestra cómo el manejador puede procesar estos datos.

```c
#include <stdio.h>
#include <string.h>

#define TAMANO_BUFFER 256 // Tamaño del buffer de entrada

// Estructura para representar un dispositivo de entrada virtual
typedef struct {
    char buffer[TAMANO_BUFFER];
    int tamano_actual;
} DispositivoEntrada;

// Función para inicializar el dispositivo de entrada
void inicializar_dispositivo(DispositivoEntrada *dispositivo) {
    dispositivo->tamano_actual = 0;
    memset(dispositivo->buffer, 0, TAMANO_BUFFER);
    printf("Dispositivo de entrada inicializado.\n");
}

// Función para leer datos del dispositivo
void leer_datos(DispositivoEntrada *dispositivo) {
    printf("Ingrese datos: ");
    fgets(dispositivo->buffer, TAMANO_BUFFER, stdin);
    // Eliminar el salto de línea si está presente
    if (dispositivo->buffer[strlen(dispositivo->buffer) - 1] == '\n') {
        dispositivo->buffer[strlen(dispositivo->buffer) - 1] = '\0';
    }
    dispositivo->tamano_actual = strlen(dispositivo->buffer);
}

// Función para mostrar los datos leídos
void mostrar_datos(DispositivoEntrada *dispositivo) {
    if (dispositivo->tamano_actual > 0) {
        printf("Datos leídos: %s\n", dispositivo->buffer);
    } else {
        printf("No se han leído datos.\n");
    }
}

int main() {
    DispositivoEntrada dispositivo;

    // Inicializar el dispositivo
    inicializar_dispositivo(&dispositivo);

    // Leer y mostrar datos del dispositivo
    leer_datos(&dispositivo);
    mostrar_datos(&dispositivo);

    return 0;
}
```
# 4.2 Mecanismos y Funciones de los Manejadores de Dispositivos

## 1. Interrupción por E/S y su Administración por el Sistema Operativo

### ¿Qué es la Interrupción por E/S?
La interrupción por E/S (Entrada/Salida) es un mecanismo que permite que los dispositivos de entrada y salida notifiquen al sistema operativo cuando necesitan atención. En lugar de que el sistema operativo tenga que consultar de manera continua el estado de un dispositivo (lo que se conoce como *polling*), las interrupciones permiten que el sistema responda de manera más eficiente y reactiva a los eventos de E/S.

### ¿Cómo la Administra el Sistema Operativo?
El sistema operativo administra las interrupciones mediante un componente llamado *controlador de interrupciones*, que se encarga de gestionar y responder a las interrupciones. Cuando un dispositivo emite una señal de interrupción, el procesador interrumpe su ejecución actual y guarda el estado para atender la solicitud de interrupción. El sistema operativo luego ejecuta un *manejador de interrupciones* específico para procesar la solicitud y, una vez finalizado, retorna a la tarea anterior.

### Ejemplo en Pseudocódigo para Simular Interrupciones
```pseudo
// Pseudocódigo para simular interrupciones de E/S

función manejar_interrupcion() {
    // Este es el manejador de interrupciones
    imprimir("Interrupción recibida: Procesando solicitud de E/S...");
    procesar_solicitud();
    imprimir("Interrupción procesada. Retornando al proceso anterior.");
}

función proceso_principal() {
    mientras (true) {
        imprimir("Proceso principal ejecutándose...");
        
        // Simular una interrupción
        si (condición_interrupcion) {
            manejar_interrupcion();
        }
        
        // Continuar con el proceso principal
    }
}

// Ejecutar el proceso principal
proceso_principal();
```
# Programa de Simulación de Manejo de Interrupciones

Este programa en C simula el manejo de interrupciones en un sistema básico. La simulación incluye un proceso principal que puede ser interrumpido, y un manejador de interrupciones que se activa cuando se detecta una interrupción.

```c
#include <stdio.h>
#include <stdbool.h>

bool interrupcion_detectada = false; // Variable para indicar si hay una interrupción

// Manejador de interrupciones
void manejar_interrupcion() {
    printf("Interrupción recibida: Procesando solicitud de E/S...\n");
    // Simulación del procesamiento de la solicitud de E/S
    printf("Solicitud de E/S procesada.\n");
    // Restablecer el estado de la interrupción
    interrupcion_detectada = false;
}

// Función para simular el proceso principal
void proceso_principal() {
    while (true) {
        printf("Proceso principal ejecutándose...\n");
        
        // Verificar si hay una interrupción
        if (interrupcion_detectada) {
            manejar_interrupcion();
        }

        // Simulación de la condición para detectar una interrupción (para fines de demostración)
        char entrada;
        printf("¿Detectar interrupción? (s/n): ");
        scanf(" %c", &entrada);
        if (entrada == 's') {
            interrupcion_detectada = true;
        }
    }
}

int main() {
    proceso_principal();
    return 0;
}
```
# 4.3 Estructuras de Datos para Manejo de Dispositivos

## 1. ¿Qué es una Cola de E/S?

### Definición de Cola de E/S
Una cola de E/S (Entrada/Salida) es una estructura de datos utilizada en los sistemas operativos para gestionar las solicitudes de dispositivos de entrada y salida. Estas colas permiten organizar y procesar las solicitudes en el orden en que se reciben, o en función de una prioridad específica.

Existen diferentes tipos de colas de E/S, y se utilizan principalmente para optimizar el tiempo de respuesta y la eficiencia del manejo de las solicitudes de dispositivos. Una cola de prioridad, por ejemplo, se usa para garantizar que las solicitudes más críticas sean procesadas antes que las menos importantes.

### Simulación de una Cola con Prioridad
Para simular una cola con prioridad, se puede usar una estructura de datos como una lista ordenada, donde los elementos con mayor prioridad se encuentran al frente de la cola.

**Pseudocódigo de la simulación de una cola con prioridad:**
```pseudo
estructura Solicitud {
    id: entero
    prioridad: entero
}

cola_prioridad = []

función agregar_a_cola(solicitud) {
    insertar(solicitud) en cola_prioridad en orden decreciente de prioridad
}

función procesar_solicitudes() {
    mientras (cola_prioridad no está vacía) {
        solicitud = extraer el primer elemento de la cola
        imprimir("Procesando solicitud con ID: ", solicitud.id)
    }
}
```
## 2. Programa de Simulación de Operaciones de un Manejador de Dispositivos

Este programa en C simula las operaciones de un manejador de dispositivos utilizando una tabla de estructuras para gestionar las solicitudes de los dispositivos.

```c
#include <stdio.h>
#include <string.h>

#define MAX_SOLICITUDES 5

// Estructura que representa una solicitud de dispositivo
typedef struct {
    int id;
    char tipo[20]; // Tipo de solicitud (ej. "lectura", "escritura")
    int prioridad; // Prioridad de la solicitud (1 = alta, 2 = media, 3 = baja)
} Solicitud;

// Tabla de solicitudes
Solicitud tabla_solicitudes[MAX_SOLICITUDES];
int num_solicitudes = 0;

// Función para agregar una solicitud a la tabla
void agregar_solicitud(int id, char *tipo, int prioridad) {
    if (num_solicitudes < MAX_SOLICITUDES) {
        tabla_solicitudes[num_solicitudes].id = id;
        strcpy(tabla_solicitudes[num_solicitudes].tipo, tipo);
        tabla_solicitudes[num_solicitudes].prioridad = prioridad;
        num_solicitudes++;
        printf("Solicitud ID %d agregada.\n", id);
    } else {
        printf("La tabla de solicitudes está llena.\n");
    }
}

// Función para procesar las solicitudes en orden de prioridad
void procesar_solicitudes() {
    // Ordenar las solicitudes por prioridad (menor número = mayor prioridad)
    for (int i = 0; i < num_solicitudes - 1; i++) {
        for (int j = 0; j < num_solicitudes - i - 1; j++) {
            if (tabla_solicitudes[j].prioridad > tabla_solicitudes[j + 1].prioridad) {
                // Intercambiar las solicitudes si la prioridad es mayor
                Solicitud temp = tabla_solicitudes[j];
                tabla_solicitudes[j] = tabla_solicitudes[j + 1];
                tabla_solicitudes[j + 1] = temp;
            }
        }
    }

    // Procesar y mostrar las solicitudes
    for (int i = 0; i < num_solicitudes; i++) {
        printf("Procesando solicitud ID %d, tipo: %s, prioridad: %d\n",
               tabla_solicitudes[i].id, tabla_solicitudes[i].tipo, tabla_solicitudes[i].prioridad);
    }
}

int main() {
    // Agregar algunas solicitudes de ejemplo
    agregar_solicitud(1, "lectura", 2);
    agregar_solicitud(2, "escritura", 1);
    agregar_solicitud(3, "lectura", 3);
    agregar_solicitud(4, "escritura", 1);

    // Procesar las solicitudes en orden de prioridad
    procesar_solicitudes();

    return 0;
}
```
# 4.4 Operaciones de Entrada/Salida

## 1. Flujo para la Lectura de un Archivo desde un Disco Magnético

### Descripción del Flujo
El proceso de lectura de un archivo desde un disco magnético involucra varios pasos para asegurar que los datos se transfieran de manera eficiente desde el dispositivo de almacenamiento a la memoria del sistema. A continuación, se describe el flujo de este proceso:

1. **Solicitud de Lectura**: El sistema operativo recibe una solicitud de lectura de un archivo.
2. **Localización del Archivo**: El sistema busca la ubicación del archivo en el disco.
3. **Acceso al Disco**: El disco magnético se accede para localizar la posición de los datos.
4. **Transferencia de Datos**: Los datos se leen y se transfieren a la memoria del sistema.
5. **Confirmación de Lectura**: El sistema operativo confirma que la operación de lectura se ha completado y los datos están disponibles para el proceso solicitante.

### Programa de Simulación del Proceso de Lectura

A continuación, se presenta un programa en C que simula el proceso de lectura de un archivo desde un disco magnético de manera básica:

```c
#include <stdio.h>
#include <stdlib.h>

// Simulación de la función de lectura de un archivo
void leer_archivo(const char *nombre_archivo) {
    FILE *archivo = fopen(nombre_archivo, "r"); // Abre el archivo en modo lectura

    if (archivo == NULL) {
        printf("Error: No se pudo abrir el archivo.\n");
        return;
    }

    char buffer[256]; // Buffer para almacenar los datos leídos
    printf("Lectura del archivo %s:\n", nombre_archivo);

    // Lee el contenido del archivo y lo imprime en consola
    while (fgets(buffer, sizeof(buffer), archivo) != NULL) {
        printf("%s", buffer);
    }

    fclose(archivo); // Cierra el archivo después de la lectura
    printf("\nLectura completada.\n");
}

int main() {
    const char *nombre_archivo = "ejemplo.txt"; // Nombre del archivo a leer
    leer_archivo(nombre_archivo);

    return 0;
}
```
## 2. Programa de Operaciones de Entrada/Salida Asíncronas usando Archivos

A continuación, se presenta un programa en Python que simula operaciones de entrada/salida asíncronas utilizando archivos.

```python
import asyncio

# Función asíncrona para leer un archivo
async def leer_archivo(nombre_archivo):
    print(f"Iniciando la lectura del archivo {nombre_archivo}...")
    with open(nombre_archivo, 'r') as archivo:
        contenido = archivo.read()
    print(f"Lectura del archivo {nombre_archivo} completada.")
    return contenido

# Función asíncrona para escribir en un archivo
async def escribir_archivo(nombre_archivo, contenido):
    print(f"Iniciando la escritura en el archivo {nombre_archivo}...")
    with open(nombre_archivo, 'w') as archivo:
        archivo.write(contenido)
    print(f"Escritura en el archivo {nombre_archivo} completada.")

async def main():
    # Nombres de los archivos
    archivo_entrada = 'entrada.txt'
    archivo_salida = 'salida.txt'
    
    # Realizar operaciones asíncronas
    contenido = await leer_archivo(archivo_entrada)
    await escribir_archivo(archivo_salida, contenido)

# Ejecutar el evento asíncrono principal
asyncio.run(main())
```
# Integración

El algoritmo de planificación de discos "Elevator (SCAN)" es un algoritmo de disco que simula el movimiento de un ascensor (elevator) para mover la cabeza de lectura/escritura a lo largo de la pista de un disco. El cabezal se mueve en una dirección hasta llegar al final, y luego cambia de dirección para atender las solicitudes en el sentido contrario.

## 1. Programa en C para Implementar el Algoritmo Elevator (SCAN)

```c
#include <stdio.h>
#include <stdlib.h>

// Función para implementar el algoritmo Elevator (SCAN)
void elevator_scan(int *cylinders, int num_cylinders, int initial_position, int direction) {
    int i, j, temp;
    int max_cylinder = 0, min_cylinder = 0;

    // Determinar el cilindro máximo y mínimo en la solicitud
    for (i = 0; i < num_cylinders; i++) {
        if (cylinders[i] > max_cylinder) {
            max_cylinder = cylinders[i];
        }
        if (cylinders[i] < min_cylinder || min_cylinder == 0) {
            min_cylinder = cylinders[i];
        }
    }

    // Mostrar la dirección del movimiento
    if (direction > 0) {
        printf("Movimiento hacia arriba.\n");
    } else {
        printf("Movimiento hacia abajo.\n");
    }

    // Mover la cabeza de lectura/escritura en la dirección especificada
    if (direction > 0) {
        // Mover hacia el final y luego hacia atrás
        for (i = initial_position; i <= max_cylinder; i++) {
            for (j = 0; j < num_cylinders; j++) {
                if (cylinders[j] == i) {
                    printf("Atendiendo solicitud en el cilindro %d\n", i);
                }
            }
        }
        // Cambiar la dirección y mover hacia el principio
        for (i = max_cylinder - 1; i >= min_cylinder; i--) {
            for (j = 0; j < num_cylinders; j++) {
                if (cylinders[j] == i) {
                    printf("Atendiendo solicitud en el cilindro %d\n", i);
                }
            }
        }
    } else {
        // Mover hacia el principio y luego hacia el final
        for (i = initial_position; i >= min_cylinder; i--) {
            for (j = 0; j < num_cylinders; j++) {
                if (cylinders[j] == i) {
                    printf("Atendiendo solicitud en el cilindro %d\n", i);
                }
            }
        }
        // Cambiar la dirección y mover hacia el final
        for (i = min_cylinder + 1; i <= max_cylinder; i++) {
            for (j = 0; j < num_cylinders; j++) {
                if (cylinders[j] == i) {
                    printf("Atendiendo solicitud en el cilindro %d\n", i);
                }
            }
        }
    }
}

int main() {
    int cylinders[] = {10, 22, 25, 36, 40, 45, 50};
    int num_cylinders = sizeof(cylinders) / sizeof(cylinders[0]);
    int initial_position = 30;  // Posición inicial de la cabeza de lectura/escritura
    int direction = 1;  // 1 para arriba, -1 para abajo

    elevator_scan(cylinders, num_cylinders, initial_position, direction);

    return 0;
}
```
## 1. Sistema para Manejo de Múltiples Dispositivos Simulados

Este documento describe y presenta un programa en C que simula la gestión de múltiples dispositivos (disco duro, impresora y teclado) y cómo se realiza la comunicación entre ellos.

### Diseño del Sistema

### Descripción de los Dispositivos Simulados
1. **Disco duro**: Simula la lectura y escritura de datos en un disco.
2. **Impresora**: Simula la impresión de un documento o mensaje.
3. **Teclado**: Simula la entrada de datos por parte del usuario.

#### Comunicación entre Dispositivos
Los dispositivos se comunicarán a través de una función central que coordina las operaciones y transfiere datos de un dispositivo a otro.

### Programa en C para Simular el Manejo de Múltiples Dispositivos

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Estructura para representar un dispositivo genérico
typedef struct {
    char nombre[20];
    char datos[100];
} Dispositivo;

// Función para simular la lectura de datos desde un disco duro
void leer_disco(Dispositivo *disco, const char *contenido) {
    strcpy(disco->datos, contenido);
    printf("Disco duro: Datos leídos -> %s\n", disco->datos);
}

// Función para simular la impresión de datos desde una impresora
void imprimir(Dispositivo *impresora, const char *contenido) {
    strcpy(impresora->datos, contenido);
    printf("Impresora: Imprimiendo -> %s\n", impresora->datos);
}

// Función para simular la entrada de datos desde un teclado
void leer_teclado(Dispositivo *teclado, const char *entrada) {
    strcpy(teclado->datos, entrada);
    printf("Teclado: Entrada de datos -> %s\n", teclado->datos);
}

// Función para coordinar la comunicación entre dispositivos
void comunicacion(Dispositivo *teclado, Dispositivo *disco, Dispositivo *impresora) {
    // Simula la lectura de datos desde el teclado
    leer_teclado(teclado, "Texto de prueba para imprimir");
    
    // Simula la transferencia de datos desde el teclado al disco
    leer_disco(disco, teclado->datos);

    // Simula la impresión de los datos desde el disco
    imprimir(impresora, disco->datos);
}

int main() {
    Dispositivo disco = {"Disco Duro", ""};
    Dispositivo impresora = {"Impresora", ""};
    Dispositivo teclado = {"Teclado", ""};

    // Simular la comunicación entre dispositivos
    comunicacion(&teclado, &disco, &impresora);

    return 0;
}
```
# Avanzados

Los sistemas operativos modernos implementan diversas técnicas para optimizar las operaciones de entrada/salida (E/S), y una de las más importantes es el uso de la memoria caché. Esta técnica permite mejorar significativamente el rendimiento al reducir el tiempo de acceso a los datos y disminuir la carga sobre los dispositivos de almacenamiento más lentos.

## ¿Qué es la Memoria Caché?
La memoria caché es un tipo de memoria de acceso rápido que se utiliza para almacenar temporalmente los datos que se utilizan con frecuencia. Los sistemas operativos la utilizan para almacenar datos de acceso reciente o anticipado, de modo que cuando se realiza una operación de E/S, se puede acceder a estos datos desde la caché en lugar de buscar en dispositivos de almacenamiento más lentos, como discos duros o unidades SSD.

## Cómo Optimiza la Memoria Caché las Operaciones de Entrada/Salida
1. **Reducción de la Latencia**: Cuando un programa solicita datos, el sistema operativo primero busca en la memoria caché. Si los datos están presentes (lo que se conoce como un "acierto de caché"), se pueden leer de manera casi instantánea, reduciendo la latencia de acceso.

2. **Minimización de Accesos a Disco**: Al almacenar los datos recientemente leídos o escritos en la caché, el sistema evita accesos repetidos al disco, que son mucho más lentos en comparación con el acceso a la memoria RAM. Esto reduce la carga de trabajo del disco y mejora el rendimiento general del sistema.

3. **Algoritmos de Sustitución de Caché**: Los sistemas operativos implementan algoritmos sofisticados para decidir qué datos deben mantenerse en la caché y cuáles deben ser reemplazados. Algunos de los algoritmos más comunes son:
   - **LRU (Least Recently Used)**: Reemplaza el bloque de caché que no ha sido utilizado por más tiempo.
   - **FIFO (First In, First Out)**: Reemplaza el bloque de caché más antiguo.
   - **LFU (Least Frequently Used)**: Reemplaza el bloque que se ha utilizado menos veces.

4. **Escrituras y Operaciones de E/S Asíncronas**: Las operaciones de escritura en caché pueden realizarse de manera asíncrona, lo que significa que la escritura se realiza en la caché primero y se sincroniza con el dispositivo de almacenamiento más tarde. Esto permite que el sistema continúe su operación mientras se completan las escrituras en segundo plano, mejorando la eficiencia de las operaciones de E/S.

5. **Caché de Disco y Caché de Red**: En sistemas distribuidos, se utilizan cachés de disco y cachés de red para optimizar el acceso a archivos y datos compartidos. Esto minimiza la cantidad de tráfico de red y los tiempos de espera al acceder a archivos almacenados en servidores remotos.

## Ejemplo de Optimización en la Práctica
Los sistemas de bases de datos y servidores web son ejemplos donde la caché juega un papel crucial. Cuando un servidor recibe una solicitud de un archivo o datos, primero busca en la caché del sistema. Si el archivo está en caché, se entrega al cliente rápidamente. De lo contrario, el sistema accede al disco, lo carga en la caché y lo entrega, almacenando una copia para futuras solicitudes.

## Conclusión
El uso de la memoria caché en los sistemas operativos modernos es esencial para optimizar las operaciones de entrada/salida. Permite reducir la latencia, minimizar los accesos a disco y manejar de manera más eficiente las operaciones de escritura. La implementación de algoritmos de reemplazo de caché y la escritura asíncrona son componentes clave que contribuyen a un rendimiento superior en la gestión de datos.
