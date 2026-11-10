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
* **Control:** Determina el radio de alcance del jugador. Si la pelota está dentro de este radio, podrá intentar interactuar con ella. También define la precisión de los remates: cada remate se desvía al azar hasta 30° para cada lado con Control 20 y hasta 5° con Control 100.
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
| `shoot(dx, dy, porcentaje_power)` | `dx`: Dirección horizontal del remate.<br>`dy`: Dirección vertical del remate.<br>`porcentaje_power`: Fuerza del golpe (0-100). | Intenta ejecutar un remate en la dirección vectorial especificada durante este tick. | **Agility**, **Power**, **Strength**, **Control** |
| `shoot_arco()` | Ninguno | Intenta ejecutar un remate apuntando automáticamente en la dirección del arco rival. | **Agility**, **Power**, **Strength**, **Control** |

---

### **Tanda de penales**

En la tanda, cada pateador usa su comportamiento: el que eligió el club para la tanda o, si no eligió, el que tenía en el partido. El comportamiento no sabe que es un penal: ve un partido con un solo compañero (él mismo) y un rival, y dura como máximo 80 ticks. Cuando a ese jugador le toca atajar, su comportamiento no se usa: al arquero lo maneja el sistema.

# <center> **Contrato del DT** </center>

Un comportamiento de tipo DT (`kind = "coach"`) no controla a un jugador: dirige al equipo durante el partido. Cambia comportamientos de los jugadores en cancha, programa sustituciones y elige a los pateadores de la tanda de penales.

Mientras el club tiene un DT activo, el usuario no puede hacer cambios manuales en ese partido: los endpoints de cambio de comportamiento en vivo, sustituciones y orden de penales responden `409` para ese club. El DT que dirige un partido es el que el club tenía activo cuando el partido empezó. Activarlo, cambiarlo o quitarlo rige desde el próximo partido y no cambia el que se está jugando.

### **Estructura Obligatoria del Código (Plantilla)**

Todo código de un DT debe incluir una función principal llamada `decide_coach()`, que no recibe parámetros.

* El sistema la invoca **una vez por tick mientras se juega**. No se invoca durante las pausas (hidratación y entretiempo).
* Si el partido se define por penales, se invoca **una sola vez más al abrir la ventana de penales**, para que el DT elija a los pateadores. En esa llamada `is_penalty_shootout()` devuelve `True`.
* El código se carga una sola vez al empezar el partido, así que **las variables globales se mantienen entre ticks**. Sirven para que el DT recuerde lo que ya hizo.
* Tiene las mismas restricciones que un comportamiento de jugador: los mismos builtins permitidos, solo puede importar `math` y tiene el mismo límite de tiempo por llamada.
* Si `decide_coach()` lanza una excepción o se pasa del tiempo, **ese tick no se aplica ninguna orden** y el partido sigue normalmente. En el tick siguiente se vuelve a invocar.

**Ejemplo de plantilla:**
```python
defendiendo = False


def buscar_comportamiento(nombre):
    for behavior_id, behavior_name in behaviors():
        if behavior_name == nombre:
            return behavior_id
    return None


def decide_coach():
    global defendiendo

    # 1. Tanda de penales: elegir a los 5 pateadores
    if is_penalty_shootout():
        pateador = buscar_comportamiento("Delantero")
        if pateador is not None:
            convocados = [player_id for player_id, _ in on_field()] + bench()
            penalty_order([(player_id, pateador) for player_id in convocados[:5]])
        return

    # 2. Durante el juego: si va ganando en el segundo tiempo, pasar a defender
    propios, rival = score()
    if not defendiendo and time() >= total_time() // 2 and propios > rival:
        defensor = buscar_comportamiento("Defensor")
        if defensor is not None:
            for player_id, _ in on_field():
                set_behavior(player_id, defensor)
            defendiendo = True
```

### **Primitivas de estado (Lectura del Partido)**

