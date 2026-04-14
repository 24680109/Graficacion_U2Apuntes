# Graficacion_U2Apuntes
# Unidad 2: Graficación 2D

## Introducción

La graficación 2D es una de las áreas fundamentales dentro de la computación gráfica, ya que permite representar objetos en un plano mediante coordenadas. A través de diferentes técnicas como transformaciones, curvas, fractales y el uso de texto, es posible crear desde figuras simples hasta animaciones complejas.

Estas herramientas son ampliamente utilizadas en videojuegos, interfaces gráficas, simuladores y software de diseño, permitiendo una interacción más dinámica entre el usuario y el sistema.

---

# 2.1 Transformación bidimensional

Las transformaciones bidimensionales son operaciones matemáticas que permiten modificar la posición, tamaño, orientación o forma de un objeto dentro de un plano cartesiano.

Estas transformaciones son esenciales en gráficos por computadora, ya que permiten manipular objetos sin necesidad de redefinirlos completamente.

---

## 2.1.1 Traslación

La traslación consiste en desplazar un objeto de una posición a otra dentro del plano sin alterar su forma ni tamaño.

Se realiza sumando una cantidad constante a las coordenadas del objeto.

x' = x + t_x
y' = y + t_y

Donde:

* t_x es el desplazamiento horizontal
* t_y es el desplazamiento vertical

Esta transformación es muy utilizada en animaciones, por ejemplo, cuando un personaje se mueve dentro de una pantalla.

---

## 2.1.2 Escalamiento

El escalamiento permite aumentar o disminuir el tamaño de un objeto.

x' = x * s_x
y' = y * s_y

Donde:

* s_x es el factor de escala en el eje X
* s_y es el factor de escala en el eje Y

Si los valores son mayores a 1, el objeto crece.
Si son menores a 1, el objeto se reduce.

Esta transformación es útil en zoom, redimensionamiento de imágenes y efectos visuales.

---

## 2.1.3 Rotación

La rotación permite girar un objeto alrededor de un punto, generalmente el origen.

x' = x cos(θ) - y sin(θ)

y' = x sin(θ) + y cos(θ)

Donde:

* θ representa el ángulo de rotación

Esta transformación es fundamental en simulaciones, videojuegos y modelado gráfico.

---

## 2.1.4 Sesgado

El sesgado (o "shear") modifica la forma de un objeto inclinándolo en una dirección determinada.

Este tipo de transformación cambia la geometría del objeto sin alterar necesariamente su área, generando efectos visuales interesantes.

Es utilizado en diseño gráfico y animaciones para simular perspectiva o deformaciones.

---

## 🔹 Aplicación práctica: Control de objeto con teclado (Blender)

Como parte de la implementación de las transformaciones bidimensionales, se desarrolló un ejemplo práctico utilizando Blender y su API en Python (`bpy`). En este ejercicio, se controla la posición de un objeto en tiempo real mediante el uso de las teclas de dirección del teclado.

Este ejemplo representa una aplicación directa de la **traslación**, ya que el objeto cambia su posición en los ejes coordenados al presionar las flechas.

Archivo: `ejemplos/transformaciones_blender.py`

```python
import bpy

class ModalMoveOperator(bpy.types.Operator):
    """Controlar objeto con flechas del teclado"""
    bl_idname = "object.modal_move_operator"
    bl_label = "Modal Move Operator"

    def modal(self, context, event):

        obj = bpy.data.objects.get("GPencil")

        if obj is None:
            return {'RUNNING_MODAL'}

        # Movimiento en eje X (izquierda/derecha)
        if event.type == 'LEFT_ARROW' and event.value == 'PRESS':
            obj.location.x -= 0.5

        elif event.type == 'RIGHT_ARROW' and event.value == 'PRESS':
            obj.location.x += 0.5

        # Movimiento en eje Z (arriba/abajo)
        elif event.type == 'UP_ARROW' and event.value == 'PRESS':
            obj.location.z += 0.5

        elif event.type == 'DOWN_ARROW' and event.value == 'PRESS':
            obj.location.z -= 0.5

        # Salir del programa
        elif event.type == 'ESC':
            print("Control finalizado.")
            return {'FINISHED'}

        return {'RUNNING_MODAL'}

    def invoke(self, context, event):
        context.window_manager.modal_handler_add(self)
        print("Control activo: Usa flechas para mover el objeto.")
        return {'RUNNING_MODAL'}

bpy.utils.register_class(ModalMoveOperator)
bpy.ops.object.modal_move_operator('INVOKE_DEFAULT')
```


### Explicación

En este código se implementa un operador modal en Blender que detecta eventos del teclado para modificar la posición de un objeto llamado **GPencil**.

