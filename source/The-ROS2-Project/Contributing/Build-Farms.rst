.. redirect-from::

  Contributing/Build-Farms

.. _BuildFarms:

===============
ROS Granjas de Commpilación
===============

.. contents:: Table of Contents
   :depth: 1
   :local:

Las granjas de commpilación ROS son una infraestructura importante para soportar el ecosistema ROS. Son proporcionadas y mantenidas por `Open Robotics`_.
Estas proporcionan: la compilación de paquetes fuente y binarios, integración continua, pruebas y análisis para paquetes ROS 1 y ROS 2.
Hay dos instancias alojadas para paquetes de código abierto:

#. https://build.ros.org/ para paquetes ROS 1
#. https://build.ros2.org/ para paquetes ROS 2

Si vas a utilizar alguna de las infraestructuras provistas, considera inscribirte en el `build farm discussion forum <http://discourse.ros.org/c/buildfarm>`__ para recibir notificaciones, por ejemplo, sobre cualquier cambio próximo.


Trabajos e implementación
-------------------

Las granjas de compilación de ROS realizan varios trabajos diferentes.
Para cada tipo de trabajo encontrarás una descripción detallada de lo que hacen y cómo funcionan:

* `release jobs`_ generar paquetes binarios, por ejemplo, paquetes debian
* `devel jobs`_ compilar y probar paquetes ROS dentro de un solo repositorio en base a sondeos
* `pull_request jobs`_ compilar y probar paquetes ROS dentro de un único repositorio activado por webhooks
* `CI jobs`_ compilar y probar paquetes ROS en repositorios con la opción de usar artifacts de otros trabajos CI para acelerar la compilación
* `doc jobs`_ generar la documentación API de paquetes y extraer información de los manifiestos
* `miscellaneous jobs`_ realizar tareas de mantenimiento y generar datos informativos para visualizar el estado de la granja de compilación y los artifacts generados

Creación e Implementación
.......................

Los trabajos anteriores se crean y se implementan cuando los paquetes son bloomed_, es decir, se liberan para ROS 1 o ROS 2.
Una vez los paquetes superan la instancia de bloomed_ y son incorporados en alguna de las distribuciones ROS (via un pull request a rosdistro_), se generarán los trabajos correspondientes.
Los nombres de los trabajos codifican su tipo y finalidad de las siguientes formas: [1]_

* release jobs:

   * ``{distro}src_{platf}__{package}__{platform}__source`` compila paquetes fuente de lanzamientos
   * ``{distro}bin_{platf}__{package}__{platform}__binary`` compila paquetes binarios de lanzamientos

   Por ejemplo, el trabajo de empaquetado binario de rclcpp en ROS 2 Humble (que se ejecuta en Ubuntu Jammy amd64) se llama ``Hbin_uJ64__rclcpp__ubuntu_focal_amd64__binary``.

* devel jobs:

   * ``{distro}dev__{package}__{platform}`` realiza una compilación de CI para la rama de lanzamiento

* pull_request jobs

   * ``{distro}pr__{package}__{platform}`` realizar una compilación de CI para un pull request. Por ejemplo, el trabajo de pull request para rclcpp en ROS 2 Humble (que se ejecuta en Ubuntu Jammy amd64) se llama ``Hpr__rclcpp__ubuntu_jammy_amd64``.


Ejecución
.........

La ejecución de los trabajos depende del tipo de trabajo:

* `devel jobs`_ se activará cada vez que se realice un commit en el sondeo de la rama respectiva en función de una frecuencia configurada.
* `pull_request jobs`_ será activado por webhooks de la pull request respectiva del repositorio [2]_ ascendente
* `release jobs`_ se activará cada vez que se publique una nueva versión del paquete, es decir, cada vez que se aceptó un nuevo pull_request en rosdistro_ para este paquete. Los trabajos de compilado se desencadenan por un cambio de versión en el archivo de distribución de rosdistro y los trabajos binarios se desencadenan por su equivalente en el código fuente.


Preguntas frecuentes (FAQ) y resolución de problemas
---------------------------------------------------

