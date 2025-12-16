# EmuladorArquitecturax86
Emulador interactivo de un microprocesador basado en el modelo de arquitectura de Von Neumman

# Documentación del Simulador Interactivo de Arquitectura Von Neumann
## Descripción General
Este simulador interactivo, implementado en Microsoft Excel con VBA, modela el funcionamiento básico de una computadora basada en la arquitectura Von Neumann, incluyendo memoria caché (L1, L2, L3) y memoria virtual. El simulador permite a los usuarios ejecutar instrucciones, visualizar el ciclo de instrucción (fetch-decode-execute), gestionar cuellos de botella y observar el comportamiento de la caché y la memoria virtual. Está diseñado como una herramienta educativa para comprender los fundamentos de la arquitectura de computadoras, específicamente el modelo Von Neumann, con énfasis en la interacción entre el contador de programa (PC), registros, memoria y operaciones aritméticas.
El simulador utiliza una hoja de Excel llamada "Micro" como interfaz principal, donde los usuarios pueden:

Ingresar instrucciones y parámetros en las celdas C9:E29.
Interactuar mediante botones ("Ejecutar" y "Resetear") o modificando el contador de programa (celda D31).
Observar el estado de la caché (U5:W8), la tabla de páginas (S5:T8), y registros específicos (como acumulador, contador, entradas, salidas, etc.).
Visualizar resultados en tiempo real, con celdas resaltadas para indicar operaciones activas.

## Objetivo
El simulador tiene como objetivo principal ilustrar el funcionamiento de una arquitectura Von Neumann, permitiendo a los usuarios:

Comprender el ciclo de instrucción y la interacción entre componentes (CPU, memoria, registros).
Observar el impacto del cuello de botella de Von Neumann y cómo las memorias caché y virtual lo mitigan.
Experimentar con un lenguaje de instrucción simple que incluye operaciones como mover datos, sumar, restar, incrementar y saltar condicionalmente.

## Estructura del Proyecto
El proyecto está dividido en tres módulos principales en VBA:

### Módulo Estándar (SimuladorVMCache):
Contiene la lógica para la simulación de memoria caché y memoria virtual, incluyendo inicialización, traducción de direcciones y lectura de memoria con caché.
Código: Constantes, variables globales, InicializarSimulacionVMCache, Sim_TranslateVirtualToPhysical, Sim_ReadMemory, Sim_ActualizarParaPC.

### Módulo de la Hoja "Micro":
Maneja eventos de interacción con la hoja, incluyendo clics en botones ("Ejecutar" y "Resetear") y cambios en el contador de programa (D31).
Código: btnEjecutar_Click, btnResetear_Click, Worksheet_Change, InicializarSimIfNeeded.

### Módulo ThisWorkbook:
Ejecuta la inicialización del simulador al abrir el libro de Excel.
Código: Workbook_Open.

### Módulo Estándar (Código Original):
Contiene la lógica del simulador original, que ejecuta un lenguaje de instrucción simple con operaciones como Mover, Sumar, Restar, Incrementar, Saltar, y Saltar Igual.
Código: EjecutarInstruccion, SaltarIgualLugar, SaltarIgualNumero, Resetear, MoverNumeroLugar, MoverLugarLugar, Incrementar, Restar, Sumar, ColorCeldaActiva, QuitarColores.

## Requisitos

- Microsoft Excel: Versión compatible con VBA (por ejemplo, Excel 2016 o superior).
- Habilitar Macros: Los usuarios deben habilitar macros al abrir el archivo .xlsm para que el simulador funcione.
- Hoja "Micro": La hoja debe estar configurada con las celdas especificadas (C9:E29 para instrucciones, D31 para PC, S5:T8 para tabla de páginas, U5:W8 para caché, etc.).
- Git: Usado para control de versiones, almacenando módulos VBA (.bas y .cls) y documentación.

### Configuración de la Hoja "Micro"
- La hoja "Micro" es la interfaz principal del simulador, con las siguientes celdas y sus propósitos:
C9:E29: Área de memoria RAM simulada.
Columna C: Instrucciones (Mover, Sumar, Restar, Incrementar, Saltar, Saltar Igual).
Columna D: Primer parámetro (número o lugar, como "Entrada0", "Acumulador").
Columna E: Segundo parámetro (lugar o número, según la instrucción).

D31: Contador de programa (PC), indica la instrucción actual (0 a 20).
S5:T8: Tabla de páginas para memoria virtual.
Columna S: Offset físico de la página.
Columna T: Bit de validez (1 = válida, 0 = inválida).

U5:W8: Interfaz de la caché.
Columna U: Etiqueta (tag) de la línea de caché.
Columna V: Datos almacenados en la caché.
Columna W: Estado (HIT o MISS).

Z2: Muestra el estado actual del simulador (por ejemplo, "PC=0 | Inst=Mover").
Registros:
Contador: H11
Acumulador: N11
Estado: Q16
Registro1: M23
Registro2: O23
Entrada0-3: G27:G30
Salida0-3: I27:I30

