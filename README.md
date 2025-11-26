# Cyber physical and Robotic Intelligent Systems
En este repositorio se recogen soluciones a algunos ejercicios de Robotics Academy.

## 1. Instrucciones de Ejecución

### Prerrequisitos
* Tener Docker instalado y funcionando correctamente.
* Tener acceso a Unibotics o Robotics Academy.
* Python 3.x (solo si vas a ejecutar el controlador localmente).
* Bibliotecas de Python necesarias: opencv-python, numpy.

### Lanzamiento
#### Inicie el contenedor Docker
Inicie el contenedor Docker en su máquina. Abre una terminal y ejecuta una de las siguientes opciones según tu sistema:
1. Sin aceleración gráfica (recomendado para la mayoría de usuarios):
   ```bash
      docker run --rm -it -p 6080-6090:6080-6090 -p 7163:7163 \
      jderobot/robotics-backend:latest
   ```
2. En Linux con tarjeta gráfica:
   ```bash
      docker run --rm -it --device /dev/dri \
     -p 6080-6090:6080-6090 -p 7163:7163 \
     jderobot/robotics-backend:latest
   ```
3. En sistemas con GPU Nvidia:
   (asegúrate de tener instalados los drivers propietarios y el [Nvidia Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
    ```bash
       docker run --rm -it --device /dev/dri --gpus all \
       -p 6080-6090:6080-6090 -p 7163:7163 \
       jderobot/robotics-backend:latest
    ```
Mientras el contenedor esté en ejecución, podrá navegar por Unibotics sin necesidad de reiniciarlo.

##### Reiniciar el Docker
*   Identificar contenedores Docker activos: `docker ps -a`
*   Detener el contenedor: `docker stop ID_DEL_CONTENEDOR`
*   Eliminar el contenedor: `docker rm ID_DEL_CONTENEDOR`

#### Flujo de Trabajo en Unibotics
Una vez que el contenedor del Robotics Backend se esté ejecutando en tu terminal, sigue estos pasos para conectarte y ejecutar un ejercicio:

1. Acceder y Autenticarse:
* Ve a https://unibotics.org en tu navegador.
* Si eres un usuario nuevo, haz clic en REGISTER. Deberás completar los campos: Nombre (Name), Apellido (Surname), Nombre de usuario (Username), Email y Contraseña (Password).
* Si ya tienes una cuenta, haz clic en LOG IN e introduce tu Nombre de usuario (Username) y Contraseña (Password).

2. Navegar a la Academia:
* Después de iniciar sesión, serás dirigido al panel de aplicaciones (unibotics.org/apps).
* Haz clic en Robotics Academy (Learn robotics facing practical challenges).

3. Seleccionar un Ejercicio:
* Dentro de la Academia, selecciona un curso disponible, como el Free Course.
* Elige uno de los ejercicios prácticos disponibles. El PDF muestra la selección del ejercicio Follow Line.

4. Conectar y Ejecutar:
* Se abrirá la interfaz del ejercicio, mostrando el editor de código Python a la izquierda y los monitores de simulación a la derecha.
* Verás un mensaje que dice "Click Play to connect to the Robotics Backend".
* Haz clic en el botón Connect (o el botón de Play).
* La interfaz web se conectará a tu contenedor Docker local. Tu terminal de Docker mostrará un mensaje confirmando la conexión, como "client connected".

5. Probar el Código:
* Una vez conectado, la simulación se iniciará.
* Puedes ver la simulación 3D del robot y monitores adicionales como el Im Monitor que muestra la visión de la cámara del robot.
* El código Python se estará ejecutando, y la consola en la interfaz web mostrará cualquier salida.