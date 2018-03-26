.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 05 - POO 2018
===================
(Fecha: 26 de marzo)

:Tarea para Clase 8:
	Entregar los ejercicios que están enumerados incluyendo los de la clase 6


Clases
======

.. code-block:: c

	class ClaseEjemplo  {
	    // Lista de miembros (generalmente funciones y datos)
	    // Los datos no pueden ser inicializados (es una declaración)
	    // Si las funciones se definen fuera, se usa el operador :: 
	    // :: es el operador de acceso a ámbito
	};

**Ejemplo:**

.. code-block:: c

	#include <iostream>
	using namespace std;

	class Punto  {
	private:
	    // Datos miembro de la clase "Punto"
	    int a, b;
		
	public:
	    // Funciones miembro de la clase "Punto"
	    void getDatos(int &a2, int &b2);
	    void setDatos(int a2, int b2)  {
	        a = a2;
	        b = b2;
	    }
	};

	void Punto::getDatos(int &a2, int &b2)  {
	    a2 = a;
	    b2 = b;
	}

	int main()  {
	    Punto punto1;
		int x, y;  // Variables donde se copiarán los valores de punto1

	    punto1.setDatos(12, 32);
	    punto1.getDatos(x, y);

	    cout << "(" << x << “, ” << y << “)” << endl;
	}
	
	// La función "setDatos()" se definió en el interior de la clase (lo haremos sólo cuando
	// la definición sea muy simple, ya que dificulta la lectura y comprensión del programa). 

**Constructor**

.. code-block:: c

	class Punto  {
	public:
	    Punto(int a2, int b2);

	    void getDatos(int &a2, int &b2);
	    void setDatos(int a2, int b2);
		
	private:
	    // Datos miembro de la clase "Punto"
	    int a, b;
	};

	Punto::Punto(int a2, int b2)  {
	    a = a2;
	    b = b2;
	}

	void Punto::getDatos(int &a2, int &b2)  {
	    a2 = a;
	    b2 = b;
	}

	void Punto::setDatos(int a2, int b2)  {
	    a = a2;
	    b = b2;
	}

**Cuestiones sobre declaraciones**

.. code-block:: c

	Punto punto1;  // Llama al constructor sin parámetros. En esta última versión 
	               // de Punto, esto no serviría, ya que no hay constructor sin parámetros. 
				   // Si no se especifica un constructor, el compilador crea uno (igual que 
				   // en Java). Por lo tanto, esta declaración sirve para una clase Punto 
				   // donde el programador no escriba constructor.

	Punto punto1();  // Se entiende como el prototipo de una función sin parámetros que 
	                 // devuelve un objeto Punto. Es decir, no sirve para instanciar un 
					 // objeto con el contructor sin parámetros de Punto.

	Punto punto1(12,43);  // Válido
	Punto punto2(45,34);  // Válido


**Inicialización de objetos**

.. code-block:: c

	Punto(int a2, int b2)  {
	    a = a2;
	    b = b2;
	}

	// O también se permite:

	Punto::Punto(int a2, int b2) : a(a2), b(b2)  {  }

	Punto::Punto() : a(0), b(0)  {  }

**El puntero this**

.. code-block:: c

	#include <iostream>
	using namespace std;

	class Punto  {
	public:
	    // Constructor
	    Punto(int a2, int b2)  {  }
	
	    // Funciones miembro de la clase "Punto"
	    void getDatos(int &a2, int &b2)  {  }
	    void setDatos(int a2, int b2);
	
	private:
	    // Datos miembro de la clase "Punto"
	    int a, b;
	};

	void Punto::setDatos(int a2, int b2) {
	    a = a2;
	    b = b2;
	}

	// O lo podemos hacer con this:

	void Punto::setDatos(int a2, int b2) {
	    this->a = a2;
	    this->b = b2;
	}


**Constructores con argumentos por defecto**

.. code-block:: c

	class ClaseA  {
	public:
	    ClaseA(int a = 10, int b = 20) : a(a), b(b)  {  }
	
	    void verDatos(int &a, int &b)  {
	        a = this->a;
	        b = this->b;
	    }

	private:
	    int a, b;
	};

	int main(int argc, char** argv)  {
	    ClaseA* objA = new ClaseA;

	    int a, b;
	    objA->verDatos(a, b);
	
	    std::cout << "a = " << a << " b = " << b << std::endl;

	    return 0;
	}

	// Probar con:	
	
	ClaseA(int c, int a = 10, int b = 20) : a(a), b(b), c(0)  {  }

	ClaseA(int a = 10, int b = 20, int c) : a(a), b(b), c(0)  {  }

