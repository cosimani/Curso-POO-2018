.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 10 - POO 2017 (Aún no preparado)
===================



.. figure:: images/clase09/java_vs_cplusplus.gif

- Para crear gifs a partir de videos en youtube en: https://gifs.com 









Clases derivadas
^^^^^^^^^^^^^^^^

.. code-block:: c
 
	// personal.h
	#include <QString>

	class Personal  {
	public:
	    QString verEdad()  {  return "Edad: " + QString::number(edad);  }
	    QString verSalario()  {  return "Salario: " + QString::number(salario);  }

	protected:  // Para acceso desde las clases derivadas
	    int edad;
	    int salario;
	};

	// Modificadores de acceso para Herencia:
	//    public  ->  Mantiene los modificadores de acceso de la clase base
	//    private ->  Pasa todo a privado
	class Desarrollador : public Personal  {
	public:
	    Desarrollador(int edad)  {
	    salario = 2000;
	    this->edad = edad;
	}

	// Se podrá usar? 
	Desarrollador(int edad) : salario(2000), edad(edad)  {  }
	    // No. Sólo para miembros de la propia clase (no para heredados).
	};

	class Administrador : public Personal  {
	public:
	    Administrador()  {
	        salario = 2000;
	        edad = 30;
	    }
	};

	#include <QApplication>
	#include "personal.h"
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    Desarrollador juan(20);
	    Administrador marcos;

	    qDebug() << juan.verEdad();
	    qDebug() << juan.verSalario();

	    qDebug() << marcos.verEdad();
	    qDebug() << marcos.verSalario();

	    return a.exec();
	}

Constructor de la clase derivada
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	class Persona  {
	public:
	    Persona(int edad) : edad(edad)  {  }
	    QString verEdad()  {  return "Edad: " + QString::number(edad);  }
	    void setEdad(int edad)  {  this->edad = edad;  }

	protected:
	    int edad;
	};

	class Empleado : public Persona  {
	public:
	    // Siempre primero se llama al constructor de la clase base
	    Empleado(int edad, int salario) : Persona(edad), salario(salario)  {  }
	    QString verSalario()  {  return "Salario: " + QString::number(salario);  }

	protected:
	    int salario;
	};

	#include <QApplication>
	#include "personal.h"
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    Persona carlos(24);
	    Empleado ale(20, 2500);

	    qDebug() << carlos.verEdad();
	    //    qDebug() << carlos.verSalario();  // No compila. No está en la clase base.

	    qDebug() << ale.verEdad();
	    qDebug() << ale.verSalario();

	    return a.exec();
	}

Destructor de la clase derivada
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	class ClaseA  {
	public:
	    ClaseA() : datoA(10)  {  qDebug() << "Constructor A";  }
	    ~ClaseA()  {  qDebug() << "Destructor A";  }
	    int verA()  {  return datoA;  }

	protected:
	    int datoA;
	};

	class ClaseB : public ClaseA  {
	public:
	    ClaseB() : datoB(20)  {  qDebug() << "Constructor B";  }
	    ~ClaseB()  {  qDebug() << "Destructor B";  }
	    int verB()  {  return datoB;  }

	protected:
	    int datoB;
	};

	#include <QApplication>
	#include "personal.h"
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    {
	    ClaseB objeto;
	    qDebug() << "a=" << objeto.verA() << ", b=" << objeto.verB();
	    }

	    return a.exec();
	}

	// Publica
	Constructor A
	Constructor B
	a=10, b=20
	Destructor B
	Destructor A



Web Service
^^^^^^^^^^^

- Para intercambiar datos entre aplicaciones
- Generalmente a través del protocolo HTTP
- La info puede viajar en XML, JSON, etc.
- Fomenta y facilita el uso y desarrollo de APIs Web
- https://es.wikipedia.org/wiki/Servicio_web

**Algunas APIs disponibles**

- Twitter - https://dev.twitter.com
- Facebook - https://developers.facebook.com
- Amazon - https://developer.amazonservices.es
- Spotify - https://developer.spotify.com/web-api
- MercadoLibre - http://developers.mercadolibre.com
- Google - https://developers.google.com
	- Youtube
	- Traductor
	- Google+
	- Maps
	- Street View
	- MercadoLibre