#. **Recibo correos electrónicos de Jenkins por trabajos fallidos en una granja de compilación. ¿Qué debo hacer?**

   Ve al trabajo que planteó el problema. Encontrarás el enlace en la parte superior del correo electrónico de Jenkins.
   Una vez que hayas seguido el enlace al trabajo de compilación, haz clic en *Console Output* (Salida de la consola) a la izquierda, luego clic en
   *Full Log* (Registro completo). Esto te dará la salida completa de la consola de la compilación que falla. Trata de encontrar el
   el error más importante, pues los otros suelen ser de seguimiento.

   En la parte inferior del correo electrónico podría leer ``'apt-src build [...]' failed. This is usually because of
   an error building the package.`` Esto generalmente sugiere que faltan dependencias, consulte 2.

#. **Parece que me falta una dependencia, ¿cómo puedo saber cuál?**

   Básicamente tienes dos opciones, a. es más fácil; pero puede tomar varias iteraciones, b. es más elaborado y le brinda una visión completa, así como la depuración local.

   a) Inspecciona el trabajo de lanzamiento que generó el problema (ver 1.) y localiza el problema de dependencia de cmake.
    Para hacerlo, deberás ir a la sección cmake, por ejemplo, navega a la sección *build binarydeb* a través del menú de la izquierda en el caso de un trabajo de compilación de ubuntu/debian. El *Error de CMake* generalmente indicará una dependencia requerida por la configuración de cmake; pero que falta en el `manifiesto del paquete`_. Una vez que hayas solucionado la dependencia en el manifiesto, realiza una nueva versión de tu paquete y espera los comentarios de las granjas de compilación o...
   b) Para obtener una visión completa y una depuración local más rápida, puedes `ejecutar los trabajos de lanzamiento localmente`_.
       Esto te permite iterar el manifiesto localmente hasta que se solucionen todas las dependencias.

#. **¿Por qué fallan los trabajos de lanzamiento cuando los trabajos de desarrollo / mis github actions / mis compilaciones locales tienen éxito?**

   Hay varias razones potenciales para esto. En primer lugar, compila los trabajos de publicación con una instalación mínima de ROS para comprobar si todas las dependencias están declaradas correctamente en el `manifiesto del paquete`_. Los trabajos de desarrollo / github actions / compilaciones locales pueden realizarse en un entorno que ya tiene las dependencias instaladas y por lo tanto, no serian evidentes los problemas de dependencia. En segundo lugar, podrías haber complilado diferentes versiones del código fuente. Mientras que los trabajos de desarrollo/ github actions / compilaciones locales generalmente compilan la última versión desde el repositorio *upstream* [2]_, `release jobs`_ compila el código fuente de la última versión, es decir, el código fuente en las respectivas ramas *upstream* del repositorio *release* [3]_.

Otras lecturas
---------------

Los siguientes enlaces proporcionan más detalles e información sobre las granjas de compilación:

* https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/index.rst - Documentación general de la infraestructura para granja de compilación y los trabajos de compilación generados
* http://wiki.ros.org/regression_tests#Setting_up_Your_Computer_for_Prerelease
* http://wiki.ros.org/buildfarm - Entrada en la wiki de ROS para la granja de compilación de ROS 1 (parcialmente *desactualizada*)
* https://github.com/ros-infrastructure/cookbook-ros-buildfarm - Instala y configura máquinas para Granjas de compilación ROS.


.. [1] ``{distro}`` is the first letter of the ROS distribution, ``{platform}`` (``{platf}``)
   names the platform the package is built for (and its short code), and ``{package}`` is the
   name of the ROS package being built.
.. [2] The *upstream* repository is the repository containing the original source code of the
   respective ROS 1 / ROS 2 package.
.. [3] The *release* repository is the repository that ROS 2 infrastructure uses for releasing
   packages, see https://github.com/ros2-gbp/.

.. _`release jobs`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/release_jobs.rst
.. _`devel jobs`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst
.. _`pull_request jobs`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst
.. _`CI jobs`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/ci_jobs.rst
.. _`doc jobs`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/doc_jobs.rst
.. _`miscellaneous jobs`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/miscellaneous_jobs.rst
.. _bloomed:
   http://wiki.ros.org/bloom
.. _rosdistro:
   https://github.com/ros/rosdistro
.. _`run the release jobs locally`:
   https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/release_jobs.rst#run-the-release-job-locally
.. _`Open Robotics`:
   https://www.openrobotics.org/
.. _`job descriptions above`:
   #jobs-and-deployment
.. _`package manifest`:
   http://wiki.ros.org/Manifest