**Destructor**

.. code-block:: c

	ClaseA::~ClaseA()  {
	    a = 0;
	    b = 0;
	}
	

.. ..
 
 <!---  
 **Función con número indefinido de parámetros** 

 (para ocultar requiere una primer linea con .. ..    Los que queremos ocultar debe tener el menos un espacio)

 - Requiere:

 .. code-block:: c

 	#include <cstdarg>

 - Imprime los enteros que se pasen como parámetro
 - Se puede comprender la sintaxis de:

 .. code-block:: c

 	int printf(const char* format, ...)

 .. code-block:: c

 	void imprimirParametros(int cantidad, ...)  {

 	    // En cstdarg se define un tipo va_list y define tres macros (va_start, va_arg y va_end)
 	    // para moverse por la lista de argumentos cuyo numero y tipo no son conocidos.
 
 	    // Aqui se declara la lista de parametros
 	    va_list argumentos; 
 				
 	    // La macro va_start inicializa 'argumentos' para ser usado por va_arg y va_end.
 	    // 'cantidad' es el nombre del ultimo parametro antes de la lista de argumentos.
 	    va_start(argumentos, cantidad); 
 
 	    for (int i=0 ; i<cantidad ; i++)  {
 
 		    // La macro va_arg contiene el tipo y el valor del proximo argumento. 
 			// Cada llamada a va_arg devuelve el resto de los argumentos.
 
 	        int valor = va_arg( argumentos, int );  // Devuelve en formato de int
 
 	        cout << valor << endl;
 	    }
 
 	    // A cada invocacion de va_start le corresponde una invocacion de va_end
 	    // en la misma funcion. 	   
 	    va_end(argumentos);  // Para limpiar la pila de parametros
 	}
 	
 **Ejercicio:** 
 
 - Definir una función (que se llame mi_printf) que realice el mismo trabajo que la famosa printf. 
 - Investigar qué tipos de datos se pueden utilizar en va_arg
 
 
 **Se puede pasar cualquier tipo siempre que sea con punteros:**
  
 .. code-block:: c
  
 	#include <QApplication>
 	#include <QString>
 	#include <QDebug>
 	#include <cstdarg>
 
 	void imprimirParametros(int cantidad, ...)  {
 	    va_list argumentos; // esta linea declara la lista de parametros
 	    va_start(argumentos, cantidad);
 
 	    for (int i=0 ; i<cantidad ; i++)  {
 	        QString *str = va_arg( argumentos, QString* );
 	        qDebug() << *str;
 	    } 
 
 	    va_end(argumentos);  // Para limpiar la pila de parametros
 	}
 
 	int main(int argc, char** argv)  {
 	    QApplication app(argc, argv);
 
 	    imprimirParametros(3, new QString("uno"), new QString("dos"), new QString("tres"),
 	                       new QString("cuatro"), new QString("cinco"));
 
 	    return 0;
 	}
 --->
 
 


Primer aplicación en Qt con interfaz gráfica
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Qt(Quasar Toolkit) 
	- Biblioteca para desarrollo de software de Quasar Technologies
	- Se llamó también Trolltech
	- Biblioteca multiplataforma
	- En el 2008 lo compró Nokia
	- Aplicaciones escritas con C++ (Qt)
		- KDE
		- VLC Media Player
		- Skype
		- VirtualBox
		- Google Earth 
		- Spotify para Linux
	- En 2012 Digia compra Qt y comercializa las licencias 
	- Digia desarrolló herramientas para usar Qt en iOS, Android y Blackberry.
		
.. code-block:: c

	#include <QApplication>	
	// - Administra los controles de la interfaz
	// - Procesa los eventos
	// - Existe una única instancia
	// - Analiza los argumentos de la línea de comandos

	int main(int argc, char** argv)  {	
	    // app es la instancia y se le pasa los parámetros de la línea
	    // de comandos para que los procese.
	    QApplication app(argc, argv); 

	    QLabel hola("<H1 aling=right> Hola </H1>");
	    hola.resize(200, 100);
	    hola.setVisible(true);

	    app.exec();  // Se le pasa el control a Qt
	    return 0;
	}

Signals y slots
^^^^^^^^^^^^^^^

- signal y slot son funciones.
- Las signals de una clase se comunican con los slots de otra.
- Se deben conectar con la función connect de QObject.
- Un evento puede generar una signal.
- Los slots reciben estas signals.
- SIGNAL() y SLOT() son macros (convierten a cadena).
- emisor y receptor son punteros a QObject:

