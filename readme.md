      # SASS (Syntactically Awesome Stylesheet)
      Preprocesador de CSS que permite generar de manera automática hojas de estilo añadiendole caracteristicas
      que no tiene CSS como lo son VARIABLES, FUNCIONES, HERENCIA, entre otros.

      SASS toma el codigo programado en .scss  y lo convierten luego de una compilacion a .css. Generando las siguientes
      ventajas.

      1. Reduce el tiempo para crear y mantener CSS.
      2. Permite la organizacion modular
      3. Proporciona estructuras de programacion. (listas, variables, funciones, etc)
      4. Permite la salida automatica de css con modo watch.

      Existe una extension dentro de VSC que me ayuda con SASS y se llama Live SASS Compiler

      ¿Si no uso una extensión de CSS como puedo trabajarlo?
      
      Lo puedes utilzar con Node, para ello necesito instalar Node mediante el empleo del siguiente comando por terminarl
      
          npm install -g sass  
      
      Si utiliza MAC o Linux con Homebrew puede realizarse de la siguiente manera:
         
          brew install sass/sass/sass
      
      ¿ Cómo compilo mis archivos en sass?

          sass -watch archivo_compilar.scss  destino_compilacion
          sass -watch style.scss     /css/style.css

      ¿ Qué pasos debería realizar para pasar a sass un sitio en con una hoja estandar de css ?
        
        1. Renombra por ejemplo style.css => style.scss (Normalmente se lo denomina al archivo principal app.scss)
        2. Modulariza el estilo => divide la hoja en varios archivos modulares que sean atómicos y sus nombres deben
           comenzar con guión bajo => _menu.scss
        3. Importa los desde el archivo principal 
        4. Compila

      ¿ Cómo debo estructurarme?
      
        Una forma de estructurarse es crear el directorio scss/ dentro de la raiz creamos el directorio style.scss y en 
    este mismo directorio podemos seguir separando nuestros modulos en subcarpetas por ejemplo
        scss/componentes  => _menu.scss

        Luego en el archivo principal a travez de @import podre importar estos partials


    Forwards: 
    ----------------------------------------------------------------------------------------------------------------------
    Cuando hago uso de variables o mixin debo utilizar la regla @forward. 

    Nesting
    ----------------------------------------------------------------------------------------------------------------------
    Al igual que con HTML los estilos con sass pueden anidarse en forma de árbol

    Ejemplo 
           nav {            HTML        <nav>
               ul{                          <ul>
                   li{                         <li>
                            =>                   Contenido
                   }                            </li>
               }                            </ul>
           }                            </nav>

    Partials:
    ---------------------------------------------------------------------------------------------------------------------------
    Es la forma que tengo de modularizar. Son porciones o fragmentos de estilo que luego puedo incluir en el archivo principal
    deben ser denominados con guion bajo para que sass sepa que se trata de archivos parciales. Y se hace referencia a ellos
    a través de @use (no @import)

    ¿Por qué no import?
    
    1. Import hace que todas las variables, mixins, etc tengan acceso global. Esto que hace que sea muy dificil dar un acceso 
    restringido.

    2. Al ser global las librerias deberian tener un prefijo en su nombre siempre para evitar posibles colisiones 
    3. etc.

    https://sass-lang.com/documentation/at-rules/import


    Mixin:
    ---------------------------------------------------------------------------------------------------------------------------            
    A través del mixin puede darle a mi preprocesador un comportamiento analogo a lo que es una funcion en programacion. Es decir,
    puedo crear una funcion que dada una serie de parametros cree un estilo de forma dinamica. Para lograr esto debo seguir los
    siguientes pasos

    1. Crear un mixin por medio de @mixin. Como podemos ver en el ejemplo a continuacion
           
           @mixin  nombre_funcion (parametro1, parametro2, n) {
               Declaraciones.....
           }

           @mixin colorBoton ($color){
              background-color: $color;  
           }

    2. Una vez creado el mixin debemos utilizarlo, para ello, empleamos dentro del selector @include 
          
          selector{
              @include nombre_funcion(parametros)
          }

          .button__send {
                padding: 1rem;
                width: 100px;
                height: 40px;
                @include colorBoton(crimson);
          }

    Herencia:
    --------------------------------------------------------------------------------------------------------------------------- 
    Dentro de la programación orientada a objetos, la herencia es uno de sus pilares mas importantes. El concepto de herencia
    puede analizarse de la siguiente manera, uno analiza objetos de la realidad e identifica que muchos de ellos pertenecen
    a ciertos grupos o posee ciertas cualidades generales, de esta forma abstraen el comportamiento genreal para que los hijos 
    lo hereden. 

    Para hacer uso de herencia en SASS implemento la directiva @extend es decir que el selector que extienda es quien comparte
    una serie de propiedaes visuales de css con su padre

    Para esto necesitamos tener por un lado lo que se denomina en programacion una super clase o padre y clases hijas o hijos

    En SASS definimos el comportamiento comun (padre) de la siguiente forma::after

    %nombre-super-clase{
        Declaraciones
    }

    Por ejemplo todos mis botones son del mismo tamaño y solo se modifica el color

    %boton-general{
        padding: 1rem;
        font-size: 1.25rem;
        color: #fff;
        width: 100px;
        height:50px;
        margin: 10px;
    }

    De esta clase general me encuentro con dos hijos. Boton rojo y verde, siendo estos quienes utilzaran la directiva extend

    .button--red {
        background-color: red;
        @extend %boton-general;
    }
      
    .button--green {
        background-color: green;
        @extend %boton-general;
    }

    Operadores:
    --------------------------------------------------------------------------------------------------------------------------- 
    En SASS puedo utilizar operadores logicos + - / * %  




    Funcion debug y Tipos de datos y Estructuras Iterativas:
    --------------------------------------------------------------------------------------------------------------------------- 
    Los datos que soporta son numéricos, definicion de reglas en general de css, variables de tipo map


    @debug  lo_que_quiero_imprimir

    @debug  $variable
    @debug  type-of()  Retorna tipo de dato de la variable


    @for $i from 1 through 20 {
        //utilizo el valor de la variable a traves de #($i)
        .cols-#{$i * 5}{
            width: $i * 5%;
        }    

        
    }   
    
    //map con media
    $bp: (
            m: 640px;
            l: 1024px;
            xl: 1240px;
        );
        @media screen and (min-width: map-get($bp, m)){
            @for $i from 1 through 20 {
                .cols-m-#{$i * 5}{
                    width: $i * 5%;
                }   
            }
        }

        @media screen and (min-width: map-get($bp, l)){
            @for $i from 1 through 20 {
                .cols-l-#{$i * 5}{
                    width: $i * 5%;
                }   
            }
        }
        
    }   