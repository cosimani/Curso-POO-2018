.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 15 - POO 2018
===================
(Fecha: 8 de mayo)



Clase QCryptographicHash
^^^^^^^^^^^^^^^^^^^^^^^^

- Provee la generación de la clave hash 
- Soporta MD5, MD4 y SHA-1

.. code-block:: c

	enum Algorithm { Md4, Md5, Sha1 }

	QCryptographicHash(Algorithm metodo)

	void addData(const QByteArray & data)
	
	void reset()

	QByteArray result() const

**Método estático**

.. code-block:: c

	QByteArray hash(const QByteArray & data, Algorithm metodo)

**Otros métodos útiles**

.. code-block:: c

	QByteArray QByteArray::toHex()
	// Devuelve en hexadecimal
	// Útil para enviar por url una clave hash MD5
	// Hexadecimal tiene sólo caracteres válidos para URL

**Ejemplo**: Obtener MD5 de la clave ingresada en un QlineEdit:

.. code-block:: c

	QcryptographicHash::hash(leClave->text().toUtf8(), QCryptographicHash::Md5).toHex()
	
**Calculadora MD5 online**

http://md5calculator.chromefans.org/?langid=es

Registrar eventos (logs)
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	bool AdminDB::insertLog(QString log)  {
	    QSqlQuery query(db);

	    return query.exec("INSERT INTO logs (evento) VALUES ('" + log + "')");
	}

**Ejercicio 13**

- Diseñar una aplicación con un login inicial que valide contra la base
- Almacenar sólo el hash en MD5 de las contraseñas
- Si el usuario es válido mostrar cualquier widget ya creado (Maps, Imagen, paint)
- Registrar en la tabla 'logs' los intentos fallidos de logueo

**Armando la clase AdminDB**

.. code-block:: c

	#ifndef ADMINDB_H
	#define ADMINDB_H

	#include <QObject>
	#include <QSqlDatabase>

	class AdminDB : public QObject
	{
	    Q_OBJECT
	public:
	    explicit AdminDB(QObject *parent = 0);
	    ~AdminDB();

	    bool conectar(QString archivoSqlite);
	    QSqlDatabase getDB();
	    bool isConnected();
	    void mostrarTabla(QString tabla);

	private:
	    QSqlDatabase db;
	};

	#endif // ADMINDB_H

.. code-block:: c

	#include "admindb.h"
	#include <QDebug>
	#include <QSqlQuery>
	#include <QSqlRecord>

	AdminDB::AdminDB(QObject *parent) : QObject(parent)  {
	    qDebug() << "Drivers disponibles:" << QSqlDatabase::drivers();

	    db = QSqlDatabase::addDatabase("QSQLITE");
	}

	AdminDB::~AdminDB()  {
	    if (db.isOpen())
	        db.close();
	}

	bool AdminDB::conectar(QString archivoSqlite)  {
	    db.setDatabaseName(archivoSqlite);

	    return db.open();
	}

	QSqlDatabase AdminDB::getDB()  {
	    return db;
	}

	bool AdminDB::isConnected()  {
	    return db.isOpen();
	}

	void AdminDB::mostrarTabla(QString tabla)  {
	    if (this->isConnected())  {
	        QSqlQuery query = db.exec("SELECT * FROM " + tabla);

	        if (query.size() == 0 || query.size() == -1)
	            qDebug() << "La consulta no trajo registros";

	        while(query.next())  {
	            QSqlRecord registro = query.record();  // Devuelve un objeto que maneja un registro (linea, row)
	            int campos = registro.count();  // Devuleve la cantidad de campos de este registro

	            QString informacion;  // En este QString se va armando la cadena para mostrar cada registro
	            for (int i=0 ; i<campos ; i++)  {
	                informacion += registro.fieldName(i) + ":";  // Devuelve el nombre del campo
	                informacion += registro.value(i).toString() + " - ";
	            }
	            qDebug() << informacion;
	        }
	    }
	    else
	        qDebug() << "No se encuentra conectado a la base";
	}



		
Ejercitación para primer parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Ejercicio 11** 

- Definir la siguiente jerarquía de clases:
 
.. figure:: images/clase10/clases.png 

- Se pedirá definición de atributos y métodos (en papel y sin utilizar material de consulta)
- Instanciar objetos de estas clases.
- Prestar atención sobre los punteros a objetos, ámbitos, parámetros en funciones, modificadores de acceso, ...

**Ejercicio:** Aritmética de punteros - Escribir la salida por consola

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication app(argc, argv);

	    int a = 10, b = 10, c = 10, d = 10, e = 10;
	    int m[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	    int *p = &m[2], *q = &m[4];

	    qDebug() << a + m[d/c] + b-- / *q + 10 + e--;
	    p = m;
	    qDebug() << e + *p + m[4]++;

	    return 0;
	}
	

**Ejercicio 12**

- Comenzar un proyecto vacío con QtCreator y diseñar el siguiente login de usuarios:
 
.. figure:: images/clase10/login.png  

