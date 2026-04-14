# Graficacion_U2Apuntes
# Unidad 2: Graficación 2D

## Introducción

La graficación 2D permite representar y manipular objetos dentro de un plano mediante transformaciones matemáticas. Estas transformaciones pueden aplicarse de forma interactiva utilizando eventos del teclado, lo que permite crear animaciones y simulaciones dinámicas.

---

# 2.1 Transformación bidimensional

Las transformaciones bidimensionales permiten modificar la posición, tamaño y orientación de un objeto en un plano.

---

## 2.1.2 Escalamiento

Permite cambiar el tamaño del objeto.

x' = x * s_x  
y' = y * s_y  

---

## 2.1.3 Rotación

Permite girar un objeto alrededor de un punto.

x' = x cos(θ) - y sin(θ)  
y' = x sin(θ) + y cos(θ)
---

## 2.1.4 Sesgado

El sesgado inclina la figura modificando su forma sin cambiar su área base.

---

## 🔹 Ejercicio: Control con teclado (Blender)

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

        if event.type == 'LEFT_ARROW' and event.value == 'PRESS':
            obj.location.x -= 0.5

        elif event.type == 'RIGHT_ARROW' and event.value == 'PRESS':
            obj.location.x += 0.5

        elif event.type == 'UP_ARROW' and event.value == 'PRESS':
            obj.location.z += 0.5

        elif event.type == 'DOWN_ARROW' and event.value == 'PRESS':
            obj.location.z -= 0.5

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

### 🔎 Explicación del código

Este programa utiliza la API de Blender (`bpy`) para controlar un objeto en tiempo real mediante el teclado.

* Se define un operador modal que detecta eventos del teclado.
* Se utiliza el objeto **GPencil** como referencia.
* Las flechas del teclado aplican **traslación**:

  * Izquierda/Derecha → eje X
  * Arriba/Abajo → eje Z
* Se utiliza ESC para finalizar la ejecución.

Este ejemplo demuestra una aplicación práctica de las transformaciones bidimensionales mediante interacción del usuario.

---

# 2.2 Representación matricial

Las transformaciones pueden representarse mediante matrices, lo que permite combinarlas de manera eficiente:

[
\begin{bmatrix}
x' \
y'
\end{bmatrix}
=============

\begin{bmatrix}
a & b \
c & d
\end{bmatrix}
\begin{bmatrix}
x \
y
\end{bmatrix}
]

Esto es fundamental en motores gráficos y software como Blender.

---

# 2.3 Curvas

## 2.3.1 Bézier

Se definen mediante puntos de control y permiten crear curvas suaves.

## 2.3.2 B-Spline

Son curvas más flexibles que no necesariamente pasan por todos los puntos.

---

## 🔹 Ejercicio: Animación de curva

Archivo: `ejemplos/curvas.py`

```python
# (tu código anterior aquí)
```

---

# 2.4 Fractales

Los fractales son estructuras que se repiten a diferentes escalas.

---

## 🔹 Ejemplo

Archivo: `ejemplos/fractales.py`

```python
# (tu código anterior aquí)
```

---

# 2.5 Uso y creación de fuentes de texto

Permite mostrar información en pantalla dentro de aplicaciones gráficas.

---

## 🔹 Ejemplo

Archivo: `ejemplos/fuentes.py`

```python
# (tu código anterior aquí)
```

---

# Conclusión

Las transformaciones, curvas y fractales son fundamentales en la graficación 2D. Además, el uso de herramientas como Blender permite aplicar estos conceptos de manera interactiva, facilitando la comprensión de su funcionamiento.

---

# Autor

[Tu nombre]
