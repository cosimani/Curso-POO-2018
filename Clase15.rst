.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 15 - POO 2018
===================
(Fecha: 8 de mayo)


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

