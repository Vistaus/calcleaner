Packaging CalCleaner
====================

In addition to the Python package itself, you will find in this folder various
useful files for Linux packaging, such as .desktop file to launch the
application or a man page file.

This folder also contains a script to copy all required files (desktop, icons
and manual) at the right location.

USAGE::

    ./copy-data.sh <PREFIX>

Where ``<PREFIX>`` is the prefix directory for the installation. For a real
installation, the prefix will be ``/usr``::

    ./copy-data.sh /usr

For a package, it may be your package's build folder::

    ./copy-data.sh /tmp/build-package/usr


Packaging
---------

Here we will see how to package CalCleaner for Linux.

To simplify commands, I will use a ``$ROOT`` varaible that contain the path
where the file will be installed. For a real installation this variable should
contain ``/``, for packaging it may be something like
``/tmp/my-packaging-build``.

All commands are run from the project root directory (the one that contains the
``setup.py`` file.


Build Dependencies
~~~~~~~~~~~~~~~~~~

Packaging will require some build dependencies:

* Python 3
* setuptools (search for a ``python3-setuptools`` package)
* nox (search for a ``python3-nox`` package)
* gettext

Except Python 3, the other dependencies are no more needed once the package is
built.


Release Tarball
~~~~~~~~~~~~~~~

You can get the release tarball on the `release page
<https://github.com/flozz/calcleaner/releases>`_. For example:

* https://github.com/flozz/calcleaner/archive/refs/tags/v1.0.0.tar.gz


Building the package
~~~~~~~~~~~~~~~~~~~~

The first step is to build the locales::

    nox -s locales_compile

Then the the Python package can be installed to the output directory::

    python3 setup.py install -O1 --skip-build --root $ROOT

And finally we can add additional files (.desktop, icons, man page,...)::

    ./linuxpkg/copy-data.sh $ROOT/usr