Estas funciones no reciben parámetros y devuelven el estado del partido en el tick actual. Todos los IDs son strings. El DT **no ve posiciones**: no tiene acceso a la ubicación de la pelota ni de los jugadores.

| Primitiva | Parámetros | Retorno | Descripción |
| :--- | :--- | :--- | :--- |
| `score()` | Ninguno | `(int, int)` | Devuelve el marcador actual del partido con el formato goles (propios, rival). |
| `time()` | Ninguno | `int` | Retorna el tiempo actual del partido **medido en ticks**. |
| `total_time()` | Ninguno | `int` | Retorna la duración total del partido **medida en ticks**. |
| `on_field()` | Ninguno | `Lista de tuplas` | Retorna los jugadores propios que están en cancha con el comportamiento que tiene cada uno, ej: `[(player_id, behavior_id), ...]`. |
| `bench()` | Ninguno | `Lista de IDs` | Retorna los IDs de los jugadores propios que están en el banco de suplentes, ej: `[player_id, ...]`. |
| `behaviors()` | Ninguno | `Lista de tuplas` | Retorna los comportamientos de tipo jugador del club, que son los únicos que el DT puede asignar, ej: `[(behavior_id, nombre), ...]`. |
| `is_penalty_shootout()` | Ninguno | `Booleano` | Retorna `True` solo en la llamada que se hace al abrir la ventana de penales. Durante el juego retorna `False`. |

Los jugadores de `on_field()` más los de `bench()` son los 6 convocados del club para el partido.

### **Primitivas de Acciones**

Estas funciones dan órdenes al equipo. El DT no recibe ningún error: **una orden inválida se ignora** y no tiene efecto (igual que `pass_to` con un ID que no es de un compañero).

| Primitiva | Parámetros | Descripción |
| :--- | :--- | :--- |
| `set_behavior(player_id, behavior_id)` | `player_id`: ID de un jugador propio en cancha.<br>`behavior_id`: ID de un comportamiento de `behaviors()`. | Cambia el comportamiento del jugador. **Rige desde el tick siguiente**: si se llama en el tick 10, el jugador actúa con el nuevo comportamiento desde el tick 11. Si el jugador ya tiene ese comportamiento, no tiene efecto. Si en un mismo tick se llama más de una vez para el mismo jugador, vale la primera. |
| `substitute(player_out_id, player_in_id, behavior_id)` | `player_out_id`: ID de un jugador propio en cancha.<br>`player_in_id`: ID de un jugador propio en el banco.<br>`behavior_id`: ID de un comportamiento de `behaviors()`. | Programa una sustitución: el que entra ocupa la posición del que sale con el comportamiento elegido. **Se ejecuta en la próxima pausa.** El club puede tener una sola sustitución pendiente a la vez y el DT no puede cancelarla. Después de la tercera pausa ya no quedan ventanas de cambio y no tiene efecto. |
| `penalty_order(kickers)` | `kickers`: lista de 5 tuplas `(player_id, behavior_id)`, en el orden en que patean. | Elige a los pateadores de la tanda de penales entre los 6 convocados, sin repetir, cada uno con un comportamiento de `behaviors()`. **Solo tiene efecto en la llamada de penales**; durante el juego se ignora. Si se llama más de una vez, vale la primera. |

### **Casos límite**

* **Órdenes inválidas:** un jugador que no está en cancha o en el banco, un comportamiento que no está en `behaviors()`, una sustitución cuando ya hay una pendiente o cuando no quedan ventanas, o un orden de penales con una cantidad distinta de 5, repetidos o jugadores que no son convocados. Todas se ignoran sin error.
* **Llamada de penales:** `set_behavior` y `substitute` se ignoran. Si el DT no llama a `penalty_order`, el orden es inválido, lanza una excepción o se pasa del tiempo, patean los de por defecto: los 3 que están en cancha por número de camiseta, repetidos hasta completar 5, con el comportamiento que tenían.
* **Excepción o tiempo excedido durante el juego:** ese tick no se aplica ninguna orden y el partido sigue.
