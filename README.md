# Explicación del Funcionamiento de Simulador de paginacion LRU
Este programa simula un entorno de juego donde múltiples entidades (Héroes y Monstruos) interactúan simultáneamente en un mapa, utilizando la potencia del multithreading para lograr la concurrencia.
## 📋 Características Principales
   La simulación se basa en la creación de múltiples hilos de ejecución, uno por cada entidad: $N$ hilos para los Héroes y $M$ hilos para los Monstruos.
### •Gestión de Memoria Virtual: 
Simula la división de la memoria en marcos (frames) y páginas.
### •Algoritmo LRU:
Cuando la RAM está llena, el sistema identifica y mueve a Swap la página que no ha sido utilizada por más tiempo.
### •Manejo de Fallos de Página (Page Faults):
Detecta cuando una página solicitada está en Swap y realiza el proceso de Swap-In (trayéndola a RAM y expulsando otra si es necesario).

### •Ciclo de Vida de Procesos:
• Creación dinámica de procesos con tamaños aleatorios.
• Asignación de memoria bajo demanda.
• Eliminación de procesos y liberación de recursos.
### •Métricas en Tiempo Real:
Visualización del uso de RAM, Swap y cantidad de procesos activos segundo a segundo.
## 🚀 Requisitos
• Compilador de C++ compatible con C++11 o superior (GCC, Clang, MSVC).
• Sistema Operativo: Linux, Windows o macOS.
## 🔨 Cómo Compilar
```bash
g++ main.cpp -o main
```
## ▶️ Cómo Ejecutar
1. ejecutar programa
```bash
./main
```
2.Configuración Inicial: El programa pedirá dos valores de entrada:
RAM total (MB): Cantidad de memoria física a simular (ej. 16, 32, 64).
Tamaño de Página (KB): Tamaño de cada página/marco (ej. 4, 8, 16).
La memoria virtual se calculara aleatoreamente multiplicando la ram total por un factor aleatorio entre 1.5 y 4.5
## 🧠 Lógica de la Simulación
El sistema funciona mediante un bucle de tiempo infinito que avanza por segundos (TIEMPO_SIMULACION_ACTUAL):
1. Fase de Carga (0 - 30 segundos):
El sistema crea procesos agresivamente cada 2 segundos para intentar llenar la memoria RAM rápidamente y forzar el uso inicial.
2. Fase Estable (> 30 segundos):
###  • Cada 5 segundos ocurre un "Evento Programado" que consiste en:
#### 1. Matar un proceso aleatorio: Libera memoria (marcos en RAM y espacio en Swap).
#### 2. Acceso a memoria: La CPU intenta leer una dirección virtual de un proceso existente.
• Si la página está en RAM: HIT (Se actualiza su tiempo de acceso para el LRU).
• Si la página está en Swap: PAGE FAULT (Se trae a RAM, posiblemente expulsando otra página antigua).
#### 3.Crear nuevo proceso: Para mantener la carga en el sistema.
