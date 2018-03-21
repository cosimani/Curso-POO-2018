.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 04 - POO 2018
===================
(Fecha: 20 de marzo)

:Tarea para Clase 5:
	Ver `Tutorial Qt qDebug <https://www.youtube.com/watch?v=z4cespk-EMk>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt QString <https://www.youtube.com/watch?v=gAfMOPKsgYk>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt QObject <https://www.youtube.com/watch?v=cDE9hg_Ajwc>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_



En busca de proyectos
=====================

Detección de necesidades
^^^^^^^^^^^^^^^^^^^^^^^^

- Buscar en el hogar
- En la Universidad
- Camino a casa
- Salir de compras
- Un familiar en problemas

Hagamos esto
^^^^^^^^^^^^

- Reunirse en grupo y detecten necesidades
- No proponer ninguna solución aún
- Si inevitablemente piensan en alguna solución, no se limiten a que sea una solución tecnológica
- Enumerar las cinco mejores necesidades, o las cinco mejores para ser solucionadas


Cadena de caracteres
^^^^^^^^^^^^^^^^^^^^

- Al estilo C	

.. code-block:: c

	#include <string.h>

	char cadena1[ 30 ], cadena2[ 30 ];
	strcpy( cadena1, "Hola" );
	cin >> cadena2;
	
- Con C++ usamos   

.. code-block:: c

	#include<string>

	Asignación			s1 = s2		s1 = "Hola"
	Concatenación		s1 = s2 + s3	
	Comparación			if ( s1 == s2 )
	Subcadenas			s1.substr( 3, 5 )
	Longitud			s1.length()	s2.size()  // Son lo mismo
	Acceso a char		s1[ 2 ]			s2.at( 2 )  // Lanza out_of_range
	Limpiar				s1.clear()
	Busca cadena		s1.find( "cadena" );    s1.find( s2 );
	Puntero a char		const char *c = s1.c_str()


