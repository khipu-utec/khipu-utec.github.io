# Open Ondemand

[Open Ondemand](https://www.openondemand.org/) es un servicio web que permite el acceso a ambientes de HPC desde cualquier lugar a través de un navegador. No sólo provee un nuevo método de acceso sino que utiliza interfaces gráficas intuitivas para la gestión de tareas y permite la integración de una gran cantidad de aplicaciones interactivas como Jupyter, COMSOL y Matlab. 

Desde diciembre del 2025, Khipu cuenta con su propio servidor de Open Ondemand. Esto permite a los usuarios interactuar con el Cluster a través de sus navegadores, y ya no exclusivamente a través de SSH.

## ¿Cómo ingresar?
Enlace de Login: [ood.khipu.utec.edu.pe](https://ood.khipu.utec.edu.pe)

![Formulario de acceso](/assets/images/ood-login.png)

Los usuarios de Khipu pueden acceder a los servicios de Open Ondemand a través del [formulario de login](https://ood.khipu.utec.edu.pe) ingresando sus credenciales de acceso a Khipu (las mismas que utiliza para conexiones por ssh).

!!! warning "Importante"
	Antes de poder acceder por la web, es necesario ingresar al menos una vez por `ssh`, ejecutando el comando `ssh <mi-usuario>@khipu.utec.edu.pe` e ingresando su contraseña. Esto sucede porque Khipu crea la carpeta de cada usuario en `/home` cuando este accede por primera vez a través de `ssh`. Si intena ingresar por la web sin haber hecho esto previamente, obtendrá el mensaje `"Home directory not found"`.

## Servicios Disponibles

- [Gestor de Tareas](/ondemand/jobs)
- Aplicaciones Interactivas
	- [Shell Interactiva](/ondemand/shell)
	- [Desktop Gráfico](/ondemand/desktop)
	- [Jupyter Lab](/ondemand/jupyter)