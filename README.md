# Explicación del Funcionamiento de C-DOOM-LITE
Este programa simula un entorno de juego donde múltiples entidades (Héroes y Monstruos) interactúan simultáneamente en un mapa, utilizando la potencia del multithreading para lograr la concurrencia.
## Concurrencia: Threads y Mutex
   La simulación se basa en la creación de múltiples hilos de ejecución, uno por cada entidad: $N$ hilos para los Héroes y $M$ hilos para los Monstruos.
### ¿Por qué Threads?
Al utilizar threads (hilos), conseguimos que las acciones de todos los personajes (moverse, buscar objetivos, atacar) se ejecuten concurrentemente en lugar de secuencialmente. Esto simula un entorno en tiempo real más realista.
### Uso de Mutex (Exclusión Mutua)
En entornos concurrentes, múltiples hilos intentan leer y escribir en las mismas variables globales (como la posición posicion_actual o los puntos de vida vida_actual de un personaje). Si dos hilos actualizan una variable al mismo tiempo, se produce una condición de carrera.
### El programa utiliza un Mutex (pthread_mutex_t mutex_principal_del_juego) para la sincronización:
#### • ¿Qué es un Mutex?
Es un mecanismo de bloqueo que garantiza la Exclusión Mutua. Solo un hilo puede "poseer" el Mutex a la vez
#### • ¿Cómo funciona?
1. Antes de acceder a cualquier variable global compartida (como la vida_monstruo o la posicion_actual), el hilo llama a pthread_mutex_lock(&mutex_principal_del_juego).
2. Una vez que el hilo ha terminado de leer o modificar el dato crítico, llama a pthread_mutex_unlock(&mutex_principal_del_juego) para liberar el Mutex.
#### • En la Práctica
Funciones como encontrar_monstruo_mas_cercano o el bloque donde se reduce la vida_actual del objetivo deben estar protegidas por el Mutex.
## Lógica de Juego y Comportamiento del Hilo
### Hilo del Monstruo (thread_monstruo)
• Detección: Bajo el bloqueo del Mutex, busca al Héroe vivo más cercano.
• Alerta: Si el Héroe está dentro de su rango_vision_monstruo, se establece esta_alertado_monstruo = 1.
• Movimiento: Si el Monstruo está fuera de su rango_ataque_monstruo, avanza un paso.
• Ataque: Si el Monstruo está dentro de su rango_ataque_monstruo, aplica su dano_monstruo al Héroe.
### Hilo del Héroe (thread_heroe)
• Prioridad de Combate: Primero, verifica si hay un Monstruo en su alcance_de_ataque.
• Ataque Bloqueante: Si detecta un Monstruo, llama a la función ataque_heroe_monstruos(). Esta función se convierte en un bucle bloqueante que ataca repetidamente a todos los Monstruos en rango hasta que el rango   esté despejado. Durante este bucle, el Héroe no se mueve.
• Movimiento: Si no hay amenazas en rango, el Héroe procede al siguiente punto en su ruta (arreglo_de_ruta) predefinida.
## El Parser de Configuración (lectura_config)
La función lectura_config(const char *nombre_archivo) es crucial para inicializar el estado del juego. Utiliza un enfoque de dos pasos:
### 1. Primer Paso (Contador): 
Recorre el archivo una vez para determinar CANTIDAD_TOTAL_DE_HEROES y CANTIDAD_TOTAL_DE_MONSTRUOS. Esto es necesario para asignar memoria dinámica (calloc).
### 2. Segundo Paso (Asignación de Valores):
Vuelve al inicio del archivo (rewind(file)) y lo recorre nuevamente para asignar los valores leídos a las estructuras globales (ARREGLO_HEROES, ARREGLO_MONSTRUOS).
#### • Parsin del path:
Extrae los pares de coordenadas (x,y) de la línea PATH y los almacena en h->arreglo_de_ruta.
## Instrucciones de Compilación y Ejecución
Este codigo esta programado para ser compilado y ejecutado con los comandos estándar para proyectos basados en pthreads
Se debe guarda como main.c y utilizar los siguientes comandos
### 🔨 Cómo Compilar
```bash
gcc main.c -o doom_sim -pthread
```
### ▶️ Cómo Ejecutar
```bash
./doom_sim ejemplox.txt
```