**Google Maps**

- URL para su uso: https://developers.google.com/maps/documentation/staticmaps
- Ejemplo: http://maps.googleapis.com/maps/api/staticmap?center=rondeau+100+cordoba&zoom=15&size=500x300&maptype=roadmap&sensor=false
- Descripción de los parámetros en: https://developers.google.com/maps/documentation/staticmaps/#URL_Parameters
- Pueden habilitar otros servicios en https://code.google.com/apis/console

**QUrl**

- Para manipular una url ingresada por el usuario 

.. code-block:: c
	
	// URL ejemplo: http://www.yahoo.com.ar/documento/info.html
		
	// El método path() devuelve /documento/info.html
	// El método host() devuelve www.yahoo.com.ar
	
	QUrl url("http://www.yahoo.com.ar/documento/info.html");
	qDebug() << url.host();
	qDebug() << url.path();




Clase QNetworkAccessManager
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Permite enviar y recibir solicitudes a la red
- Se obtiene un objeto ``QNetworkReply`` con toda la información recibida

.. code-block:: c

	QNetworkAccessManager* manager = new QNetworkAccessManager;

	connect(manager, SIGNAL(finished(QNetworkReply*)), this, SLOT(slot_respuesta(QNetworkReply*)));

	manager->get(QNetworkRequest(QUrl("http://mi.ubp.edu.ar")));

- Para poder utilizar las clases de network hay que agregar en el .pro

.. code-block:: c

	QT += network  // Esto agrega al proyecto el módulo network

- Por defecto, el módulo 'gui' y el módulo 'core' están incluidos.
- Para utilizar HTTPS, Qt utiliza OpenSSL https://www.openssl.org/source
	- Posiblemente sea más fácil descargarlo desde https://slproweb.com/products/Win32OpenSSL.html
	- Por ejemplo, para 64 bits elegir `Win64 OpenSSL v1.0.2h <https://slproweb.com/download/Win64OpenSSL-1_0_2h.exe>`_

Clase QIODevice
^^^^^^^^^^^^^^^

.. figure:: images/clase08/qiodevice.png 

- Clase base de los dispositivos de I/O
- Algunos métodos:

.. code-block:: c

	QByteArray readAll()  		 // Lee todos los datos disponibles.
	QByteArray read(qint64 max)  // Lee hasta max datos disponibles.
	QByteArray readLine()  		 // Lee una linea.

Clase QNetworkReply
^^^^^^^^^^^^^^^^^^^

- Contiene los datos y encabezado de una respuesta
- Una vez leídos los datos, ya no quedarán disponibles.
- Para controlar los bytes que se van descargando usar la señal:

.. code-block:: c

	void downloadProgress(qint64 bytesRecibidos, qint64 bytesTotal)

Clase QNetworkRequest
^^^^^^^^^^^^^^^^^^^^^

- Contiene la información que se envían en la petición
- Seteamos algún campo de la cabecera con:

.. code-block:: c

	void setRawHeader(const QByteArray &nombre, const QByteArray & valor)

	QNetworkRequest request;
	request.setUrl(QUrl(ui->le->text()));
	request.setRawHeader("User-Agent", "MiNavegador 1.0");

Clase QNetworkProxyFactory
^^^^^^^^^^^^^^^^^^^^^^^^^^

- Permite configurar un servidor proxy a nuestra aplicación Qt.
- Lo siguiente utiliza la configuración del sistema (Chrome y IE, no Firefox).

.. code-block:: c

	#include <QApplication>
	#include "principal.h"
	#include <QNetworkProxyFactory>

	int main(int argc, char *argv[])  {
	    QApplication a(argc, argv);

	    QNetworkProxyFactory::setUseSystemConfiguration(true);

	    Principal w;
	    w.showMaximized();

	    return a.exec();
	}

Obtener una imagen desde internet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	void Principal::slot_descargaFinalizada(QNetworkReply *reply)  {
	    QImage image = QImage::fromData(reply->readAll());
	}

**Ejercicio 8** 

