.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 18 - POO 2018
===================
(Fecha: 21 de mayo)


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

**Ejercicio 13**

- Usar QtDesigner
- Definir la clase Ventana que herede de QWidget
- Buscar una imagen de un fútbol con formato PNG (para usar transparencias).
- Ventana tendrá un formulario que pide al usuario:
	- Diámetro del fútbol (píxeles):
	- Velocidad (mseg para ir de lado a lado):
	- QPushButton para actualizar el estado.
- El fútbol irá golpeando de izquierda a derecha en Ventana.


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


**Ejercicio 14**

- Diseñar una aplicación con un login inicial que valide contra la base
- Almacenar sólo el hash en MD5 de las contraseñas
- Si el usuario es válido mostrar cualquier widget ya creado (Maps, Imagen, paint)
- Registrar en la tabla 'logs' los intentos fallidos de logueo




Clase QFileDialog
^^^^^^^^^^^^^^^^^

- Permite abrir un cuadro de diálogo para buscar un archivo en disco

.. code-block:: c	

	QString file = QFileDialog::getOpenFileName(this, "Abrir", "./", "Imagen (*.png *.jpg)");

**Ejercicio 15**

- Elegir un archivo de imagen del disco con ``QFileDialog`` y dibujarlo en un ``QWidget``.
- Agregar un botón "Iniciar rotación" que genere la rotación de la imagen sobre su centro.


**Ejercicio 16** Al ingresar la URL de una imagen deberá mostrarla como en la figura

.. figure:: images/clase10/imagenes.png  
 
- Al hacer clic sobre una de estas imágenes, deberá ocultarse la misma. 
- Cuando se oculta la segunda imagen, cerrar la aplicación.


Creando Instalador
^^^^^^^^^^^^^^^^^^

**Mexican explanation**

|ImageLink|_ 

.. |ImageLink| image:: /images/clase14/mexicano.gif
.. _ImageLink: https://www.youtube.com/watch?v=rr6G7GU52Wc

**Capturas de pantalla de la creación**

.. figure:: images/clase14/CrearInstalador.gif


**Ejercicio 17**

- Diseñar una aplicación que muestre en un ``QWidget`` cualquier imagen de 50x50
- La imagen deberá seguir al puntero del mouse cuando esté presionado un botón.
- Utilizar ``QTimer`` para actualizar la posición de la imagen dando un efecto inercial


Ejecutable del ejercicio de arrastrar y soltar la imagen
........................................................

- `Descargar Instalador de MouseMove (Windows 7 o superior - 64 bits) <https://drive.google.com/file/d/0B3bNJFNPgLHnc3ota21TVVBKb0k/view?usp=sharing>`_

- `Descargar MouseMove (Linux - 64 bits) <https://drive.google.com/file/d/0B3bNJFNPgLHnMGtzWjlQa3RIc1E/view?usp=sharing>`_




Señales propias
^^^^^^^^^^^^^^^

- Si necesitamos enviar una señal se utiliza la palabra reservada ``emit``.

.. code-block:: c	

	int i = 5;
	emit signal_enviarEntero(i);


- La función ``enviarEntero(int a)`` debe estar declarada con el modificador de acceso ``signals``

.. code-block:: c	

	signals:
	    void signal_enviarEntero(int);


- No olvidarse de la macro ``Q_OBJECT`` para permitir a esta clase usar signals y slots.
- Las signals deben ser compatibles en sus parámetros con los slots a los cuales se conecten.
- Solamente se declara esta función (Qt se encarga de definirla).


**Ejercicio 18** 

- Crear un login con un QLabel que funcione como un QPushButton
- Para esto incorporar al QLabel la señal ``void signal_clic()``


**Ejercicio 19** 

- Incorporar a un Login una señal que se emita cada vez que un usuario se valide exitosamente
- Que la señal se llame ``void signal_usuarioLogueado(QString)``
- El QString que envía es el nombre de usuario
