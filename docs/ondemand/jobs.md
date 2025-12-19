# Gestor de Tareas

Open Ondemand cuena con una interfaz de gestión de tareas que se comunica con el servidor de SLURM (gestor de colas) de Khipu. Podemos acceder a sus herramientas ingresando a la plataforma y, una vez en el panel principal, seleccionando el menú **Jobs**.

![Menú de Herramientas de gestión de tareas](/assets/images/ood-job-menu.png)

## Tareas Activas
En la ventana "Active Jobs" podemos ver:

- Nuestras tareas activas en un *cluster*
- Todas las tareas activas en un *cluster*
- Nuestras tareas activas en todos los *clusters*
- Todas las tareas activas en todos los *clusters*

![Lista de tareas activas](/assets/images/ood-job-list.png)

Podemos ver los detalles de cada una por separado, incluyendo quién la envió, en qué partición se encuentra, en qué nodos y cuántos recursos requiere. También podemos eliminar tareas en ejecución.

![Detalles de una tarea](/assets/images/ood-job-details.png)

## Compositor de Tareas

El compositor de tareas es una herramienta gráfica que facilita:

- La creación de *scripts* de ejecución
- El envío de tareas

El panel principal nos permite ejecutar tareas guardadas, modificar sus parámetros, crear plantillas, cargar *scripts* desde otras ubicaciones.

![Compositor de Tareas](/assets/images/ood-job-composer.png)