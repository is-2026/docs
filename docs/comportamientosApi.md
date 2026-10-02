# <center> **Contrato API Comportamientos** </center>

### **Estructura Obligatoria del Código (Plantilla)**

Para que el motor de simulación pueda interpretar y ejecutar el comportamiento de un jugador, todo código proporcionado por el usuario debe incluir obligatoriamente una función principal llamada `decide_action()`. El sistema invocará esta función automáticamente en cada tick.

La función `decide_action()` no recibe parámetros de entrada y toda acción ejecutada tomará exactamente un (1) tick. Las acciones de movimiento no son continuas, si se necesita que un jugador se desplace hacia un punto, se debe indicar la dirección de movimiento y la intensidad en cada tick sucesivo. Para que el jugador conozca el estado actual del partido, debe invocar las **Primitivas de posición (Lectura de Entorno)** dentro de esta función.

**Ejemplo de plantilla:**
```python
def decide_action():
    # 1. Obtener el estado usando primitivas
    x, y = my_pos()
    px, py = ball_pos()
    
    # 2. Lógica del comportamiento para este tick exacto
    if has_ball():
        shoot_arco()
    else:
        # Calcular el vector de dirección hacia la pelota
        dx = px - x
        dy = py - y
        # Moverse hacia la pelota al 100% de la velocidad
        move(dx, dy, 100)
```

### **Cómo influyen los atributos (PACSS)**
El motor resuelve las interacciones basándose en los atributos del jugador en cada tick:
* **Power:** Determina con cuánta fuerza sale la pelota en un pase o tiro.
* **Agility:** Define el tiempo de recuperación (cooldown) tras patear. No podrá volver a patear la pelota hasta que pase este tiempo.
* **Control:** Determina el radio de alcance del jugador. Si la pelota está dentro de este radio, podrá intentar interactuar con ella.
* **Speed:** Define qué tan rápido corre el jugador al moverse. El movimiento no tiene inercia: si en un tick no se invoca `move()`, el jugador se queda quieto.
* **Strength:** Si en un mismo tick dos o más jugadores (rivales o compañeros) tienen la pelota en su radio e intentan patear o pasar la pelota simultáneamente, el motor compara el atributo Strength de todos ellos: gana el impacto el de mayor Strength y se anulan las acciones del resto. Si hay empate, el ganador se elige al azar.

### **Sistema de Coordenadas**

Todas las primitivas trabajan en un sistema de coordenadas **relativo a tu equipo**: siempre ves la cancha como si tu equipo atacara hacia la derecha (**+x**). El motor se encarga de convertir las coordenadas según el lado en el que juegue cada equipo, por lo que un mismo comportamiento funciona igual para el equipo local, para el visitante y después del cambio de lado en el entretiempo.

* Las coordenadas están expresadas en **metros**, con el origen `(0, 0)` en el **centro de la cancha**.
* La cancha mide 40 m de largo por 20 m de ancho: `x` va de `-20` a `20` e `y` va de `-10` a `10`.
* El **arco rival** está sobre `x = 20` y el **arco propio** sobre `x = -20`. Ambos miden 3 m de ancho (`-1.5 <= y <= 1.5`).
* Las direcciones (`dx`, `dy`) de las primitivas de acción se interpretan en este mismo sistema: un `dx` positivo apunta hacia el arco rival.

### **Primitivas de estado (Lectura de Entorno)**

Estas funciones no reciben parámetros y se utilizan para obtener el estado actual del campo de juego en el tick actual. Todas las coordenadas que devuelven están expresadas en el sistema de coordenadas de tu equipo (ver **Sistema de Coordenadas**).

| Primitiva | Parámetros | Retorno | Descripción | 
| :--- | :--- | :--- | :--- |
| `my_pos()` | Ninguno | `(x, y)` | Devuelve las coordenadas horizontales y verticales actuales del jugador. |
| `ball_pos()` | Ninguno | `(x, y)` | Devuelve las coordenadas actuales de la pelota en la cancha. |
| `has_ball()` | Ninguno | `Booleano` | Retorna `true` si la pelota se encuentra dentro de tu radio de alcance actual (determinado por el atributo **Control**), habilitándote para intentar tocarla. |
| `team_pos()` | Ninguno | `Lista de tuplas` | Retorna una lista con el ID y las coordenadas de los compañeros de equipo, sin incluir al propio jugador, ej: `[(id, x, y), ...]`. |
| `enemy_pos()`| Ninguno | `Lista de tuplas` | Retorna una lista con el ID y las coordenadas de los jugadores rivales. |
| `score()` | Ninguno | `(int, int)` | Devuelve el marcador actual del partido con el formato goles (propios, rival). |
| `time()` | Ninguno | `int` | Retorna el tiempo actual del partido **medido en ticks**. |
| `total_time()` | Ninguno | `int` | Retorna la duración total del partido **medida en ticks**. |

### **Primitivas de Acciones**

Estas funciones requieren parámetros de entrada y dictan la acción del jugador únicamente para el tick actual. No aceptan coordenadas como destino, sino direcciones (vectores `dx`, `dy`) expresadas en el sistema de coordenadas de tu equipo (`dx` positivo apunta hacia el arco rival).

| Primitiva | Parámetros | Descripción | Atributo Asociado |
| :--- | :--- | :--- | :--- |
| `move(dx, dy, porcentaje_velocidad)` | `dx`: Dirección horizontal (vector).<br>`dy`: Dirección vertical (vector).<br>`porcentaje_velocidad`: Porcentaje de velocidad a utilizar (0-100). | Aplica movimiento al jugador en la dirección especificada durante este tick. La distancia recorrida depende del porcentaje y de la velocidad base. Si la dirección es `(0, 0)`, no tiene efecto. Un porcentaje fuera del rango se ajusta a 0-100. | **Speed** |
| `pass_to(jugador_id)` | `jugador_id`: ID del compañero. | Intenta golpear la pelota en dirección al compañero seleccionado en este tick. Si el ID no corresponde a un compañero en cancha (por ejemplo, un rival o el propio jugador), no tiene efecto. | **Agility**, **Power**, **Strength** |
| `shoot(dx, dy, porcentaje_power)` | `dx`: Dirección horizontal del remate.<br>`dy`: Dirección vertical del remate.<br>`porcentaje_power`: Fuerza del golpe (0-100). | Intenta ejecutar un remate en la dirección vectorial especificada durante este tick. | **Agility**, **Power**, **Strength** |
| `shoot_arco()` | Ninguno | Intenta ejecutar un remate apuntando automáticamente en la dirección del arco rival. | **Agility**, **Power**, **Strength** |