- Hacer una aplicación para buscar una dirección en Google Maps
- Definir la clase Mapa. Será el QWidget donde se dibujará el mapa de google.
- Definir la clase Ventana para contener al layout.
- Ese layout tendrá:
	- QLineEdit para ingresar un domicilio
	- QPushButton para buscar ese domicilio
	- Mapa
	- QSlider vertical para aumentar y disminuir el zoom





Conexión a base de datos
^^^^^^^^^^^^^^^^^^^^^^^^

**Ejemplo de la estructura de las tablas en la base de datos**

.. figure:: images/clase11/tablas.png 

- Con Qt se pueden utilizar los siguientes motores de base de datos:
	- **ODBC (Open DataBase Connectivity)**: 
		- Estándar de acceso a base de datos
		- Usado con Microsoft Access en Windows
		- Está disponible en Windows: Panel de control -> Herramientas administrativas -> ODBC Data sources
			
	- **SQLite**
		- Es un sistema de gestión de bases de datos relacional.
		- En C y libre
		- Los datos se almacenan en un archivo
		- No es cliente-servidor. La librería (dll) tiene funciones para trabajar
		- No requiere instalación, directamente con un ejecutable
		- Para Linux, Windows, Mac OS, Android, iOS, BlackBerry OS, Windows Phone, ...
		- Algunas aplicaciones que usan SQLite: Skype, Firefox, Photoshop, ...
			
	- **MySQL**
		- Quizás el motor de base de datos más utilizado
		- Usado por los más grandes: Facebook, Twitter, YouTube, Wikipedia, ...
		- Requiere una instalación más avanzada para usar con Qt dependiendo el SO que se utilice.
		
Usando SQLite
^^^^^^^^^^^^^

**Creación de una base de datos SQLite**
	
- Descargar de http://www.sqlite.org/download.html
- Precompiled Binaries for Windows–Linux–MAC (The command-line shell program)
- En Linux se puede hacer: ``sudo apt-get install sqlite3``
- Al descomprimir tenemos el ejecutable sqlite3
- Creamos una carpeta C:/Qt/db (o /home/db) y copiamos ahí el ejecutable
- En consola creamos una base de datos, por ejemplo, llamada ``test`` con una tabla ``usuarios``

::

	sqlite3 test

	create table usuarios (
	    id integer primary key,  (es autoincrementable)
	    usuario varchar(30),
	    clave varchar(30),
	    nombre varchar(50),
	    apellido varchar(50),
	    mail varchar(50)
	);

	// Podemos insertar un registro 

	insert into usuarios (usuario, clave,	nombre, apellido, mail) 
	values ("cgomez", "1234", "Carlos", "Gomez", "cgomez@gmail.com");

	// Podemos ver el contenido de la tabla "usuario":

	select * from usuarios;

	// Para salir de la base:
		
	.exit

En Qt	
^^^^^

- Requiere QT += sql
- Para averiguar los controladores disponibles, usamos el método estático:

.. code-block:: c

	qDebug() << QSqlDatabase::drivers();  // Devuelve un QStringList

- Un objeto QSqlDatabase representa la conexión a la base
- Elegimos el controlador y conectamos:

.. code-block:: c

	QSqlDatabase db = QSqlDatabase::addDatabase("QSQLITE");

	db.setDatabaseName("C:/Qt/db/test"); 
	if (db.open())
	    qDebug() << "Conexión exitosa";
	else
	    qDebug() << "No se pudo abrir la base";

- En Windows, para usar el archivo Access ``C:/db/base.mdb`` se hace lo siguiente:
	
.. code-block:: c
		
	QSqlDatabase db = QSqlDatabase::addDatabase("QODBC");

	db.setDatabaseName("DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};"
	                   "DBQ=C:/db/base.mdb"); 
	if (db.open())
		qDebug() << "Conexión exitosa";

		
		






Polimorfismo
^^^^^^^^^^^^

- Lo utilizamos con punteros.
- Nos permite acceder a objetos de la clase derivada usando un puntero a la clase base.
- Sin embargo, sólo podemos acceder a datos y funciones que existan en la clase base.
- Los datos y funciones propias de la derivada quedan inaccesibles.