.. code-block:: c

	QObject::connect(emisor, SIGNAL(signal), receptor, SLOT(slot));
	
- Se puede remover la conexión:

.. code-block:: c

	QObject::disconnect(emisor, SIGNAL(signal), receptor, SLOT(slot));

**Ejemplo:** QPushButton para cerrar la aplicación.

.. code-block:: c

	#include <QApplication>
	#include <QPushButton>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);
	    QPushButton* boton = new QPushButton("Salir");

	    QObject::connect(boton, SIGNAL(clicked()), &a, SLOT(quit()));
	    boton->setVisible(true);
		
	    return a.exec();
	}

	


**Array como parámetro en funciones**

.. code-block:: c

	#include <iostream>
	using namespace std;

	void funcion( int miArray[] );
	// Le estamos pasando un puntero al primer elemento del array.

	int main()  {
	    int miA[ 5 ] = { 0, 1, 2, 3, 4 };

	    funcion( miA );

	    cout << miA[ 0 ] << miA[ 1 ] << miA[ 2 ] << miA[ 3 ] << miA[ 4 ];
	}

	void funcion( int miArray[] )  {
	    miArray[ 0 ] = 5;  // Las modificaciones quedarán.

	    miArray[ 3 ] = 5; 
	} 

	
**Ejemplo:** Control de volumen

.. code-block:: c

	#include <QApplication>
	#include <QWidget>
	#include <QHBoxLayout>
	#include <QSlider>
	#include <QSpinBox>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    QWidget* ventana = new QWidget;  // Es la ventana padre (principal)
	    ventana->setWindowTitle("Volumen"); 
	    ventana->resize(300, 50);

	    QSpinBox* spinBox = new QSpinBox;
	    QSlider* slider = new QSlider(Qt::Horizontal);
	    spinBox->setRange(0, 100);
	    slider->setRange(0, 100);

	    QObject::connect(spinBox, SIGNAL(valueChanged(int)), slider, SLOT(setValue(int)));
	    QObject::connect(slider, SIGNAL(valueChanged(int)),  spinBox, SLOT(setValue(int)));

	    spinBox->setValue(15);

	    QHBoxLayout* layout = new QHBoxLayout;
	    layout->addWidget(spinBox);
	    layout->addWidget(slider);
	    ventana->setLayout(layout);
	    ventana->setVisible(true);	

	    return a.exec();
	}

**Ejercicio 3**

- Cuando el valor del QSlider se modifique, colocar como título de la ventana el mismo valor (de 0 a 100). 
	
QLineEdit
^^^^^^^^^

.. code-block:: c

	QLineEdit* le = new QLineEdit;
	le->setEchoMode(QLineEdit::Password);
	le->setEnabled(false);

	// QLineEdit::Normal  // Se visualizan al escribir
	// QLineEdit::NoEcho  // No se visualiza nada
	// QLineEdit::Password  // Se escribe como asteriscos
	// QLineEdit::PasswordEchoOnEdit  // Se escribe normal y al dejar de editar	se convierten en asteriscos

**Señales**

.. code-block:: c

	// void returnPressed()  // Detecta cuando el usuario presiona Enter.

	// void editingFinished()  // Cuando pierde foco.

	// void textChanged(const QString &text)  // Texto modificado por código o por usuario desde la interfaz.

	// void textEdited(const QString &text)  // Sólo por el usuario.

QGridLayout
^^^^^^^^^^^

- Ubica los widgets en una grilla
- Con setColumnMinimumWidth() podemos setear el ancho mínimo de columna
- Separación entre widget con setVerticalSpacing(int)
- void addWidget(QWidget* widget, int fila, int columna, int spanFila, int spanCol)

Macro Q_OBJECT
^^^^^^^^^^^^^^

- Convierte a una clase cualquiera en una clase Qt.
- Una clase Qt permitirá trabajar con signals y slots.
- Con la macro Q_OBJECT en la declaración de la clase la convertimos.

**Ejercicio 4**

- Construir un login.
- Usar asteriscos para la clave.
- Detectar enter para simular la pulsación del botón.
- Si la clave ingresada es admin:admin, la aplicación se cerrará.
- Si se ingresa otra clave se borrará el texto de los QLineEdit.

- Tener en cuenta que este ejercicio requiere conocer cómo se define un slot propio.

QGridLayout
^^^^^^^^^^^

- Ubica los widgets en una grilla
- Con setColumnMinimumWidth() podemos setear el ancho mínimo de columna
- Separación entre widget con setVerticalSpacing(int)
- void addWidget(QWidget* widget, int fila, int columna, int spanFila, int spanCol)





