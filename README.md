# Laboratorio: Exploración con DEBUG en DOSBox
**Unidad 3: Manejo del DEBUG - Post-Contenido 1** **Asignatura:** Arquitectura de Computadores  
**Institución:** Universidad Francisco de Paula Santander (UFPS)

---

## 1. Introducción
Este documento detalla el desarrollo de la práctica de laboratorio sobre el uso del depurador **DEBUG** en un entorno **DOSBox**. El objetivo principal es interactuar con la arquitectura interna del procesador x86 en modo real, permitiendo la inspección de registros, la manipulación de la memoria RAM y el ensamblado de instrucciones básicas en lenguaje máquina.

---

## 2. Comandos Utilizados en la Práctica
A continuación, se describen los comandos del DEBUG empleados durante la sesión de laboratorio:

| Comando | Función Técnica | Aplicación en el Laboratorio |
| :--- | :--- | :--- |
| **R (Registers)** | Muestra o modifica el contenido de los registros y banderas. | Verificación del estado inicial y carga del valor 1234 en AX. |
| **F (Fill)** | Rellena un rango de memoria con un patrón de bytes específico. | Inicialización del bloque DS:0200 con el patrón AB CD EF. |
| **D (Dump)** | Muestra el contenido de la memoria en formato hexadecimal y ASCII. | Inspección del patrón de relleno y del área del PSP. |
| **A (Assemble)** | Ensambla instrucciones mnemónicas directamente en memoria. | Escritura del programa de suma (MOV, ADD, INT) en CS:0100. |
| **U (Unassemble)** | Traduce bytes de memoria a instrucciones de ensamblador. | Verificación del código máquina generado y desensamblado inicial. |

---

## 3. Desarrollo y Checkpoints

### Checkpoint 1: Inspección y Modificación de Registros
Al iniciar el DEBUG, se utilizó el comando **R** para observar el estado del procesador. Se identificó que los registros de segmento (**DS, ES, SS, CS**) apuntan al valor **073F**, correspondiente al segmento asignado por DOSBox. 

[Image of x86 CPU registers architecture]

Se realizó la modificación del registro **AX** de `0000` a `1234`. Esta operación demuestra la capacidad del depurador para alterar el estado interno de la CPU de forma selectiva antes de la ejecución de cualquier instrucción.

### Checkpoint 2: Volcado y Relleno de Memoria
Se procedió a manipular la memoria RAM utilizando el comando **F** para rellenar 64 bytes (`L40`) con el patrón cíclico `AB CD EF` en la dirección `0200`.

[Image of computer memory hexadecimal dump visualization]

Al ejecutar el comando **D 200 L40**, se validó lo siguiente:
* **Columna Izquierda:** Indica la dirección física en formato Segmento:Offset (073F:0200).
* **Columna Central:** Muestra los valores hexadecimales ingresados, confirmando que el patrón se repite correctamente.
* **Columna Derecha (ASCII):** Muestra puntos debido a que los valores `AB`, `CD` y `EF` no son caracteres imprimibles en el código ASCII estándar (rango 20h-7Eh).

También se inspeccionó el **PSP (Program Segment Prefix)** en la dirección `0000`, localizando la instrucción `CD 20` (**INT 20**), que sirve para finalizar programas de forma controlada.

### Checkpoint 3: Ensamblado y Desensamblado de Código
Finalmente, se utilizó el comando **A** para ingresar un programa funcional de 4 instrucciones en la dirección `0100`:
1. `MOV AX, 0005` (Codificación: `B8 05 00`)
2. `MOV BX, 0003` (Codificación: `BB 03 00`)
3. `ADD AX, BX`   (Codificación: `01 D8`)
4. `INT 20`        (Codificación: `CD 20`)

Mediante el comando **U**, se verificó la correspondencia entre los mnemónicos y el código máquina. Se observó que el valor `0005` se almacena como `05 00` en memoria, evidenciando el formato **Little-endian** (byte menos significativo primero) propio de la arquitectura x86.

---

## 4. Conclusiones
La práctica permitió consolidar el conocimiento sobre la representación interna de los datos y el funcionamiento de la CPU