## Lenguaje de Instrucción
El simulador soporta un lenguaje de instrucción simple con las siguientes operaciones:
- Mover: Transfiere un valor (número o contenido de un lugar) a otro lugar.
Ejemplo: Mover 5 Salida0 (mueve el número 5 a Salida0).
Ejemplo: Mover Entrada0 Registro1 (mueve el valor de Entrada0 a Registro1).

- Sumar: Suma los valores de Registro1 (M23) y Registro2 (O23), guarda el resultado en el Acumulador (N11), y establece Estado (Q16) a 0.
- Restar: Resta Registro2 de Registro1, guarda el valor absoluto en el Acumulador, y establece Estado a 1 si Registro1 < Registro2, o 0 si no.
- Incrementar: Incrementa el Contador (H11) en 1.
- Saltar: Salta a la instrucción especificada si el Contador (H11) es igual a un valor dado.
Ejemplo: Saltar 5 (salta a la instrucción 5 si Contador = 0).

- Saltar Igual: Salta a la instrucción especificada si el Contador es igual a un número o al valor de otro lugar.
Ejemplo: Saltar Igual Entrada0 5 (salta a la instrucción 5 si Contador = valor de Entrada0).

### Lugares Válidos:
- Entrada0-3 (G27:G30)
- Salida0-3 (I27:I30)
- Registro1 (M23)
- Registro2 (O23)
- Contador (H11)
- Acumulador (N11)
- Estado (Q16)

## Funcionalidades del Simulador
1. Ciclo de Instrucción
Fetch: Lee la instrucción desde la memoria (C9:E29) según el PC (D31).
Decode: Interpreta la instrucción y sus parámetros, validando tipos (número o texto).
Execute: Realiza la operación correspondiente (Mover, Sumar, etc.).
Write-back: Actualiza registros, memoria, o el PC según la instrucción.
La subrutina EjecutarInstruccion maneja este ciclo, resaltando la instrucción actual (C9:E29) con color.

2. Gestión de Memoria Caché
Implementa una caché de 4 líneas (U5:W8) con política de reemplazo FIFO.
La subrutina Sim_ReadMemory verifica si los datos están en la caché (HIT) o los lee de la RAM (MISS), actualizando la interfaz (U5:W8).
La caché almacena etiquetas (U), datos (V), y estado (W).

3. Memoria Virtual
Usa una tabla de páginas (S5:T8) para traducir direcciones virtuales a físicas (Sim_TranslateVirtualToPhysical).
Soporta 4 páginas, cada una de tamaño 4 (definido por SIM_PAGE_SIZE).
Si una página es inválida o la dirección está fuera de rango, devuelve un error.

4. Interfaz Interactiva
Botones:
Ejecutar: Ejecuta la instrucción actual (EjecutarInstruccion) y avanza el PC (D31) si no es un salto.
Resetear: Restablece todos los registros y salidas a 0 (Resetear), limpia colores, y pone el PC en 0.

Cambio en D31: Dispara Worksheet_Change para simular automáticamente la instrucción en la dirección especificada, actualizando caché y estado (Z2).

5. Visualización
Resalta celdas activas (instrucciones, registros) con color (ColorCeldaActiva).
Limpia colores previos (QuitarColores) para mantener la interfaz clara.
Muestra el estado en Z2 (por ejemplo, "PC=0 | Inst=Mover").

## Instrucciones de Uso
- Configurar la Hoja "Micro":
Ingresar instrucciones en C9:E29 (por ejemplo, Mover Entrada0 Salida0 en C9:E9).
Configurar entradas (G27:G30) con valores iniciales si es necesario.
Opcionalmente, configurar la tabla de páginas (S5:T8) con offsets físicos y bits de validez.

- Inicializar el Simulador:
Al abrir el libro, Workbook_Open ejecuta InicializarSimulacionVMCache, configurando la caché (U5:W8) y la tabla de páginas (S5:T8).
Si no hay datos en S5:T8, se inicializa con un mapeo directo.

- Ejecutar Instrucciones:
Manualmente: Hacer clic en el botón "Ejecutar" para procesar la instrucción actual.
Automáticamente: Cambiar el valor en D31 para simular la instrucción en esa dirección.
Observar la caché (U5:W8) para ver HIT/MISS y Z2 para el estado.

- Reiniciar:
Hacer clic en el botón "Resetear" para limpiar registros, salidas, y el PC.

- Validar Instrucciones:
Asegurarse de que las instrucciones y parámetros sean válidos (ver "Lenguaje de Instrucción").
Los errores muestran un mensaje ("Error en la instrucción. Revise las reglas del lenguaje").

## Estructura del Código
Módulo Estándar (SimuladorVMCache)

