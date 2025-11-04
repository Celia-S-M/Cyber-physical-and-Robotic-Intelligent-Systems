# Autonomous-Racing-Challenge

Este repositorio contiene la solución al ejercicio "Visual Follow Line" de Robotics Academy. El objetivo es programar un coche de Fórmula 1 simulado para que siga de forma autónoma la línea roja pintada en un circuito de carreras, utilizando un controlador PID reactivo.

## 1. Descripción del Funcionamiento

La solución se basa en un bucle de control que procesa imágenes de la cámara del vehículo para calcular y aplicar las velocidades lineal y angular necesarias.

### Detección de la Línea

El primer paso es identificar la línea a seguir. El proceso es el siguiente:

1.  **Obtención de Imagen:** En cada iteración, se captura la imagen de la cámara frontal del vehículo (en formato BGR8).
2.  **Filtrado de Color:** Se aplica un filtro de color específico (usando OpenCV) para aislar únicamente los píxeles que corresponden a la línea roja. Esto genera una imagen binaria (blanco y negro) llamada "máscara".
3.  **Cálculo del Error:** Se procesa la máscara para encontrar el centroide (el centro) de la línea detectada. El "error" se define como la diferencia horizontal entre el centroide de la línea y el "Set Point" (el punto objetivo, que es el centro horizontal de la imagen).
    * Si `error = 0`, el coche está perfectamente centrado.
    * Si `error > 0`, la línea está a la derecha del coche.
    * Si `error < 0`, la línea está a la izquierda del coche.

### Controlador PD

Para que el coche siga la línea, se implementa un controlador de bucle cerrado. Este controlador calcula continuamente el error y aplica una corrección. Para este proyecto, se utilizó un **controlador PD** (Proporcional-Derivativo).

* **Término Proporcional (P):** Corrige el error actual. Es la base del control: la velocidad angular (giro) es *proporcional* al error.
* **Término Derivativo (D):** Considera la *tasa de cambio* del error (el error actual menos el error anterior). Este término ayuda a amortiguar las oscilaciones y a anticipar curvas, suavizando el movimiento del coche.

La velocidad angular (`w`) se calcula como: `w = (Kp * error) + (Kd * derivada_del_error)`.
La velocidad lineal (`v`) se mantiene constante para simplificar el control.

### Ajuste de Ganancias (Tuning)

Las constantes $K_p$ y $K_d$ son cruciales. Se ajustaron usando el método de **Prueba y Error (Trial and Error)**.

1.  Se empezó con $K_d = 0$ y se aumentó $K_p$ gradualmente hasta que el coche empezó a seguir la línea, aunque con oscilaciones.
2.  Luego, se aumentó $K_d$ para reducir las oscilaciones y estabilizar la respuesta.

Las ganancias finales utilizadas en este código son:

```python
# Ganancias del controlador
Kp = 0.0045
Kd = 0.001
Ki = 0.0  # No se usa en un controlador PD
## 2. Arquitectura del Software

[cite_start]El código está implementado en **Python** utilizando la arquitectura **HAL (Hardware Abstraction Layer)** proporcionada por Robotics Academy[cite: 25, 26, 27].

* [cite_start]Se importan las bibliotecas `HAL` y `WebGUI`[cite: 27, 29].
* [cite_start]El bucle principal se regula a una frecuencia constante usando `Frequency.tick(ideal_rate)`[cite: 13].
* [cite_start]La imagen de la cámara se obtiene con `HAL.getImage()`[cite: 31].
* [cite_start]Las velocidades lineal y angular se envían al simulador con `HAL.setV(velocity)` y `HAL.setW(velocity)`[cite: 32, 34].
* [cite_start]La máscara de color procesada se visualiza en la interfaz de depuración usando `WebGUI.showImage(image)`[cite: 35].

*(Si usaste ROS 2, deberías reemplazar esta sección por la siguiente):*

## 3. Instrucciones de Ejecución

### Prerrequisitos

* [cite_start]Tener Robotics Academy instalado y configurado (ver [guía de usuario](https://jderobot.github.io/RoboticsAcademy/user_guide/#installation))[cite: 8, 9].
* Python 3.x.
* [cite_start]Bibliotecas de Python: `opencv-python`, `numpy` (necesarias para el procesamiento de imágenes [cite: 163, 164]).

### Instalación

1.  Clona este repositorio en tu máquina local:
    ```bash
    git clone [https://github.com/TU_USUARIO/TU_REPOSITORIO.git](https://github.com/TU_USUARIO/TU_REPOSITORIO.git)
    cd TU_REPOSITORIO
    ```

2.  Instala las dependencias de Python (si aún no las tienes):
    ```bash
    pip install opencv-python numpy
    ```

### Lanzamiento

1.  En una terminal, lanza el ejercicio `follow_line` desde la aplicación de Robotics Academy.
2.  Abre una segunda terminal y navega al directorio donde clonaste este repositorio.
3.  Ejecuta el script de la solución:
    ```bash
    python follow_line_solution.py
    ```
    *(Asegúrate de que el nombre del script coincida con el de tu archivo)*

## 4. Ejemplos y Rendimiento

### Capturas de Pantalla

A continuación, se muestra el funcionamiento del algoritmo. La imagen superior es la vista de la cámara, y la inferior es la imagen procesada (máscara) que usa el controlador para calcular el error.

| Vista de la Cámara | Imagen Procesada (Máscara) |
| :---: | :---: |
| ![Vista de la cámara en recta](https://i.imgur.com/example_camera_straight.png) | ![Máscara procesada en recta](https://i.imgur.com/example_mask_straight.png) |
| *Coche en una recta* | *Detección de la línea en la recta* |
| ![Vista de la cámara en curva](https://i.imgur.com/example_camera_curve.png) | ![Máscara procesada en curva](https://i.imgur.com/example_mask_curve.png) |
| *Coche tomando una curva* | *Detección de la línea en la curva* |


### Rendimiento
* **Término Proporcional (P):** 112
* **Término Integral (I):**
* **Término Derivativo (D):**
