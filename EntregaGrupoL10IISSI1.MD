    PRIMERA ENTREGA GRUPO 10L
MIEMBROS DEL GRUPO:

    1.ÁLVARO AVILÉS VALENZUELA

    2.ÁNGEL RAMÍREZ VEGA

    3.JAVIER CASTILLO IGLESIAS

    4.ROBERTO FRITSCHI LUACES

1.Introducción al problema:
   
    -The Gathering es un juego de cartas coleccionables con decenas de miles de cartas editadas. Los jugadores invierten mucho tiempo y dinero en construir sus colecciones, y la creación de mazos (deckbuilding) es una de las partes más complejas y atractivas del juego, requiriendo un profundo conocimiento de las sinergias entre cartas. 

    -El sistema está dirigido principalmente a jugadores de Magic (desde principiantes hasta competitivos) que poseen colecciones físicas o digitales y necesitan optimizar su uso.

    -Actualmente, los jugadores guardan sus cartas en archivadores o cajas, y digitalizar esta colección suele ser un proceso manual y tedioso. Además, saber qué mazos óptimos o competitivos se pueden construir exclusivamente con las cartas que uno ya posee es casi imposible sin herramientas avanzadas, lo que lleva a los jugadores a gastar dinero en cartas innecesarias o a construir mazos subóptimos.

    -Se espera desarrollar una plataforma donde el usuario pueda subir y registrar su colección de cartas de forma ágil. El valor diferencial del sistema será un motor capaz de analizar esa colección y generar o sugerir automáticamente mazos legales y sinérgicos para distintos formatos de juego (Standard o  Commander).

2.Glosario de términos
        
    Carta: Unidad básica del juego. Tiene atributos como coste de maná, color, tipo (criatura, instantáneo, conjuro, tierra) y reglas de texto.
    
![Carta](fotoCarta.png)



    Colección: Conjunto de cartas de Magic que posee un usuario registrado en el sistema.

    Color (Maná): Sistema de recursos del juego (Blanco, Azul, Negro, Rojo, Verde e Incoloro). Define la identidad de las cartas y los mazos.

    Formato: Conjunto de reglas que define qué cartas son legales para jugar y cómo se construye el mazo (ya sea Standard o  Commander).

    Mazo (Deck): Conjunto de cartas seleccionado por un jugador para disputar una partida. Debe cumplir las restricciones del formato elegido.


![Mazo](fotoMazo.png)

3.Visión general del sistema
        
    Requisitos generales: El sistema debe permitir el registro de usuarios, la gestión (subida, edición y borrado) de las cartas que conforman su colección personal, y la generación automática de mazos basados en dicha colección aplicando reglas de formato y sinergias predefinidas.

    Usuarios del sistema:

    -Jugador: Usuario registrado que sube su colección, consulta cartas y genera mazos.

    -Administrador: Usuario encargado de actualizar la base de datos maestra con las nuevas expansiones de cartas de Magic y de gestionar a los usuarios.

4.Catálogo de requisitos
        
    Requisitos de información (RI):

        RI-01 Carta: Identificador, nombre, coste de maná, colores, tipo, subtipo, rareza, texto de reglas, expansión, imagen.

        RI-02 Colección (Inventario): Cantidad disponible de cada carta (RI-01) asociada a un Usuario (RI-04).

        RI-03 Mazo: Identificador, nombre, formato, fecha de creación, lista de cartas principales (Mainboard), lista de banquillo (Sideboard).

        RI-04 Usuario: Identificador, nombre de usuario, correo electrónico, contraseña cifrada, fecha de registro.

    -Reglas de negocio (RN):

        RN-01 Tamaño mínimo de mazo: Un mazo de formatos tradicionales (como Standard o Commander) debe contener un mínimo de 60 cartas.

        RN-02 Límite de copias: Un mazo no puede contener más de 4 copias de una misma carta (basado en el nombre en inglés), a excepción de las cartas con el supertipo "Tierra básica".

       RN-03 Restricción de inventario: El generador automático de mazos solo podrá incluir cartas y cantidades que existan actualmente en la Colección (RI-02) del usuario.
       
        RN-04 Límite de colores por carta: Una carta puede tener entre 0 (incolora) y un máximo de 5 colores asociados. (Esto viene determinado por la cardinalidad 0..5 en la relación entre Carta y Color en la parte superior izquierda del diagrama).   

        RN-05 Restricción de color para Tierras Básicas: Si una carta es del subtipo Tierra y su atributo esBasica es verdadero, entonces debe tener asociado exactamente 1 único color. (Esto aparece en la nota inferior que apunta a la clase Tierra: "Si Basica exactamente 1 color").  





    -Requisitos funcionales (Solo listados y consultas)

        RF-01 Consultar catálogo de cartas: El sistema debe permitir consultar los detalles completos (RI-01) de cualquier carta de la base de datos maestra.

        RF-02 Listar colección del usuario: El sistema debe mostrar un listado paginado y filtrable (por color, tipo, rareza) de las cartas que el usuario ha subido a su inventario.

        RF-03 Listar mazos generados: El sistema debe mostrar un listado con los mazos que el usuario ha generado o guardado, mostrando el nombre, los colores y el formato.

        RF-04 Consultar composición de mazo: El sistema debe permitir ver el desglose detallado de las cartas que componen un mazo específico, separadas por tipos (criaturas, hechizos, tierras).

    RF-05 Buscar cartas en la base de datos maestra: El sistema debe permitir buscar cartas en el catálogo global usando filtros combinados por nombre, texto de reglas, coste de maná convertido, formato de juego permitido y edición.

    RF-06 Consultar mazos viables según colección (Motor de Sugerencias): El sistema debe permitir consultar un listado de mazos legales y competitivos que el usuario puede construir directamente con su inventario actual, ordenados por porcentaje de cartas disponibles, desde 100% construible hasta un umbral configurable.

    RF-07 Consultar faltantes y coste de adquisición: El sistema debe permitir listar las cartas concretas que faltan al usuario para completar un mazo sugerido o seleccionado, junto con una estimación del coste económico total de adquisición basado en precios de mercado actualizados (Cardmarket / TCGPlayer).

    RF-08 Consultar métricas y análisis del mazo: El sistema debe mostrar consultas analíticas de un mazo seleccionado, incluyendo la curva de maná (distribución por CMC), proporción de fuentes de maná frente a devoción requerida y métricas del motor.

    RF-09 Privatización de mazos: El mazo de un usuario es privado de forma que no es accesible por otros usuarios.

    RF-10 Consultar historial de modificaciones y versiones del mazo: El sistema debe mostrar un registro cronológico de las revisiones de un mazo, permitiendo consultar las diferencias (diff) de cartas añadidas o retiradas entre versiones.



    -Pruebas de aceptación de reglas de negocio

        -Prueba para RN-02 (Límite de copias):

            Condición inicial: El usuario tiene 6 copias de la carta "Relámpago" en su colección.

            Acción: El sistema intenta generar un mazo rojo e incluir las 6 copias.

            Resultado esperado: El sistema incluye solo 4 copias de "Relámpago" en el mazo y busca otras alternativas para rellenar el resto del mazo.

        -Prueba para RN-03 (Restricción de inventario):

        Condición inicial: El usuario tiene 0 copias de "Tarmogoyf".

            Acción: Se solicita la generación de un mazo verde de formato Modern.

            Resultado esperado: El mazo generado no incluye "Tarmogoyf" en su lista bajo ninguna circunstancia, utilizando solo las cartas disponibles en el inventario del usuario.
        

![Modelo](fotoModelo.png)
