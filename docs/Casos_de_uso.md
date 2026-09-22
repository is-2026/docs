﻿
# <center> Casos de Uso: FutBot </center>

### Índice de Contenidos
  - [Cuenta y sesión](#cuenta-y-sesión)
    - [CU1: Registrar usuario y club](#caso-de-uso-1-registrar-usuario-y-club)
    - [CU2: Iniciar sesión](#caso-de-uso-2-iniciar-sesión)
    - [CU3: Cerrar sesión](#caso-de-uso-3-cerrar-sesión)
    - [CU4: Cambiar contraseña](#caso-de-uso-4-cambiar-contraseña)
    - [CU5: Cambiar avatar del club](#caso-de-uso-5-cambiar-avatar-del-club)
  - [Jugadores](#jugadores)
    - [CU6: Crear jugador](#caso-de-uso-6-crear-jugador)
    - [CU7: Ver mis jugadores](#caso-de-uso-7-ver-mis-jugadores)
    - [CU8: Asignar comportamiento a un jugador](#caso-de-uso-8-asignar-comportamiento-a-un-jugador)
    - [CU9: Eliminar jugador](#caso-de-uso-9-eliminar-jugador)
    - [CU10: Elegir nueva formación](#caso-de-uso-10-elegir-nueva-formación)
  - [Comportamientos](#comportamientos)
    - [CU11: Ver comportamientos](#caso-de-uso-11-ver-comportamientos)
    - [CU12: Crear nuevo comportamiento](#caso-de-uso-12-crear-nuevo-comportamiento)
    - [CU13: Editar comportamiento](#caso-de-uso-13-editar-comportamiento)
    - [CU14: Eliminar comportamiento](#caso-de-uso-14-eliminar-comportamiento)
  - [Club y perfil](#club-y-perfil)
    - [CU15: Ver club de un usuario](#caso-de-uso-15-ver-club-de-un-usuario)
    - [CU16: Ver historial de mi club](#caso-de-uso-16-ver-historial-de-mi-club)
    - [CU17: Ver historial compartido <!-- revisar caso excepcional)-->](#caso-de-uso-17-ver-historial-compartido----revisar-caso-excepcional--)
  - [Partidos amistosos](#partidos-amistosos)
    - [CU18: Enviar desafío amistoso](#caso-de-uso-18-enviar-desafío-amistoso)
    - [CU19: Aceptar desafío amistoso](#caso-de-uso-19-aceptar-desafío-amistoso)
    - [CU20: Cancelar desafío amistoso enviado](#caso-de-uso-20-cancelar-desafío-amistoso-enviado)
    - [CU21: Buscar partido amistoso](#caso-de-uso-21-buscar-partido-amistoso)
    - [CU22: Cancelar busqueda](#caso-de-uso-22-cancelar-busqueda)
  - [Partidos (gestión en vivo)](#partidos-gestión-en-vivo)
    - [CU23: Gestionar alineación pre-partido](#caso-de-uso-23-gestionar-alineación-pre-partido)
    - [CU24: Programar sustitución de jugador](#caso-de-uso-24-programar-sustitución-de-jugador)
    - [CU25: Cancelar sustitución de jugador](#caso-de-uso-25-cancelar-sustitución-de-jugador)
    - [CU26: Cambiar comportamiento durante el partido](#caso-de-uso-26-cambiar-comportamiento-durante-el-partido)
    - [CU27: Observar partido como espectador](#caso-de-uso-27-observar-partido-como-espectador)
    - [CU28: Abandonar partido como espectador](#caso-de-uso-28-abandonar-partido-como-espectador)
  - [Ligas](#ligas)
    - [CU29: Crear liga](#caso-de-uso-29-crear-liga)
    - [CU30: Ver ligas disponibles](#caso-de-uso-30-ver-ligas-disponibles)
    - [CU31: Unirse a liga privada](#caso-de-uso-31-unirse-a-liga-privada)
    - [CU32: Unirse a liga pública](#caso-de-uso-32-unirse-a-liga-pública)
    - [CU33: Abandonar liga](#caso-de-uso-33-abandonar-liga)
    - [CU34: Cancelar liga](#caso-de-uso-34-cancelar-liga)
    - [CU35: Iniciar liga](#caso-de-uso-35-iniciar-liga)
    - [CU36: Consultar fixture](#caso-de-uso-36-consultar-fixture)
    - [CU37: Consultar tabla de posiciones](#caso-de-uso-37-consultar-tabla-de-posiciones)
  - [Ranking](#ranking)
    - [CU38: Consultar ranking global](#caso-de-uso-38-consultar-ranking-global)

## Cuenta y sesión

### Caso de Uso 1: Registrar usuario y club
- **Actor primario:** Usuario visitante
- **Precondición:** El usuario se encuentra en la pantalla de ingreso del sistema.
- **Escenario exitoso principal:**
  1. El usuario inicia el proceso de registro.
  2. El sistema solicita ingresar nombre de usuario, email y contraseña.
  3. El usuario ingresa los datos solicitados y confirma la operación.
  4. El sistema solicita crear club, pidiendo un nombre y un avatar para el mismo, avisando que sólo se puede crear un club por cuenta.
  5. El usuario crea un club ingresando los datos pedidos y confirmando su creación.
  6. El sistema valida los datos y crea la nueva cuenta.
- **Escenarios excepcionales:**
  4. a) El email ingresado ya se encuentra registrado.
  - El sistema informa al usuario que el email proporcionado ya está en uso, y cancela la operación.
  6. a) El usuario intenta crear más de un club.
  - El sistema informa al usuario que sólo se puede crear un club por cuenta, guarda sólo el primer club creado, y pide confirmar la creación de la cuenta, avisando que tendrá asociado a su cuenta el primer club que creó.

### Caso de Uso 2: Iniciar sesión
- **Actor primario:** Usuario
- **Precondición:** El usuario se registró previamente y no está logueado.
- **Escenario exitoso principal:**
  1. El usuario selecciona iniciar sesión .
  2. El sistema solicita ingresar email y contraseña.
  3. El usuario ingresa los datos solicitados y confirma la operación.
  4. El sistema valida los datos e inicia la sesión.
- **Escenarios excepcionales:**
  4. a) El campo email o contraseña ingresado es incorrecto.
  - El sistema informa al usuario que el email o la contraseña proporcionada es incorrecto.

### Caso de Uso 3: Cerrar sesión
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado.
- **Escenario exitoso principal:**
  1. El usuario selecciona cerrar sesión.
  2. El sistema cierra la sesión del usuario y actualiza la vista al apartado de inicio de sesión.

### Caso de Uso 4: Cambiar contraseña
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado.
- **Escenario exitoso principal:**
  1. El usuario selecciona cambiar contraseña.
  2. El sistema solicita la contraseña actual y la nueva contraseña.
  3. El usuario ingresa la información solicitada.
  4. El sistema valida las contraseñas ingresadas y las actualiza en la base de datos.
- **Escenarios excepcionales:**
  4. a) La contraseña actual no es correcta:
  - El sistema informa al usuario que la contraseña actual proporcionada es incorrecta y solicita reingresar la contraseña.
  4. b) La nueva contraseña coincide con la actual:
  - El sistema informa al usuario que la nueva contraseña no puede ser igual a la actual y solicita ingresar otra contraseña.

### Caso de Uso 5: Cambiar avatar del club
- **Actor primario:** Usuario
- **Precondición:** El usuario esta logueado.
  1. El usuario selecciona cambiar avatar del club.
  2. El sistema solicita elegir la nueva imagen para el avatar. 
  3. El usuario selecciona un nuevo avatar y confirma el cambio.
  4. El sistema valida el avatar, lo actualiza en la base de datos y refleja el cambio en la vista del usuario.
- **Escenarios excepcionales:**
  4. a) El archivo no cumple con los requisitos:
  - El sistema informa error y solicita un nuevo archivo.

## Jugadores

### Caso de Uso 6: Crear jugador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema.
- **Escenario exitoso principal:**
  1. El usuario selecciona crear nuevo jugador.
  2. El sistema solicita el nombre del jugador, número de camiseta y los valores para los atributos PACSS (Power, Agility, Control, Speed, Strength), indicando que hay un valor mínimo y un valor máximo permitido para cada uno, y que que su suma debe ser igual a un valor determinado.
  3. El usuario ingresa la información solicitada y confirma la creación.
  4. El sistema valida que los atributos sumen exactamente el valor determinado, crea al jugador y lo muestra en la plantilla.
- **Escenarios excepcionales:**
  6. a) La suma total de los atributos PACSS es distinta al valor determinado.
  - El sistema informa que la suma total de los atributos de un jugador debe ser igual al valor determinado y solicita modificar los valores de los atributos.
  6. b) Algún atributo posee un valor fuera del rango permitido.
  - El sistema informa que el atributo posee un valor fuera del rango permitido, y solicita modificarlo.

### Caso de Uso 7: Ver mis jugadores
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema.
- **Escenario exitoso principal:**
  1. El usuario selecciona visualizar jugadores.
  2. El sistema muestra los jugadores previamente creados.
- **Escenarios excepcionales:**
  2. a) El usuario no posee ningún jugador:
  - El sistema muestra una plantilla vacía.

### Caso de Uso 8: Asignar comportamiento a un jugador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema y tiene al menos un jugador y un comportamiento previamente creado.
- **Escenario exitoso principal:**
  1. El usuario selecciona asignar/reasignar comportamiento a un jugador.
  2. El sistema muestra los comportamientos disponibles.
  3. El usuario selecciona el nuevo comportamiento.
  4. El sistema asigna/reasigna el comportamiento al jugador.

### Caso de Uso 9: Eliminar jugador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema y tiene al menos un jugador previamente creado.
- **Escenario exitoso principal:**
  1. El usuario selecciona eliminar jugador.
  2. El sistema valida que el jugador no esté inscripto en una liga activa ni jugando un partido, advierte sobre la acción irreversible y solicita confirmación.
  3. El usuario confirma la eliminación.
  4. El sistema lo elimina de la base de datos y actualiza la plantilla de jugadores.
- **Escenarios excepcionales:**
  2. a) El jugador se encuentra disputando ligas o partidos activos:
  - El sistema informa que el jugador está en uso y cancela la operación de eliminación.

### Caso de Uso 10: Elegir nueva formación 
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema.
- **Escenario exitoso principal:**
  1. El usuario selecciona gestionar formaciones.
  2. El sistema muestra todas las formaciones, indicando cuál está equipada.
  3. El usuario selecciona una formación.
  4. El sistema desequipa la formación anterior y equipa la formación elegida.

## Comportamientos

### Caso de Uso 11: Ver comportamientos
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema.
- **Escenario exitoso principal:**
  1. El usuario selecciona ver comportamientos.
  2. El sistema muestra los comportamientos creados por el usuario.


### Caso de Uso 12: Crear nuevo comportamiento
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema.
- **Escenario exitoso principal:**
  1. El usuario selecciona crear nuevo comportamiento.
  2. El sistema solicita escribir código para el comportamiento así como un nombre descriptivo.
  3. El usuario escribe el código del nuevo comportamiento, ingresa un nombre para el mismo y confirma la creación.
  4. El sistema valida el código (sintaxis y seguridad), y guarda el comportamiento.
- **Escenarios excepcionales:**
  4. a) No se ingresa ningún nombre:
  - El sistema informa al usuario que la casilla de nombre está vacía y solicita escribirlo.
  4. b) Se ingresa un nombre ya existente:
  - El sistema informa al usuario que ese nombre de comportamiento ya existe, y solicita cambiarlo.
  4. a) El código contiene errores de sintaxis o usa instrucciones prohibidas.
  - El sistema informa los errores detectados en el editor y solicita corregirlos.

### Caso de Uso 13: Editar comportamiento
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y tiene al menos un comportamiento previamente creado.
- **Escenario exitoso principal:**
  1. El usuario selecciona editar comportamiento.
  2. El sistema verifica que el comportamiento no esté en uso y permite su edición.
  3. El usuario edita los datos del comportamiento y confirma la edición.
  4. El sistema valida el comportamiento y lo actualiza en la base de datos.
- **Escenarios excepcionales:**
  2. a) El comportamiento se encuentra en uso por un jugador jugando un partido:
  - El sistema informa al usuario que el comportamiento se encuentra en uso y no es posible editarlo.
  4. a) El código contiene errores de sintaxis o usa instrucciones prohibidas.
  - El sistema informa los errores detectados en el editor y solicita corregirlos.

### Caso de Uso 14: Eliminar comportamiento
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y tiene al menos un comportamiento previamente creado.
- **Escenario exitoso principal:**
  1. El usuario selecciona eliminar comportamiento.
  2. El sistema verifica que el comportamiento no esté en uso y solicita confirmación para eliminar el comportamiento.
  3. El usuario confirma la eliminación del comportamiento.
  4. El sistema elimina el comportamiento elegido. <!-- y lo desasocia de los jugadores que lo tienen asociado -->
- **Escenarios excepcionales:**
  2. a) El comportamiento se encuentra en uso por un jugador jugando un partido:
  - El sistema informa al usuario que el comportamiento se encuentra en uso y no es posible eliminarlo.

## Club y perfil

### Caso de Uso 15: Ver club de un usuario
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado.
- **Escenario exitoso principal:**
  1. El usuario selecciona ver club.
  2. El sistema muestra la vista del club elegido.

### Caso de Uso 16: Ver historial de mi club
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado.
- **Escenario exitoso principal:**
  1. El usuario selecciona el historial.
  2. El sistema actualiza la vista y muestra el historial.

### Caso de Uso 17: Ver historial compartido <!-- revisar caso excepcional)-->
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, viendo otro club.
- **Escenario exitoso principal:**
  1. El usuario selecciona el historial compartido con el club.
  2. El sistema actualiza la vista y muestra el historial compartido.
- **Escenarios excepcionales:**
  2. a) No se puede visualizar el historial compartido de tu propio club:
  - El sistema informa la imposibilidad y se rechaza la operación.

## Partidos amistosos

### Caso de Uso 18: Enviar desafío amistoso
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y tiene al menos la mínima cantidad de jugadores para jugar un partido, cada uno con un comportamiento asignado.
- **Escenario exitoso principal:**
  1. El usuario desafía un club del ranking.
  2. El sistema solicita elegir los jugadores titulares y suplentes, cada uno con un comportamiento asignado, y pide confirmar la acción.
  3. El usuario elige sus titulares y suplentes, y confirma el desafío amistoso.
  4. El sistema valida la elección, envía el desafío al club seleccionado y avisa que el desafío fue enviado con éxito.
- **Escenarios excepcionales:**
  4. a) No se eligieron suficientes jugadores:
  - El sistema advierte que faltan jugadores por elegir, y vuelve a pedir que elija los jugadores faltantes, avisando que los mismos deben tener un comportamiento asignado.
  4. b) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de Uso 19: Aceptar desafío amistoso
- **Actor primario:** Usuario desafiado
- **Precondición:** 
 El usuario está logueado, tiene al menos la mínima cantidad de jugadores para jugar un partido, cada uno con un comportamiento asignado, y un desafío amistoso pendiente.
- **Escenario exitoso principal:**
  1. El usuario acepta un desafío amistoso.
  2. El sistema solicita elegir los jugadores titulares y suplentes, cada uno con un comportamiento asignado, y pide confirmar la acción.
  3. El usuario elige sus titulares y suplentes, y confirma la aceptación del desafío amistoso.
  4. El sistema valida la elección e inicia el partido.
- **Escenarios excepcionales:**
  4. a) No se eligieron suficientes jugadores:
  - El sistema advierte que faltan jugadores por elegir, y vuelve a pedir que elija los jugadores faltantes, avisando que los mismos deben tener un comportamiento asignado.
  4. b) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de Uso 20: Cancelar desafío amistoso enviado
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, y tiene al menos un desafío enviado que aún no ha sido aceptado.
- **Escenario exitoso principal:**
  1. El usuario selecciona cancelar desafío en un desafío enviado.
  2. El sistema registra la cancelación y elimina el desafío.

### Caso de uso 21: Buscar partido amistoso 
- **Actor primario:** Usuario
- **Actor secundario:** Oponente
- **Precondición:** El usuario está logueado viendo el apartado mi club y tiene al menos la mínima cantidad de jugadores para jugar un partido, cada uno con un comportamiento asignado.
- **Escenario exitoso principal:**
  1. El usuario busca partido amistoso.
  2. El sistema solicita elegir los jugadores titulares y suplentes, cada uno con un comportamiento asignado, y pide confirmar la acción.
  3. El usuario elige sus titulares y suplentes, y confirma la acción.
  4. El sistema valida la elección y comienza a buscar un oponente para emparejar con el usuario.  
  5. Aparece un oponente.
  6. El sistema empareja este oponente con el usuario y crea un partido amistoso para ambos.
- **Escenarios excepcionales:**
  4. a) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de uso 22: Cancelar busqueda
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y buscando partido amistoso.
- **Escenario exitoso principal:**
  1. El usuario cancela la búsqueda iniciada.
  2. El sistema finaliza la búsqueda, cancelando el emparejamiento.

## Partidos (gestión en vivo)

### Caso de Uso 23: Gestionar alineación pre-partido
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, por empezar un partido de su liga y convocó previamente sus jugadores con comportamiento asociado.
- **Escenario exitoso principal:**
  1. El usuario ingresa a la sala de preparación del partido.
  2. El sistema permite modificar los titulares y jugadores que jugarán el partido, así como sus comportamientos y formación.
  3. El usuario cambia los titulares y suplentes, reasigna los comportamientos y confirma su alineación.
  4. El sistema registra las modificaciones realizadas. <!-- La capacidad de modificar elementos se termina al iniciar el partido -->
- **Escenarios excepcionales:**
  4. a) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de Uso 24: Programar sustitución de jugador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y visualizando un partido en curso de su club.
- **Escenario exitoso principal:**
  1. El usuario hace un cambio táctico.
  2. El sistema valida que queden cambios, y solicita indicar qué titular sale y qué suplente entra.
  3. El usuario selecciona al jugador saliente y al jugador entrante, y confirma el cambio.
  4. El sistema registra la solicitud, que será ejecutada en la próxima pausa reglamentaria (hidratación o entretiempo).
- **Escenarios excepcionales:**
  2. a) El usuario ya agotó los cambios permitidos:
  - El sistema informa que no quedan cambios disponibles y deniega la operación.
  
### Caso de Uso 25: Cancelar sustitución de jugador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, visualizando un partido en curso de su club, y tiene una solicitud de sustitución registrada.
- **Escenario exitoso principal:**
  1. El usuario cancela el cambio táctico.
  2. El sistema elimina la solicitud previamente registrada.

### Caso de Uso 26: Cambiar comportamiento durante el partido
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, visualizando un partido en curso de su club.
- **Escenario exitoso principal:**
  1. El usuario selecciona cambiar el comportamiento de un jugador activo.
  2. El sistema solicita elegir el comportamiento a reasignar.
  3. El usuario selecciona el nuevo comportamiento y confirma el cambio.
  4. El sistema lo asigna al jugador, y actualiza sus acciones para el siguiente "tick" del simulador.

### Caso de Uso 27: Observar partido como espectador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, viendo una liga a la que pertenece y hay al menos un partido en curso en la misma
- **Escenario exitoso principal:**
  1. El usuario selecciona un partido que se está jugando en vivo.
  2. El sistema muestra el partido con sus jugadores, la pelota y los arcos en tiempo real.

### Caso de Uso 28: Abandonar partido como espectador
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado, visualizando un partido como espectador.
- **Escenario exitoso principal:**
  1. El usuario deja de observar un partido.
  2. El sistema finaliza la transmisión en vivo.

## Ligas

### Caso de Uso 29: Crear liga
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema y tiene al menos la mínima cantidad de jugadores para jugar un partido, cada uno con un comportamiento asignado.
- **Escenario exitoso principal:**
  1. El usuario crea una liga.
  2. El sistema solicita ingresar nombre, contraseña, cantidad máxima de clubes, duración de los partidos, así como seleccionar los titulares y suplentes, cada uno con un comportamiento asignado.
  3. El usuario ingresa los datos solicitados y confirma la creación.
  4. El sistema valida los datos, crea la liga en estado "No iniciada" y anota al club del usuario creador en ella.
- **Escenarios excepcionales:**
  4. a) El usuario omite campos obligatorios:
  - El sistema informa los campos faltantes y solicita completarlos para crear la liga.
  4. b) No se eligieron suficientes jugadores:
  - El sistema advierte que faltan jugadores por elegir, y vuelve a pedir que elija los jugadores faltantes, avisando que los mismos deben tener un comportamiento asignado.
  4. c) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de Uso 30: Ver ligas disponibles
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado exitosamente.
- **Escenario exitoso principal:**
  1. El usuario busca ligas.
  2. El sistema muestra las ligas no iniciadas.

### Caso de Uso 31: Unirse a liga privada
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema, viendo las ligas disponibles, y tiene al menos la mínima cantidad de jugadores para jugar un partido, cada uno con un comportamiento asignado.
- **Escenario exitoso principal:**
  1. El usuario se une a una liga privada.
  2. El sistema valida la acción, y solicita la contraseña de acceso a la liga.
  3. El usuario ingresa la contraseña y confirma.
  4. El sistema valida la contraseña y solicita seleccionar los titulares y suplentes, cada uno con un comportamiento asignado.
  5. El usuario selecciona sus titulares y suplentes y confirma la elección.
  6. El sistema valida la elección, inscribe al usuario en la liga y lo dirige hacia ella.
- **Escenarios excepcionales:**
  2. a) El usuario ya pertenece a la liga seleccionada:
  - El sistema informa que el usuario ya es parte de esa liga y no se permite una doble inscripción a la misma.
  2. b) La liga está llena:
  - El sistema informa al usuario que no se puede unir a la liga pues ésta presenta la cantidad máxima de participantes.
  4. a) La contraseña ingresada por el usuario es incorrecta.
  - El sistema informa que la contraseña es incorrecta y solicita ingresarla nuevamente.
  6. a) No se eligieron suficientes jugadores:
  - El sistema advierte que faltan jugadores por elegir, y vuelve a pedir que elija los jugadores faltantes, avisando que los mismos deben tener un comportamiento asignado.
  6. b) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de Uso 32: Unirse a liga pública
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado en el sistema, viendo las ligas disponibles, y tiene al menos la mínima cantidad de jugadores para jugar un partido, cada uno con un comportamiento asignado.
- **Escenario exitoso principal:**
  1. El usuario se une a una liga pública.
  2. El sistema valida la acción, solicita seleccionar los titulares y suplentes, cada uno con un comportamiento asignado.
  3. El usuario selecciona sus titulares y suplentes y confirma la elección.
  4. El sistema valida la elección, inscribe al usuario en la liga y lo dirige hacia ella.
- **Escenarios excepcionales:**
  2. a) La liga está llena:
  - El sistema informa al usuario que no se puede unir a la liga pues ésta presenta la cantidad máxima de participantes.
  4. a) No se eligieron suficientes jugadores:
  - El sistema advierte que faltan jugadores por elegir, y vuelve a pedir que elija los jugadores faltantes, avisando que los mismos deben tener un comportamiento asignado.
  4. a) Se eligieron jugadores sin comportamiento asignado:
  - El sistema advierte que se seleccionaron jugadores sin comportamiento asignado, y cancela la acción.

### Caso de Uso 33: Abandonar liga
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y se encuentra inscripto en una liga.
- **Escenario exitoso principal:**
  1. El usuario ingresa a los detalles de una liga en la que participa y la abandona.
  2. El sistema solicita confirmación para darse de baja.
  3. El usuario confirma la operación.
  4. El sistema valida que la liga se encuentre en estado "No iniciada" y retira al club del torneo.
- **Escenarios excepcionales:**
  4. a) La liga ya comenzó o está en curso:
  - El sistema informa que no es posible abandonar una competencia iniciada y bloquea la acción.

### Caso de Uso 34: Cancelar liga
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y tiene al menos una liga creada.
- **Escenario exitoso principal:**
  1. El usuario cancela una de sus ligas creadas.
  2. El sistema valida la acción y solicita confirmación para cancelar la liga.
  3. El usuario confirma la cancelación.
  4. El sistema informa a los usuarios participantes que la liga fue cancelada, los retira de la misma y elimina la liga de la base de datos.
- **Escenarios excepcionales:**
  2. a) La liga ya comenzó:
  - El sistema informa que no es posible eliminar una liga ya iniciada y bloquea la acción.

### Caso de Uso 35: Iniciar liga
- **Actor primario:** Usuario
- **Precondición:** El usuario es dueño de al menos una liga, y está viendo sus ligas creadas.
- **Escenario exitoso principal:**
  1. El usuario ingresa a los detalles de una liga y selecciona la opción iniciar.
  2. El sistema valida la elección, solicita confirmación para iniciar la liga informando la cantidad de participantes actuales.
  3. El usuario confirma la operación.
  4. El sistema cambia el estado de la liga a "Iniciada", calcula el fixture con el orden de los partidos a realizarse, y se actualiza la liga en la base de datos.
- **Escenarios excepcionales:**
  2. a) La liga no supera la cantidad mínima de jugadores:
  - El sistema informa que la cantidad de participantes es insuficiente y cancela la operación, volviendo a mostrar la lista de ligas creadas.
  3. a) El usuario cancela la confirmación:
  - El sistema vuelve a mostrar las ligas creadas por el usuario.

### Caso de Uso 36: Consultar fixture
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y es miembro de al menos una liga.
- **Escenario exitoso principal:**
  1. El usuario ingresa a los detalles de una liga en la que participa y consulta el fixture.
  2. El sistema valida que la liga esté iniciada, solicita la información y muestra el fixture.
- **Escenarios excepcionales:**
  2. a) La liga no comenzó:
  - El sistema informa que no es posible visualizar el fixture de una competencia no iniciada y bloquea la acción.

### Caso de Uso 37: Consultar tabla de posiciones
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado y es miembro de al menos una liga.
- **Escenario exitoso principal:**
  1. El usuario selecciona la tabla de posiciones de una liga a la pertenece.
  2. El sistema valida que la liga esté iniciada, solicita la información y muestra la tabla de posiciones.
- **Escenarios excepcionales:**
  2. a) La liga no comenzó:
  - El sistema informa que no es posible visualizar la tabla de posiciones de una competencia no iniciada y bloquea la acción.

## Ranking

### Caso de Uso 38: Consultar ranking global
- **Actor primario:** Usuario
- **Precondición:** El usuario está logueado.
- **Escenario exitoso principal:**
  1. El usuario selecciona ver el ranking global.
  2. El sistema solicita la información y actualiza la vista del usuario mostrando el ranking.