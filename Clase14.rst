.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 14 - POO 2018
===================
(Fecha: 7 de mayo)
		
	
Funciones virtuales
^^^^^^^^^^^^^^^^^^^

- Puede ser interesante llamar a la función de la derivada (en polimorfismo).
- Al declarar una función como virtual en la clase base, si se superpone en la derivada, al invocar usando el puntero a la clase base, se ejecuta la versión de la derivada.

.. code-block:: c

	class Persona  {
	public:
	    Persona(QString nombre) : nombre(nombre)  {  }
	    virtual QString verNombre()  {  return "Persona: " + nombre;  }  // Y si no fuera virtual?

	protected:  
	    QString nombre;
	};

	class Empleado : public Persona  {
	public:
	    Empleado(QString nombre) : Persona(nombre)  {  }
	    QString verNombre()  {  return "Empleado: " + nombre;  }
	};


	#include <QApplication>
	#include "personal.h"
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    {
	    Persona *carlos = new Empleado("Carlos");

	    qDebug() << carlos->verNombre();  // Qué publica?

	    delete carlos;
	    }

	    return a.exec();
	}




Función virtual pura y clase abstracta
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- No necesita ser definida, sólo se declara.
- Será definida en las clases derivadas

.. code-block:: c

	virtual void verValor(int a) = 0;

- Algunos pueden decir que no es muy elegante igualar a cero una función:

.. code-block:: c

	#define abstracta =0

	// entonces podemos usar:
	virtual void verValor(int a) abstracta;

- Una clase con al menos una función virtual pura la convierte en clase abstracta.
- Una clase abstracta no puede ser instanciada.
- Si en la clase derivada no se define la función virtual pura, significa que esta clase derivada también es abstracta.

.. code-block:: c

	#define abstracta =0

	class Persona  {
	public:
	    Persona(QString nombre) : nombre(nombre)  {  }
	    virtual QString verNombre() abstracta;

	protected:  
	    QString nombre;
	};

	class Empleado : public Persona  {
	public:
	    Empleado(QString nombre) : Persona(nombre)  {  }
	    QString verNombre()  {  return "Empleado: " + nombre;  }
	};

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    {
	    Persona *carlos = new Empleado("Carlos");

	    qDebug() << carlos->verNombre();

	    delete carlos;
	    }

	    return a.exec();
	}


**Ejercicio 9**

- Crear el siguiente método dentro de la clase AdminDB:

.. code-block:: c	
	
	/**
	 * @brief Método que ejecuta una consulta SQL a la base de datos que ya se encuentra conectado. 
	          Utiliza QSqlQuery para ejecutar la consulta, con el método next() se van extrayendo 
	          los registros que pueden ser analizados con QSqlRecord para conocer la cantidad de 
	          campos por registro.
	 * @param comando es una consulta como la siguiente: SELECT nombre, apellido, id FROM usuarios
	 * @return Devuelve un QVector donde cada elemento es un registro, donde cada uno de estos registros 
	           están almacenados en un QStringList que contiene cada campo de cada registro.	           
	 */
	QVector<QStringList> select(QString comando); 


**Ejercicio 10**

- Diseñar una aplicación para una galería de fotos
- Debe tener una base con una tabla 'imagenes' que tenga las URLs de imágenes
- Un botón >> y otro << para avanzar o retroceder en la galería de fotos
- Se podrá navegar sobre las fotos que se descargarán desde internet
	
	
**Para independizar del SO**

.. code-block:: c

	AdminDB adminDB;
	QString nombreSqlite;

	#ifdef __APPLE__
	    nombreSqlite = "/home/cosimani/db/test";
	#elif __WIN32__
	    nombreSqlite = "C:/Qt/db/test";
	#elif __linux__
	    nombreSqlite = "/home/cosimani/db/test";
	#else
	    nombreSqlite = "/home/cosimani/db/test";
	#endif

	if (adminDB.conectar(nombreSqlite))
	    qDebug() << "Conexion exitosa";




Métodos virtuales de QWidget para capturar eventos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Algunos de ellos:

.. code-block:: c

	virtual void mouseDoubleClickEvent(QMouseEvent* event);
	virtual void mouseMoveEvent(QMouseEvent* event);
	virtual void mousePressEvent(QMouseEvent* event);
	virtual void keyPressEvent(QKeyEvent* event);
	virtual void resizeEvent(QResizeEvent* event);
	virtual void moveEvent(QMoveEvent* event);
	virtual void closeEvent(QCloseEvent* event);

- Estos métodos pueden ser reimplementados en una clase derivada para recibir los eventos.

**Algunos argentinos que también explican como los mexicanos** 

- Clic sobre los GIF para abrir los videos 

**Crear base de datos**

|ImageLink|_ 

.. |ImageLink| image:: /images/clase12/crearBase.gif
.. _ImageLink: https://www.youtube.com/watch?v=U9iE6pM0bxM

**Crear tabla**

|ImageLink2|_ 

.. |ImageLink2| image:: /images/clase12/crearTabla.gif
.. _ImageLink2: https://www.youtube.com/watch?v=_-hKca2k784

**Insertar registro**

|ImageLink3|_ 

.. |ImageLink3| image:: /images/clase12/insertarRegistro.gif
.. _ImageLink3: https://www.youtube.com/watch?v=RggFhFZnCPU

**Consultar datos**

|ImageLink4|_ 

.. |ImageLink4| image:: /images/clase12/consultarDatos.gif
.. _ImageLink4: https://www.youtube.com/watch?v=8emd37mvN2E

