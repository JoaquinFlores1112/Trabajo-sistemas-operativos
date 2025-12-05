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
### 🔨 Cómo Compilar
1. ejecutar programa
```bash
g++ main.cpp -o main
```
2.
### ▶️ Cómo Ejecutar
```bash
./main
```
