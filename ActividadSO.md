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