- Este login tendrá las siguientes características:
	- Cuidar muy bien el layout. Notar la ubicación del botón con respecto a los campos.
	- Definido en la clase Login en los archivos login.h y login.cpp.
	- La ventana tendrá un tamaño de 250x120 píxeles y llevará por título "Login".
	- El único usuario válido es (DNI del alumno):(últimos 4 números del DNI)
	- Ocultar con asteriscos la clave.
	- Si el usuario y clave no es válido, sólo el campo de la clave se deberá limpiar.
	- Al fallar la clave 3 veces, la aplicación se cierra. 

- Si el usuario es válido, entonces se ocultará el login y se visualizará un nuevo QWidget como el que sigue:

.. figure:: images/clase10/ventana.png  
 
- Este widget tendrá las siguientes características:
 	- Definido en la clase Ventana en los archivos ventana.h y ventana.cpp.
	- Con QNetworkAccessManager descargar una imagen cualquiera de 100x100 píxeles.
	- Esta imagen se mostrará en el QWidget centrada (como muestra el ejemplo).
	- Dibujar además un cuadrado que envuelva la imagen (como muestra el ejemplo).
	- La ventana puede tener cualquier tamaño y llevará por título "Ventana".





Clase QTimer
^^^^^^^^^^^^

- Permite programar tareas de una sola ejecución o tareas repetitivas. 
- Conectamos la señal ``timeout()`` con algún slot.
- Con ``start()`` comenzamos y la señal ``timeout()`` se emitirá al terminar.

**Ejemplo (repetitivo):** Temporizador que cada 1000 mseg llamará a ``slot_update()``

.. code-block:: c

	QTimer *timer = new QTimer(this);
	connect(timer, SIGNAL(timeout()), this, SLOT(slot_update()));
	timer->start(1000);
 
**Para una sola ejecución**

- Para temporizador de una sola ejecución usar ``setSingleShot(true)``
- El método estático ``QTimer::singleShot()`` nos permite la ejecución.

**Ejemplo:** Luego de 200 mseg se llamará a ``slot_update()``:

.. code-block:: c

	QTimer::singleShot(200, this, SLOT(slot_update()));
	// donde this es el objeto que tiene definido el slot_update().
	

Uso de Qt Designer
..................

- Nuevo proyecto -> Qt GUI Application
- Utilizar el puntero ``ui`` para acceder a los objetos del diseño
- Tener en cuenta que los métodos virtuales de QWidget para eventos se pueden usar:

.. code-block:: c	

	virtual void mousePressEvent(QMouseEvent* event);
	virtual void resizeEvent(QResizeEvent* event);
	virtual void moveEvent(QMoveEvent* event);
	...

**Ejemplo**

.. code-block:: c	
	
	// ventana.h
	#ifndef VENTANA_H
	#define VENTANA_H

	#include <QWidget>

	namespace Ui {
	    class Ventana;
	}

	class Ventana : public QWidget  {
	    Q_OBJECT

	public:
	    explicit Ventana(QWidget *parent = 0);
	    ~Ventana();

	private:
	    Ui::Ventana *ui;
	};

	#endif // VENTANA_H

.. code-block:: c

	// ventana.cpp
	#include "ventana.h"
	#include "ui_ventana.h"

	Ventana::Ventana(QWidget *parent) : QWidget(parent), ui(new Ui::Ventana)  {
	    ui->setupUi(this);
	}

	Ventana::~Ventana()  {
	    delete ui;
	}

**Ejercicio 14**

- Usar QtDesigner
- Definir la clase Ventana que herede de QWidget
- Buscar una imagen de un fútbol con formato PNG (para usar transparencias).
- Ventana tendrá un formulario que pide al usuario:
	- Diámetro del fútbol (píxeles):
	- Velocidad (mseg para ir de lado a lado):
	- QPushButton para actualizar el estado.
- El fútbol irá golpeando de izquierda a derecha en Ventana.



Enumeraciones
^^^^^^^^^^^^^

- Es un tipo especial de variable
- Sus valores son constantes enteras
- Estos valores pueden ser autogenerados (0, 1, 2, 3, ...)

.. code-block:: c	

	enum los_dias { DOM, LUN, MAR, MIE, JUE, VIE, SAB } dia;

	enum los_dias { DOM = 7, LUN = 1, MAR, MIE, JUE = 0, VIE, SAB };

- Las variables de este tipo pueden adoptar sólo valores DOM, LUN, ...
- Es decir, la variable "dia" puede tomar DOM o LUN o MAR ...
- Las enumeraciones declaradas dentro de una clase tiene la visibilidad de la clase

.. code-block:: c	

	class Dia  {
	public:
	    enum los_dias { LUN, MAR, MIE, JUE, VIE };
	    int un_dia;
	};

	int main(int argc, char** argv)  {
	    Dia d1;
	    d1.un_dia = Dia::LUN;
	}

**Ejercicio 15**
 
.. figure:: images/clase15/ejercicio.jpg 




Un par de memes antes del examen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: images/clase10/meme2.jpg

.. figure:: images/clase10/meme4.jpg