.. code-block:: c

	class Persona  {
	public:
	    Persona(QString nombre) : nombre(nombre)  {  }
	    QString verNombre()  {  return "Nombre: " + nombre;  }

	protected:  // Para acceso desde las clases derivadas
	    QString nombre;
	};

	class Empleado : public Persona  {
	public:
	    Empleado(QString nombre) : Persona(nombre)  {  }
	    QString verNombre()  {  return "Empleado: " + nombre;  }
	    void mostrarAlgo()  {  qDebug() << "Algo";  }
	};

	class Estudiante : public Persona  {
	public:
	    Estudiante(QString nombre) : Persona(nombre)  {  }
	    QString verNombre()  {  return "Estudiante: " + nombre;  }
	};


	#include <QApplication>
	#include "personal.h"
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    {
	    Persona *jose = new Estudiante("Jose");
	    Persona *carlos = new Empleado("Carlos");

	    qDebug() << carlos->verNombre();
	    qDebug() << jose->verNombre();
	    carlos->mostrarAlgo();  // Muestra algo? 

	    delete jose;
	    delete carlos;
	    }

	    return a.exec();
	}
	
**Para todo hay un mexicano que también lo explica** 

- Clic sobre el GIF para abrir video 

|ImageLink|_

.. |ImageLink| image:: /images/clase10/explicacion_mexicana.gif
.. _ImageLink: https://www.youtube.com/watch?v=6lIGfzZ4oqo
	
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


**Volviendo a base de datos**

- Definir una clase AdminDB para administrar la base de datos
- Crear el siguiente método:

.. code-block:: c
	
	bool conectar(QString archivoSqlite); 

- En un proyecto nuevo y desde la función main() intentar la conexión.

.. code-block:: c

	// --- adminDB.h ---------------
	#include <QSqlDatabase>
	#include <QString>
	#include <QObject>

	class AdminDB : public QObject  {
	    Q_OBJECT

	public:
	    AdminDB();
	    bool conectar(QString archivoSqlite);
	    QSqlDatabase getDB();

	private:
	    QSqlDatabase db;
	};

	// --- adminDB.cpp ------------
	#include "adminDB.h"

	AdminDB::AdminDB()  {
	    db = QSqlDatabase::addDatabase("QSQLITE");
	}

	bool AdminDB::conectar(QString archivoSqlite)  {
	    db.setDatabaseName(archivoSqlite);

	    if(db.open())
	        return true;

	    return false;
	}

	QSqlDatabase AdminDB::getDB()  {
	    return db;
	}

	// --- main.cpp  ----------------
	#include <QApplication>
	#include "adminDB.h"

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    qDebug() << QDir::currentPath();

	    AdminDB adminDB;
	    if (adminDB.conectar("C:/Qt/db/test"))
	        qDebug() << "Conexion exitosa";
	    else
	        qDebug() << "Conexion NO exitosa";

	return 0;
	}

Consulta a la base de datos
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	QSqlDatabase db = QSqlDatabase::addDatabase("QSQLITE");

	db.setDatabaseName("C:/Qt/db/test"); 

	if (db.open())  {
	    QSqlQuery query = db.exec("SELECT nombre, apellido FROM usuarios");

	    while(query.next())  {
	        qDebug() << query.value(0).toString() << " " << query.value(1).toString();
	    }
	}

**Ejercicio 9**

- Crear el siguiente método dentro de la clase AdminDB:

.. code-block:: c	
	
	/**
	 * @brief Método que ejecuta una consulta SQL a la base de datos que ya se encuentra conectado. 
	          Utiliza QSqlQuery para ejecutar la consulta, con el método next() se van extrayendo los registros 
	          que pueden ser analizados con QSqlRecord para conocer la cantidad de campos por registro.
	 * @param comando es una consulta como la siguiente: SELECT nombre, apellido, id FROM usuarios
	 * @return Devuelve un QVector donde cada elemento es un registro, donde cada uno de estos registros 
	           están almacenados en un QStringList que contiene cada campo de cada registro.	           
	 */
	QVector<QStringList> select(QString comando); 


	





