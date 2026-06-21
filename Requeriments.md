# 🚀 Proyecto A: "TaskFlow" - Gestor de Proyectos
> **Momento recomendado para realizarlo:** Hacer tras la *Sección 5: Manejo de Excepciones*.

## 🎯 Objetivo
Dominar **IoC (Inyección de Dependencias)**, **Excepciones personalizadas**, **Programación Funcional en Java (Streams)** y el **manejo manual de archivos (JSON)**, sin involucrar bases de datos todavía.

---

## 💼 El Problema de Negocio
Un equipo de desarrollo necesita una API REST básica para gestionar sus Tareas (con un estilo similar a Trello o Jira).

---

## 🛠️ Requisitos Arquitectónicos y Técnicos

### 1. Persistencia Manual (Patrón Repository Simulado)
* Crea una interfaz llamada `TaskRepository`.
* Implementa la clase `TaskRepositoryJsonImpl` que se encargue de leer y escribir una lista de objetos en un archivo llamado `tareas.json`, utilizando la librería **Jackson** (u otra similar de Java).
* El repositorio debe ser gestionado e inyectado automáticamente por Spring utilizando la anotación `@Repository`.

### 2. Tipos de Tareas (Patrón Factory)
* Existen tres tipos de tareas diferenciadas: `Bug`, `Feature` y `Epic`.
* Utiliza el patrón **Fábrica Simple (Simple Factory)** para instanciar las tareas correspondientes antes de ser guardadas.
* Cada tipo de tarea tiene reglas de negocio distintas (por ejemplo: un `Bug` siempre debe crearse con prioridad *ALTA* de forma predeterminada).

### 3. Lógica de Negocio Pura (Streams & Programación Funcional)
Tu servicio (`@Service`) debe incluir un método llamado `generarReporte()`. Este método debe hacer uso exclusivo de la **API Stream de Java 8+** para realizar las siguientes operaciones:
* **Filtrar:** Aislar las tareas que se encuentren atrasadas (`.filter(...)`).
* **Agrupar:** Clasificar las tareas según su Estado (`TODO`, `IN_PROGRESS`, `DONE`) utilizando `.collect(Collectors.groupingBy(...))`.
* **Calcular:** Obtener el porcentaje total de completitud mapeando y procesando los datos de las tareas (`.map(...)`, `.reduce(...)`).

### 4. Manejo de Excepciones Global (ControllerAdvice)
* Si un usuario intenta buscar una tarea mediante un identificador que no existe en el sistema, el backend debe lanzar una excepción personalizada: `TaskNotFoundException`.
* Implementa un manejador global utilizando `@ControllerAdvice` y `@ExceptionHandler` (conceptos vistos en la sección 5).
* El manejador debe capturar la excepción y retornar al cliente un objeto **JSON estructurado** que contenga de forma clara:
  * El mensaje de error.
  * La fecha y hora exacta del suceso.
  * El código de estado HTTP correspondiente (`404 Not Found`).