* Las flechas izquierda y derecha modifican la posición en el eje X.
* Las flechas arriba y abajo modifican la posición en el eje Z.
* La tecla ESC finaliza la ejecución.

Cada vez que se presiona una tecla, se aplica una transformación de **traslación**, ya que se suma o resta un valor a las coordenadas del objeto.

Este tipo de implementación permite observar de manera práctica cómo funcionan las transformaciones bidimensionales en tiempo real.

# Ejemplo de como cambio 
<img width="316" height="261" alt="image" src="https://github.com/user-attachments/assets/eebec321-d0b3-4097-bc91-5bfe8742b63d" />
<img width="322" height="262" alt="image" src="https://github.com/user-attachments/assets/bbc11ab9-ded4-4487-8d73-0f5e3f110a1f" />
<img width="321" height="266" alt="image" src="https://github.com/user-attachments/assets/121ef805-5789-45f2-b0f9-7dfc12a8046d" />
<img width="322" height="259" alt="image" src="https://github.com/user-attachments/assets/f1aaf932-687b-4bb2-8648-f39fb50e891f" />

---

# 2.2 Representación matricial de transformaciones

Las transformaciones bidimensionales pueden representarse mediante matrices, lo que permite realizar cálculos de forma más eficiente y combinar múltiples transformaciones en una sola operación.

| x' |   | a  b |   | x |
|----| = |------| * |---|
| y' |   | c  d |   | y |

Esto equivale a:

x' = a*x + b*y
y' = c*x + d*y

El uso de matrices es fundamental en motores gráficos, ya que permite optimizar cálculos y aplicar varias transformaciones de manera simultánea.

---

# 2.3 Trazo de líneas curvas

Las curvas permiten representar formas más complejas que las líneas rectas, lo cual es esencial en gráficos avanzados.

---

## 2.3.1 Curvas Bézier

Las curvas Bézier se construyen a partir de puntos de control que determinan la forma de la curva.

Son muy utilizadas en diseño gráfico, animación y tipografía, ya que permiten crear trayectorias suaves y precisas.

Una de sus principales características es que la curva siempre está contenida dentro del área definida por sus puntos de control.

---

## 2.3.2 Curvas B-Spline

Las curvas B-Spline son una extensión de las curvas Bézier y permiten mayor flexibilidad.

A diferencia de las Bézier:

* No necesariamente pasan por todos los puntos
* Permiten modificar solo una parte de la curva sin afectar toda la figura

Son ampliamente utilizadas en modelado 3D y diseño industrial.

---

# 2.4 Fractales

Los fractales son figuras geométricas que presentan auto-similitud, es decir, su estructura se repite a diferentes escalas.

Estos se generan mediante procesos iterativos y pueden producir formas muy complejas a partir de reglas simples.

Algunos ejemplos conocidos son:

* Conjunto de Mandelbrot
* Árboles fractales
* Curva de Koch

Los fractales son utilizados en:

* Gráficos por computadora
* Simulación de la naturaleza
* Arte digital

---

# 2.5 Uso y creación de fuentes de texto

El uso de texto en gráficos es fundamental para mostrar información dentro de una aplicación.

Las fuentes permiten definir:

* Tipo de letra
* Tamaño
* Color
* Estilo

En programación, las fuentes se utilizan para mostrar títulos, etiquetas, instrucciones y datos dentro de una interfaz gráfica.

Esto mejora la comunicación con el usuario y hace más comprensible la aplicación.

---

# Conclusión

La graficación 2D es una herramienta esencial en el desarrollo de software moderno. A través de transformaciones, curvas, fractales y el uso de texto, es posible crear aplicaciones visuales dinámicas e interactivas.

El uso de herramientas como Blender y bibliotecas gráficas permite llevar estos conceptos teóricos a la práctica, facilitando su comprensión y aplicación en proyectos reales.

---

# Bibliografías
## Bibliografía

- GeeksforGeeks. 2D Transformations in Computer Graphics. https://www.geeksforgeeks.org/computer-graphics-2d-transformation/
- TutorialsPoint. Computer Graphics - 2D Transformation. https://www.tutorialspoint.com/computer_graphics/2d_transformation.htm
- MDN Web Docs. Bezier Curve. https://developer.mozilla.org/en-US/docs/Glossary/Bezier_curve
- MathWorld. B-Spline. https://mathworld.wolfram.com/B-Spline.html
- Khan Academy. Fractals. https://www.khanacademy.org/
- Britannica. Fractal. https://www.britannica.com/science/fractal
- W3Schools. Canvas Text. https://www.w3schools.com/
- Blender Foundation. Blender Python API. https://docs.blender.org/api/current/


