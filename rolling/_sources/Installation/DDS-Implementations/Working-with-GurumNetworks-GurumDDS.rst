.. redirect-from::

    Working-with-GurumNetworks-GurumDDS

GurumNetworks GurumDDS
======================

rmw_gurumdds es una implementación de la interfaz de middleware de ROS que utiliza GurumNetworks GurumDDS.
Más información sobre GurumDDS está disponible en nuestro sitio web: https://gurum.cc/index_eng


Prerequisitos
-------------

La siguiente descripción asume que ha completado el proceso de 'Configuración del entorno'
desde :doc:`Instalación de ROS 2 a través de paquetes Debian <../Ubuntu-Install-Debians>` o
del :doc:`Building ROS 2 on Ubuntu Linux <../Alternatives/Ubuntu-Development-Setup>`.

Prerequisites
-------------

The following description assumes that you have completed the 'Environment setup' process
from the :doc:`Installing ROS 2 via Debian Packages <../Ubuntu-Install-Debians>` or
from the :doc:`Compilando ROS 2 en Ubuntu Linux <../Alternatives/Ubuntu-Development-Setup>`.

rmw_gurumdds requiere la versión de GurumDDS-2.8.x.
Los paquetes Debian de GurumDDS se proporcionan en los repositorios apt de ROS 2 en Linux.
El instalador binario de Windows de GurumDDS estára compatible pronto.

GurumDDS requiere una licencia. Consulte la página siguiente: https://gurum.cc/free_trial_eng.html

Después de solicitar la licencia de prueba, por favor descargala desde la página web de licencia.
Después de obtener una licencia, muévela a la siguiente ubicación.

=============  ================
 DDS Version   License Location
=============  ================
<= 2.7.2860    /etc/flame
>= 2.7.2861    /etc/gurumnet
2.8.x          /etc/gurumnet
=============  ================


Instalar paquetes
-----------------

La forma más fácil es instalar desde el repositorio apt de ROS 2.
Cuando se instala ros-{DISTRO}-rmw-gurumdds-cpp, también se instala gutumdds-2.8 .

.. code-block:: bash

   sudo apt install ros-{DISTRO}-rmw-gurumdds-cpp

Compilar a partir del código fuente
-----------------------------------

Compilar a partir del código fuente también es otra forma de instalar.

Primero, clona rmw_gurumdds en el directorio src del espacio de trabajo de ROS 2.

.. code-block:: bash

   cd ros2_ws/src
   git clone https://github.com/ros2/rmw_gurumdds -b {DISTRO} ros2/rmw_gurumdds

Luego, instale los paquetes necesarios para GurumDDS.

.. code-block:: bash

   cd ..
   rosdep install --from src -i --rosdistro {DISTRO}

Finalmente, ejecuta colcon build.

.. code-block:: bash

   colcon build --symlink-install

Cambiar a rmw_gurumdds
----------------------

Cambie de otro rmw a rmw_gurumdds especificando la variable de entorno.

.. code-block:: bash

   export RMW_IMPLEMENTATION=rmw_gurumdds_cpp

Consulta también: :doc:`Trabajar con varias implementaciones de RMW <../../How-To-Guides/Working-with-multiple-RMW-implementations>`

Ejecuta el talker y el listener
-------------------------------

Ahora ejecuta ``talker`` y ``listener`` para probar GurumDDS.

.. code-block:: bash

   ros2 run demo_nodes_cpp talker

.. code-block:: bash

   ros2 run demo_nodes_cpp listener
