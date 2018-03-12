.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 16 - POO 2017 (Aún no preparado)
===================

:Tarea para Clase 18:
	Traer el ejercicio de la Clase 15 en un ejecutable.
	
	Enviarlo por mail antes de las 13 horas del día 15 de mayo

const
.....

- Una variable definida como const no podrá ser modificada a lo largo del programa (se crea como sólo lectura)
- Se puede aplicar a cualquier tipo:

.. code-block:: c	

	const float pi = 3.14;
	const peso = 67;	// Si no se indica el tipo entonces es int
					    // Ojo: Sólo en compiladores viejos


const con punteros
^^^^^^^^^^^^^^^^^^

.. code-block:: c	

	int x = 10;
	int* px = &x;		// normal

	const int y = 10;
	int* py = &y;	// El compilador dirá: "invalid conversion from const int*
					// to int*". La inversa sí se permite

	int y = 10;
	const int* py = &y;	// permitido (pero el contenido es de sólo lectura)

	*py = 6;  // No permitido. El contenido apuntado es de sólo lectura


const en parámetros de funciones
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Cuando los parámetros son punteros, decimos que no podrá modificar los objetos referenciados

.. code-block:: c	

	int funcion(const char* ch)

- Lo mismo sucede con referencias

.. code-block:: c	

	int funcion(const char& ch)

const en clases
^^^^^^^^^^^^^^^

.. code-block:: c	

	class ClaseA  {
	    const int i;
	    int x;

	public:
	    int funcion(ClaseA cA, const ClaseA &c)  {
	        cA.x = 1;
	        cA.i = 2;  // No compila. i es de sólo lectura.
	        c.x = 3;  // No compila. El objeto c es de sólo lectura.

	        return cA.x;
	    }
	}; 

.. code-block:: c	

	// A la variable i sólo la puede inicializar el constructor y sólo con la forma:
	ClaseA() : i(8)  {  }   

	// Si en el cuerpo del constructor se hace:
	ClaseA()   { 
	    i = 8;  // Compila? i es de solo lectura o no
	}   

- Aplicado a métodos de una clase no permite modificar ninguna propiedad de la clase

.. code-block:: c	

	class ClaseB  {
	    int x;

	    void funcion(int i) const  {
	        x = x + i;  // Compila?
	    }
	};

API de Google Street
^^^^^^^^^^^^^^^^^^^^

- Permite descargar una vista
- Puede utilizarse con una clave de API para la aplicación
	- Acceder a https://code.google.com/apis/console y loguearse
	- Administración de las API - Google Street View Image API
	- Habilitar el servicio
	- Credenciales - Crear credenciales - Clave de Servidor o Clave de navegador

- Parámetros de la URL:
	- https://developers.google.com/maps/documentation/streetview

- Parámetros obligatorios
	- size - Imagen en píxeles. Por ejemplo, ``size=600x400`` (máximo 640x640)
	- location - Texto (universidad blas pascal) o lat./long. (40.457375,-80.009353)
	- sensor - Si el dispositivo dispone de GPS "true" o "false"

- Ejemplo: http://maps.googleapis.com/maps/api/streetview?size=400x400&location=donato%20alvarez%20380&sensor=false

- Opcionales:
	- heading - Rotación entre 0 y 360 (heading=45)
	- fov (field of view) - zoom (aprox. entre 10 y 120 - valor predeterminado 90)
	- pitch - Ángulo de inclinación (predeterminado 0 - entre -90 y 90)
	- key: Clave de API (ver https://code.google.com/apis/console)

**Ejercicio 16**

- Con la misma idea que la clase Mapa, hacer ahora la clase ``StreetView``. 
- En un QLineEdit ingresar el domicilio a buscar.
- Con sólo movimientos del mouse horizontales, girar la rotación entre 0 y 360.

**Ejercicio 17**

- Agregar a ``StreetView`` lo siguiente:
- Agregar un QSlider para controlar el zoom.
- Además del QSlider, controla el zoom con dobleclic derecho para aumentarlo y con el izquierdo para disminuirlo.
- Actualizar también la posición del QSlider luego de los dobleclics.
- Almacenar todas las direcciones buscadas en la tabla ``logs`` de la base de datos		
	








