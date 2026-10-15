# DECKSMITHS
## MIEMBROS DEL GRUPO:

    1.ÁLVARO AVILÉS VALENZUELA

    2.ÁNGEL RAMÍREZ VEGA

    3.JAVIER CASTILLO IGLESIAS

    4.ROBERTO FRITSCHI LUACES

## 1.Introducción al problema:
   
Magic:The Gathering es un juego de cartas coleccionables con decenas de miles de cartas editadas. Los jugadores invierten mucho tiempo y dinero en construir sus colecciones, y la creación de mazos (deckbuilding) es una de las partes más complejas y atractivas del juego, requiriendo un profundo conocimiento de los complementos o relación  entre cartas. 

La realidad de la mayoría de los jugadores, quienes invierten un volumen enorme de tiempo y dinero en construir sus colecciones físicas o digitales, es que sus cartas terminan acumulándose en carpetas, cajas de cartón o fundas tras años de partidas. Trasladar y digitalizar todo ese material a una herramienta informática suele convertirse en un proceso manual, pesado y eternamente postergado. La consecuencia directa de esta falta de control sobre el propio inventario es un problema de análisis combinatorio: saber qué mazos óptimos o verdaderamente competitivos puedes construir únicamente con las cartas que ya tienes en casa es casi imposible sin ayuda tecnológica. Esto lleva de forma inevitable a que los jugadores gasten dinero de más comprando cartas en el mercado secundario creyendo que las necesitan, cuando en realidad guardan alternativas funcionales en sus propios archivadores, o a que acaben compitiendo con mazos subóptimos por no detectar las combinaciones estratégicas ocultas entre sus pertenencias.

Para resolver de raíz esta problemática, el sistema se plantea como una plataforma ágil donde el usuario pueda registrar y actualizar su colección sin fricciones, ofreciendo como gran valor diferencial un motor de generación automática de mazos enfocado de manera estricta en su inventario real. A diferencia de los buscadores tradicionales que sugieren mazos ideales partiendo del supuesto de que el jugador posee todas las cartas del mercado, este motor toma la colección del usuario como el límite de búsqueda para analizar las cartas disponibles, calcular la base de tierras necesaria y construir de forma automática listas totalmente legales y cargadas de sinergia para distintos formatos de juego, como la competitividad de Standard o la versatilidad de Commander, logrando que el jugador aproveche al máximo su inversión y devuelva a la mesa de juego el valor de su colección.

## 2.Glosario de términos
        
Carta: Unidad básica del juego. Tiene atributos como coste de maná, color, tipo (criatura, instantáneo, conjuro, tierra) y reglas de texto.
    
![Carta](fotoCarta.png)


Colección: Es el conjunto de cartas de Magic que posee un usuario registrado en el sistema.

Color (Maná): Es el sistema de recursos o "hablidades por categorías" del juego (Blanco, Azul, Negro, Rojo, Verde e Incoloro). Define la identidad de las cartas y los mazos.

Formato: Es el conjunto de reglas que define qué cartas son legales para jugar y cómo se construye el mazo (ya sea Standard o  Commander).

Mazo (Deck):Es el conjunto de cartas seleccionado por un jugador para disputar una partida. Debe cumplir las restricciones del formato elegido.



![Mazo](fotoMazo.png)
Criatura: Son los monstruos, soldados o bestias que invocas para luchar por ti, son cartas
Conjuro: Es un  hechizo puntual o un habilidad de una carta de gran poder (ejemplo: lanzar un rayo )
Cementerio:Es el lugar donde se envía un conjuro tras ser usado o las cartas eliminadas en combate al jugar una partida

## 3.Visión general del sistema
        
####Requisitos generales:

 -Gestión Privada de Usuarios y Seguridad de Sesiones

El sistema debe permitir que los usuarios se registren, inicien sesión y accedan de forma segura. Cada jugador debe poder manejar su información, su colección personal y sus mazos. Todo debe estar protegido y no accesible para otros usuarios. Esto se logra con el control de sesiones en el servidor.