### Constantes:
SIM_CACHE_SIZE: 4 (tamaño de la caché).
SIM_PAGE_COUNT: 4 (número de páginas).
SIM_PAGE_SIZE: 4 (tamaño de página).
SIM_RAM_START_ROW: 9 (fila inicial de la RAM en C9:E9).
SIM_PAGETABLE_ROW_START: 5 (tabla de páginas en S5:T8).
SIM_PAGETABLE_COL_OFFSET: 19 (columna S).
SIM_CACHE_UI_ROW_START: 5 (caché en U5:W8).
SIM_CACHE_UI_COL_TAG: 21 (columna U).
SIM_CACHE_UI_COL_DATA: 22 (columna V).
SIM_CACHE_UI_COL_VALID: 23 (columna W).

### Variables Globales:
Sim_CacheTags, Sim_CacheData, Sim_CacheValid: Arrays para la caché.
Sim_PageTable: Tabla de páginas (offset físico, validez).
Sim_LastPC: Último PC procesado.

### Subrutinas:
InicializarSimulacionVMCache: Inicializa la caché y la tabla de páginas.
Sim_TranslateVirtualToPhysical: Traduce direcciones virtuales a físicas.
Sim_ReadMemory: Lee datos de la memoria con caché (HIT/MISS).
Sim_ActualizarParaPC: Simula la instrucción en el PC dado, actualizando caché y estado.

## Módulo de la Hoja "Micro"
Eventos:
btnEjecutar_Click: Llama a EjecutarInstruccion al hacer clic en el botón "Ejecutar".
btnResetear_Click: Llama a Resetear al hacer clic en el botón "Resetear".
Worksheet_Change: Detecta cambios en D31, ejecuta Sim_ActualizarParaPC para simular la instrucción.
InicializarSimIfNeeded: Inicializa el simulador si no se ha hecho.

## Módulo ThisWorkbook
Evento:
Workbook_Open: Ejecuta InicializarSimulacionVMCache al abrir el libro.

## Módulo Estándar (Código Original)
Subrutinas:
EjecutarInstruccion: Lee y ejecuta la instrucción en el PC actual, validando parámetros y manejando errores.
SaltarIgualLugar: Salta a una dirección si el Contador (H11) es igual al valor de un lugar.
SaltarIgualNumero: Salta a una dirección si el Contador es igual a un número.
Resetear: Restablece registros, salidas y el PC a 0.
MoverNumeroLugar: Mueve un número a un lugar (Salida0-3, Registro1-2, Contador).
MoverLugarLugar: Mueve el valor de un lugar (Entrada0-3, Acumulador, etc.) a otro.
Incrementar: Incrementa el Contador en 1.
Restar: Resta Registro2 de Registro1, guarda el resultado en el Acumulador, y actualiza Estado.
Sumar: Suma Registro1 y Registro2, guarda el resultado en el Acumulador.
ColorCeldaActiva: Resalta la celda activa con color.
QuitarColores: Limpia colores de celdas de memoria y registros.

## Limitaciones
Lenguaje de Instrucción: Limitado a 6 instrucciones y lugares específicos, no soporta operaciones complejas.
Caché: Usa FIFO simple, sin soporte para algoritmos como LRU.
Memoria Virtual: Soporta solo 4 páginas, sin manejo avanzado de paginación o swapping.
Errores: Los mensajes de error son genéricos y no especifican detalles del problema.
Interfaz: La visualización depende de colores y celdas, lo que puede ser confuso si no se limpia correctamente.

## Recomendaciones para Mejoras
- Validación de Datos:
Agregar comprobaciones en Sim_ReadMemory para manejar celdas no numéricas o vacías.
Mejorar mensajes de error en EjecutarInstruccion para indicar el problema específico.

- Interfaz Visual:
Usar formato condicional en Excel para resaltar HIT (verde) y MISS (rojo) en la columna W.
Agregar un botón para limpiar manualmente la caché sin reiniciar todo.

- Escalabilidad:
Permitir configurar el tamaño de la caché o el número de páginas desde celdas de Excel.
Implementar algoritmos de reemplazo de caché adicionales (por ejemplo, LRU).

- Integración:
Fusionar Resetear con InicializarSimulacionVMCache para un reinicio consistente.
Evitar bucles en Worksheet_Change si EjecutarInstruccion modifica D31, usando Application.EnableEvents.

## Instalación y Ejecución
Abrir el archivo .xlsm y habilitar macros.
Configurar la hoja "Micro" con instrucciones y datos iniciales.
Usar los botones "Ejecutar" y "Resetear" o modificar D31 para interactuar.
Guardar módulos VBA en un repositorio Git para control de versiones.

## Conclusión
Este simulador es una herramienta educativa efectiva para ilustrar los conceptos de la arquitectura Von Neumann, incluyendo el ciclo de instrucción, la gestión de memoria caché y la memoria virtual. La integración de Excel y VBA permite una interfaz interactiva y visualmente clara, ideal para estudiantes y entusiastas de la informática. Con las mejoras sugeridas, el simulador puede volverse más robusto y versátil, ampliando su utilidad en contextos académicos.