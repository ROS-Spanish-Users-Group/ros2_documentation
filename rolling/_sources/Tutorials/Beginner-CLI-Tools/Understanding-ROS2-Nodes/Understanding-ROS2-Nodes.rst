.. redirect-from::

    Tutorials/Understanding-ROS2-Nodes

.. _ROS2Nodes:

Comprender los Nodos
====================

**Objetivo:** Aprender sobre la función de los nodos en ROS 2 y las herramientas para interactuar con ellos.

**Nivel del Tutorial:** Principiante

**Tiempo:** 10 minutos

.. contents:: Contenido
   :depth: 2
   :local:

Historial
---------

1 El grafo ROS 2
^^^^^^^^^^^^^^^^^

En los próximos tutoriales, aprenderás sobre una serie de conceptos básicos de ROS 2 que componen lo que se conoce como el 'grafo de ROS (2)'.

<<<<<<< HEAD
El grafo ROS es una red de elementos de ROS 2, que procesan datos al mismo tiempo.
Abarca todos los ejecutables y las conexiones entre ellos, como si tuviera que mapearlos y visualizarlos.
=======
The ROS graph is a network of ROS 2 elements processing data together at the same time.
It encompasses all executables and the connections between them if you were to map them all out and visualize them.
>>>>>>> og

2 Nodos in ROS 2
^^^^^^^^^^^^^^^^

<<<<<<< HEAD
Cada nodo en ROS debe ser responsable de un solo propósito, en forma modular (por ejemplo, un nodo para controlar los motores de las ruedas, un nodo para controlar un telémetro láser, etc.).
Cada nodo puede enviar y recibir datos a otros nodos a través de topics, servicios, acciones o parámetros.
=======
Each node in ROS should be responsible for a single, modular purpose, e.g. controlling the wheel motors or publishing the sensor data from a laser range-finder.
Each node can send and receive data from other nodes via topics, services, actions, or parameters.
>>>>>>> og

.. image:: images/Nodes-TopicandService.gif

Un sistema robótico completo se compone de muchos nodos que trabajan en conjunto.
En ROS 2, un único ejecutable (programa C++, programa Python, etc.) puede contener uno o más nodos.

Requisitos previos
------------------

El :doc:`tutorial previo<../Introducing-Turtlesim/Introducing-Turtlesim>` mustra como instalar el paquete ``turtlesim`` utilizado a continuación.

<<<<<<< HEAD
Como siempre, no olvides hacer un source ROS 2 en :doc:`cada terminal nueva<../Configuring-ROS2-Environment>`.
=======
As always, don't forget to source ROS 2 in :doc:`every new terminal you open <../Configuring-ROS2-Environment>`.
>>>>>>> og

Tareas
------

1 ros2 run
^^^^^^^^^^

El comando ``ros2 run`` lanza un ejecutable desde un paquete.

.. code-block:: console

    ros2 run <package_name> <executable_name>

Para ejecutar Turtlesim, abre una nueva terminal e introduce el siguiente comando:

.. code-block:: console

    ros2 run turtlesim turtlesim_node

Se abrirá la ventana de turtlesim, como se vió en el :doc:`tutorial previo<../Introducing-Turtlesim/Introducing-Turtlesim>`.

Aquí, el nombre del paquete es ``turtlesim`` y el nombre del ejecutable es ``turtlesim_node``.

<<<<<<< HEAD
Sin embargo, todavía no sabemos el nombre del nodo.
Puedes encontrar nombres de nodos usando el comando ``ros2 node list``.
=======
We still don't know the node name, however.
You can find node names by using ``ros2 node list``
>>>>>>> og

2 ros2 node list
^^^^^^^^^^^^^^^^

``ros2 node list`` te mostrará los nombres de todos los nodos que están actualmente en ejecución.
Esto es especialmente útil cuando desea interactuar con un nodo o cuando tiene un sistema que ejecuta muchos nodos y necesitas realizar un seguimiento de ellos.

Abre una nueva terminal mientras turtlesim aún se está ejecutando en la otra, e ingrese el siguiente comando:

.. code-block:: console

    ros2 node list

El terminal devolverá el nombre del nodo:

.. code-block:: console

  /turtlesim

Abre otra terminal nueva e inicia el nodo teleop con el comando:

.. code-block:: console

    ros2 run turtlesim turtle_teleop_key

<<<<<<< HEAD
Aquí estamos buscando de nuevo en el paquete turtlesim, esta vez el ejecutable llamado ``turtle_teleop_key``.
=======
Here, we are referring to the ``turtlesim`` package again, but this time we target the executable named ``turtle_teleop_key``.
>>>>>>> og

Regresa a la terminal donde se ejecutó ``ros2 node list`` y vuelve a ejecutarlo.
Ahora verás los nombres de dos nodos activos:

.. code-block:: console

  /turtlesim
  /teleop_turtle

2.1 Reasignación
~~~~~~~~~~~~~~~~

