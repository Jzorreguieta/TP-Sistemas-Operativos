# Simulador de Sistema Operativo — Barney Stinson Team

Proyecto académico grupal desarrollado en la materia **Sistemas Operativos** de la Universidad Tecnológica Nacional (UTN FRBA), durante el primer cuatrimestre de 2024.

El trabajo consiste en la implementación de un simulador de sistema operativo en **C**, organizado en cuatro módulos independientes: **Kernel, CPU, Memoria y Entrada/Salida (I/O)**. Los módulos se comunican mediante sockets y colaboran para simular la planificación y ejecución de procesos, la administración de memoria y las operaciones de entrada/salida.

## Arquitectura

| Módulo | Responsabilidad |
|---|---|
| **Kernel** | Gestiona el ciclo de vida y los estados de los procesos, las colas de planificación, el grado de multiprogramación y los recursos del sistema. |
| **CPU** | Ejecuta las instrucciones de los procesos, atiende interrupciones y realiza la traducción de direcciones lógicas. |
| **Memoria** | Administra la memoria mediante paginación, tablas de páginas y operaciones de lectura y escritura. |
| **Entrada/Salida (I/O)** | Implementa interfaces de entrada/salida y operaciones sobre el sistema de archivos DialFS. |

## Características implementadas

- **Planificación de CPU:** algoritmos FIFO, Round Robin (RR) y Virtual Round Robin (VRR).
- **Gestión de procesos:** administración de estados, colas, bloqueos y grado de multiprogramación.
- **Administración de recursos:** instrucciones `WAIT` y `SIGNAL` para solicitar y liberar recursos.
- **Comunicación entre módulos:** intercambio de mensajes mediante sockets.
- **Concurrencia y sincronización:** uso de hilos POSIX (pthreads) y semáforos.
- **Ejecución de instrucciones:** ciclo de ejecución de instrucciones y gestión de interrupciones.
- **Traducción de direcciones:** implementación de una TLB con políticas de reemplazo FIFO y LRU.
- **Memoria virtual:** administración mediante paginación.
- **Sistema de archivos:** implementación de DialFS y operaciones sobre archivos.

## Tecnologías y herramientas

- **Lenguaje:** C.
- **Concurrencia:** POSIX Threads (pthreads) y semáforos.
- **Comunicación:** sockets.
- **Bibliotecas:** [so-commons-library](https://github.com/sisoputnfrba/so-commons-library).
- **Compilación:** GCC y Make.
- **Herramientas de análisis:** los Makefiles incluyen objetivos para ejecutar Valgrind (`start`, `memcheck` y `helgrind`).

## Estructura del repositorio

```text
.
├── cpu/             # Ejecución de instrucciones y TLB
├── entradasalida/   # Interfaces de I/O y DialFS
├── kernel/          # Planificación y gestión de procesos
├── memoria/         # Administración de memoria
└── utils/           # Biblioteca auxiliar compartida por los módulos
```

Cada módulo contiene su código fuente en `src/`, un `makefile` y archivos de configuración para distintos escenarios de prueba.

## Requisitos

El proyecto está preparado para compilarse en un entorno Linux y requiere:

- GCC.
- GNU Make.
- Git.
- La biblioteca `so-commons-library` de la cátedra.
- Bibliotecas de desarrollo requeridas por `so-commons-library` y el proyecto.

## Instalación de dependencias

Clonar e instalar la biblioteca de la cátedra siguiendo sus instrucciones:

```bash
git clone https://github.com/sisoputnfrba/so-commons-library
cd so-commons-library
make debug
sudo make install
```

## Compilación

Desde el directorio raíz del repositorio, compilar primero la biblioteca auxiliar del proyecto y luego los módulos:

```bash
make -C utils
make -C kernel
make -C cpu
make -C memoria
make -C entradasalida
```

Los ejecutables se generan en el directorio `bin/` de cada módulo.

## Ejecución

Los módulos se ejecutan de manera independiente, cada uno en una terminal. Como punto de partida, los archivos de configuración incluyen escenarios de planificación, memoria, entrada/salida, sistema de archivos y detección de bloqueos.

Ejemplo de inicio utilizando los archivos de configuración del escenario de planificación:

```bash
# Terminal 1: Memoria
cd memoria
./bin/memoria memoria_PLANIFICACION.config

# Terminal 2: CPU
cd cpu
./bin/cpu cpu_PLANIFICACION.config

# Terminal 3: Kernel
cd kernel
./bin/kernel kernel_PLANIFICACION.config

# Terminal 4: una interfaz de I/O (si el escenario la requiere)
cd entradasalida
./bin/entradasalida STDOUT STDOUT.config
```

**Importante:** antes de ejecutar, revisar las rutas configuradas en los archivos `.config`. En particular, `PATH_INSTRUCCIONES` en los archivos de Memoria y `PATH_BASE_DIALFS` en los archivos de I/O deben apuntar a directorios existentes en el entorno local. También se deben mantener consistentes las direcciones IP y los puertos entre los módulos.

Los archivos `.psc` incluidos en el repositorio contienen programas de instrucciones utilizados en distintos escenarios de prueba. Los archivos `.config` permiten seleccionar parámetros y configuraciones específicas para cada escenario.

## Contexto académico

Trabajo práctico grupal de la cátedra Sistemas Operativos de UTN FRBA. El proyecto se desarrolló siguiendo los requerimientos y las consignas establecidos por la cátedra.