- Digitalización y Administración del Inventario Personal

El sistema debe ofrecer herramientas para que los usuarios gestionen completamente su colección de cartas. Estas herramientas incluyen alta, edición y eliminación de elementos. También debe permitir la importación masiva de datos. Además, debe tener un sistema de búsqueda con filtros avanzados para revisar el stock real.

-Motor Automático de Generación, Ajuste y Análisis de Mazos

El sistema debe crear mazos de forma automática según el formato de juego y la sinergia que el jugador desee. Debe calcular el número necesario de tierras y permitir que el usuario revise, cambie, exporte y analice gráficamente las listas generadas.

-Control de Reglas, Administración del Catálogo y Sincronización Externa

El servidor debe asegurar que se cumplan las reglas del juego, como los límites de copias, el tamaño de los mazos, las restricciones de color y la disponibilidad real en el inventario. También debe dar a los administradores herramientas para gestionar el catálogo global, actualizar los formatos y mantener la base de datos sincronizada con servicios externos.


Usuarios del sistema:

  -Jugador: Usuario registrado que sube su colección, consulta cartas y genera mazos.

  -Administrador: Usuario encargado de actualizar la base de datos maestra con las nuevas expansiones de cartas de Magic y de gestionar a los usuarios.

## 4.Catálogo de requisitos
        
  ### Requisitos de información (RI):

  RI-01 Carta: Identificador, nombre, coste de maná, colores, tipo, subtipo, rareza, texto de reglas, expansión, imagen.

  RI-02 Colección (Inventario): Cantidad disponible de cada carta (RI-01) asociada a un Usuario (RI-04).

  RI-03 Mazo: Identificador, nombre, formato, fecha de creación, lista de cartas principales (Mainboard), lista de banquillo (Sideboard).

  RI-04 Usuario: Identificador, nombre de usuario, correo electrónico, contraseña cifrada, fecha de registro.

  ### Reglas de negocio (RN):

  RN-01 Tamaño mínimo de mazo: Todo mazo generado o validado dentro del sistema para formatos tradicionales (como Standard o Commander) debe contener un total mínimo de 60 cartas. Esta regla actúa como un filtro de completitud e integridad: el sistema no considerará válido ni guardará un mazo en estado "Completado" si el conteo total de sus cartas es inferior a 60

  RN-02 Límite de copias: Un mazo no puede incluir más de cuatro copias de una misma carta en total. La carta se identifica estrictamente por su nombre oficial en inglés (nombre_ingles). La única excepción a esta regla son las cartas que tengan el supertipo “Tierra básica” (Basic Land, como Llanura, Isla, Pantano, Montaña o Bosque). Estas cartas se pueden incluir en cantidad ilimitada dentro del mazo, siempre que el inventario del usuario lo permita.

  RN-03 Restricción de inventario: El generador de mazos funciona dentro de un espacio limitado por la Colección Personal del usuario . No se pueden incluir cartas que el usuario no tenga, ni se permiten más copias de las que estén registradas en su inventario. El sistema revisa en tiempo real cuántas copias de cada carta tiene el usuario  antes de agregarla al mazo. Así se asegura que todos los mazos se creen con base en lo que realmente posee el usuario.
       
   RN-04 Límite de colores por carta: Una carta puede tener entre 0 (incolora) y un máximo de 5 colores asociados.Una carta con 0 colores asociados se clasifica como Incolora (Colorless, como la mayoría de los artefactos y tierras), mientras que una carta con 5 colores asociados se clasifica como Pentacolor (posee los colores Blanco, Azul, Negro, Rojo y Verde).

  RN-05 Restricción de color para Tierras Básicas: Si una carta es del subtipo Tierra y su atributo esBasica es verdadero, entonces debe tener asociado exactamente 1 único color. 

 ### Pruebas de aceptación de reglas de negocio
          