<<<<<<< HEAD
La reasignación te permite cambiar propiedades predeterminadas de los nodos, como su nombre, nombre del topic, nombres de servicios, etc., a valores personalizados.
En el último tutorial, utilizaste la reasignación en ``turtle_teleop_key`` para cambiar la tortuga que se controla.

Ahora, vamos a reasignar el nombre de nuestro nodo ``/turtlesim``.
En una nueva terminal, ejecuta el siguiente comando:
=======
`Remapping <https://design.ros2.org/articles/ros_command_line_arguments.html#name-remapping-rules>`__ allows you to reassign default node properties, like node name, topic names, service names, etc., to custom values.
In the last tutorial, you used remapping on ``turtle_teleop_key`` to change the cmd_vel topic and target **turtle2**.

Now, let's reassign the name of our ``/turtlesim`` node.
In a new terminal, run the following command:
>>>>>>> og

.. code-block:: console

  ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle

<<<<<<< HEAD
Ya que estás llamando a ``ros2 run`` para que se ejecute en turtlesim nuevamente, se abrirá otra ventana de turtlesim.
Sin embargo, ahora, si regresas a la terminal donde ejecutó la lista de nodos ros2 y lo vuelves a ejecutar, verás tres nombres de nodos:
=======
Since you're calling ``ros2 run`` on turtlesim again, another turtlesim window will open.
However, now if you return to the terminal where you ran ``ros2 node list``, and run it again, you will see three node names:
>>>>>>> og

.. code-block:: console

    /my_turtle
    /turtlesim
    /teleop_turtle

3 ros2 node info
^^^^^^^^^^^^^^^^

Ahora que conoces los nombres de tus nodos, puedes acceder a más información sobre ellos con:

.. code-block:: console

    ros2 node info <node_name>

Para obtener información del nodo ``my_turtle``, ejecuta el siguiente comando:

.. code-block:: console

    ros2 node info /my_turtle

<<<<<<< HEAD
``ros2 node info`` devuelve una lista de suscriptores, publicadores, servicios y acciones (las conexiones del grafo de ROS) que interactúan con ese nodo.
La salida debería verse así:
=======
``ros2 node info`` returns a list of subscribers, publishers, services, and actions. i.e. the ROS graph connections that interact with that node.
The output should look like this:
>>>>>>> og

.. code-block:: console

  /my_turtle
    Subscribers:
      /parameter_events: rcl_interfaces/msg/ParameterEvent
      /turtle1/cmd_vel: geometry_msgs/msg/Twist
    Publishers:
      /parameter_events: rcl_interfaces/msg/ParameterEvent
      /rosout: rcl_interfaces/msg/Log
      /turtle1/color_sensor: turtlesim/msg/Color
      /turtle1/pose: turtlesim/msg/Pose
    Service Servers:
      /clear: std_srvs/srv/Empty
      /kill: turtlesim/srv/Kill
      /my_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters
      /my_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
      /my_turtle/get_parameters: rcl_interfaces/srv/GetParameters
      /my_turtle/list_parameters: rcl_interfaces/srv/ListParameters
      /my_turtle/set_parameters: rcl_interfaces/srv/SetParameters
      /my_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
      /reset: std_srvs/srv/Empty
      /spawn: turtlesim/srv/Spawn
      /turtle1/set_pen: turtlesim/srv/SetPen
      /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
      /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    Service Clients:

    Action Servers:
      /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
    Action Clients:

Ahora intenta ejecutar el mismo comando en el nodo ``/teleop_turtle`` y vea cómo las conexiones difieren de ``my_turtle``.

Aprenderás más sobre los conceptos de conexión de gráficos de ROS, incluidos los tipos de mensajes, en los próximos tutoriales.

Resumen
-------

Un nodo es un elemento fundamental de ROS 2, que es modular y tiene un único propósito en un sistema de robótica.

<<<<<<< HEAD
En este tutorial, utilizaste nodos creados a partir del paquete ``turtlesim`` ejecutando ``turtlesim_node`` y ``turtle_teleop_key``.

Aprendiste a usar el comando ``ros2 node list`` para descubrir nombres de nodos activos y el comando ``ros2 node info`` para obtener información de un nodo en particular.
Estas herramientas son vitales para comprender el flujo de datos en un sistema robótico complejo del mundo real.
=======
In this tutorial, you utilized nodes created in the ``turtlesim`` package by running the executables ``turtlesim_node`` and ``turtle_teleop_key``.

You learned how to use ``ros2 node list`` to discover active node names and ``ros2 node info`` to introspect a single node.
These tools are vital to understanding the flow of data in a complex, real-world robot system.
>>>>>>> og

Pasos siguientes
----------------

Ahora que comprendes los nodos en ROS 2, puedes continuar con el tutorial :doc:`de topics <../Understanding-ROS2-Topics/Understanding-ROS2-Topics>`.
Los topics son uno de los tipos de comunicación que conecta los nodos.

Contenido Relacionado
---------------------

La página :doc:`../../../Concepts` agrega más detalles al concepto de nodos.