-Prueba para RN-02 (Límite de copias):

  Condición inicial: El usuario tiene 6 copias de la carta "Relámpago" en su colección.

  Acción: El sistema intenta generar un mazo rojo e incluir las 6 copias.

  Resultado esperado: El sistema incluye solo 4 copias de "Relámpago" en el mazo y busca otras alternativas para rellenar el resto del mazo.

  -Prueba para RN-03 (Restricción de inventario):

  Condición inicial: El usuario tiene 0 copias de "Tarmogoyf".

  Acción: Se solicita la generación de un mazo verde de formato Modern.

Resultado esperado: El mazo generado no incluye "Tarmogoyf" en su lista bajo ninguna circunstancia, utilizando solo las cartas disponibles en el inventario del usuario.


### Requisitos funcionales (Solo listados y consultas)

  RF-01 Consultar catálogo de cartas: El sistema debe permitir abrir la ficha completa de cualquier carta de la base de datos central de Magic. En esta ficha se mostrarán todos sus datos oficiales: nombre en español e inglés, coste de maná, línea de tipo, texto de reglas, ilustración, edición a la que pertenece y los formatos en los que es legal jugar.

  
  RF-02 Listar colección del usuario: El sistema debe mostrar un listado paginado con todas las cartas que el usuario ha añadido a su inventario personal. Esta vista debe permitir organizar y filtrar el inventario por color, tipo de carta, rareza o coste de maná, mostrando claramente la cantidad de copias que posee el usuario de cada carta.

  RF-03 Listar mazos generados:El sistema debe permitir abrir un mazo específico y revisar en detalle las cartas que lo forman. La lista debe presentarse organizada por categorías (criaturas, hechizos, tierras, etc.) y por zonas (mazo principal, banquillo o comandante), indicando cuántas copias hay de cada carta.

  RF-04 Consultar composición de mazo: El sistema debe permitir abrir un mazo específico y revisar en detalle las cartas que lo forman. La lista debe presentarse organizada por categorías (criaturas, hechizos, tierras, etc.) y por zonas (mazo principal, banquillo o comandante), indicando cuántas copias hay de cada carta.

   RF-05 Buscar cartas en la base de datos maestra: El sistema debe permitir buscar cartas en el catálogo global usando filtros combinados por nombre, texto de reglas, coste de maná convertido, formato de juego permitido y edición.

  RF-06 Consultar mazos viables según colección (Motor de Sugerencias):El sistema debe incluir un motor de sugerencias que analice las cartas que el usuario tiene en su inventario y las compare con listas de mazos del juego. El sistema mostrará una lista de mazos sugeridos ordenados según el porcentaje de cartas que el usuario ya posee, desde mazos listos para armar al 100% hasta los que requieran un mínimo de cartas adicionales.

  RF-07 Consultar faltantes y coste de adquisición: Para aquellos mazos que el usuario no tenga completos, el sistema debe generar una lista con las cartas concretas que le faltan para terminarlo y la cantidad exacta de cada una. Además, mostrará una estimación del dinero total que costaría comprarlas según los precios actuales de mercado.

  RF-08 Consultar métricas y análisis del mazo:  El sistema debe generar un análisis visual e intuitivo de cualquier mazo seleccionado para comprobar si está bien equilibrado. Esto incluye gráficos con la curva de maná (cuántas cartas hay de cada coste), la proporción entre los colores del mazo y las tierras disponibles para producirlos, y el coste medio del mazo.

RF-09 Privatización de mazos:Garantizar la privacidad de los mazos,los mazos creados o guardados por un usuario son privados. El sistema impedirá que otros usuarios o visitantes de la plataforma puedan ver, modificar o acceder a las listas de mazos que no les pertenezcan.

RF-10 Consultar historial de modificaciones y versiones del mazo: : El sistema debe guardar un registro automático cada vez que el usuario modifique un mazo. El usuario podrá consultar la línea del tiempo de su mazo y comparar dos versiones para ver resaltadas las cartas que ha ido añadiendo, quitando o cambiando de cantidad.


   
        

![Modelo](fotoModelo.png)
