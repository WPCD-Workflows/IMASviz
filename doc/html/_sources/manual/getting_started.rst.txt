.. _getting_started:

Getting Started
===============

This section describes setting the environment configuration required
to run the IMASViz tool and how to run the application itself.

.. _running_on_gateway_as_a_module:

Running IMASViz as a module on The GateWay
------------------------------------------

The procedure below describes how to use IMASViz if it is available as a
module on the HPC cluster.

Setting the Environment
~~~~~~~~~~~~~~~~~~~~~~~

In a new terminal, execute the following command in order to load the required
modules:

.. code-block:: console

    module load cineca
    module load imasenv   # or any other specific imasenv module version
    module load imas-viz/2.0.0

    # The next few modules should be loaded together with imas-viz/2.0.0
    # Listing them all here in case of module related issues.
    # module load itm-gcc/6.1.0
    # module unload itm-python/2.7
    # module load itm-python/3.6
    # module load itm-qt/5.8.0

.. Warning::
   **IMPORTANT!** IMAS databases (IDSs) were written using specific version of
   IMAS. In order to open these IDSs the **same IMAS module version** should
   be used due to possible IDS database structure changes through different
   versions. Any tools or utilities that work with IDSs, including ``IMASViz``,
   cannot work properly if this "IMAS version mismatch" is too great (!).

Running IMASViz
~~~~~~~~~~~~~~~

With the environment set, run the IMASviz by simply typing the following
command:

.. code-block:: console

    viz

The main GUI window of IMAS_VIZ should display, as shown in the figure below:

.. image:: images/startup_window_default.png
   :align: center
   :scale: 80%


The description of the above input parameters is as follows:

+--------------------+-----------------------------------------------------------+
| **GUI Fields**     | **Description**                                           |
+====================+===========================================================+
| User name          | Creator/owner of the IMAS IDSs database                   |
+--------------------+-----------------------------------------------------------+
| IMAS database name | IMAS database label, usually device/machine name of the   |
|                    | IMAS IDS database (i. e. iter, aug, west...)              |
+--------------------+-----------------------------------------------------------+
| Shot number        | Pulse shot number                                         |
+--------------------+-----------------------------------------------------------+
| Run number         | Pulse run number                                          |
+--------------------+-----------------------------------------------------------+

Available benchmark IMAS databases
----------------------------------

On the GateWay HPC there are a few **benchmark IMAS IDS cases** available. These
databases are the main source of data used for IMASViz testing purposes and
were also included in writing the this documentation. Users can freely use them
for examples and practice purposes.

.. Note:: There IMAS IDS cases are confirmed to work with IMAS versions
          **3.19.1** - **3.20.0**.

+-----------------------------------------------------+
| **Available IMAS IDS Case Parameters**              |
+--------------------+----------+----------+----------+
| Parameters         | Case 1   | Case 2   | Case 3   |
+====================+==========+==========+==========+
| User name          | g2penkod | g2penkod | g2penkod |
+--------------------+----------+----------+----------+
| IMAS database name | viztest  | viztest  | viztest  |
+--------------------+----------+----------+----------+
| Shot number        | 52344    | 52682    | 53223    |
+--------------------+----------+----------+----------+
| Run number         | 0        | 0        | 0        |
+--------------------+----------+----------+----------+

.. _running_from_source:

Running IMASViz from source
---------------------------

The procedure below describes how to run IMASViz from source.

.. _IMASVIZ_requirements:

Requirements
~~~~~~~~~~~~

The fundamental requirements in order to locally run IMASViz are:

- IMAS
- Python3 and Python libraries:
   - PyQt5
   - pyqtgraph
   - matplotlib
   - Sphinx (:command:`pip3 install sphinx`)
   - Sphinx RTD theme (:command:`pip3 install sphinx_rtd_theme`)

.. _source_code:

Obtaining the source code
~~~~~~~~~~~~~~~~~~~~~~~~~

To obtain the IMASViz code source the next two steps are required:

1. Clone repository from **git.iter.org** (permissions are required!).

   Direct link to the **IMASViz** git.iter repository:
   `IMASViz <https://git.iter.org/projects/VIS/repos/viz/browse>`_.

2. Switch to IMASViz2.0 branch (required if master branch is not updated yet)

   .. code-block:: console

      git fetch # optional
      git branch -r # optional
      git checkout viz2.0_develop

Setting the environment
~~~~~~~~~~~~~~~~~~~~~~~

To set the environment, go to :file:`viz` directory and set :guilabel:`VIZ_HOME`
and :guilabel:`VIZ_PRODUCTION` environment variables by running the next
commands in the terminal:

.. code-block:: console

   cd viz
   # bash
   export VIZ_PRODUCTION=0
   export VIZ_HOME=$PWD
   # csh
   setenv VIZ_PRODUCTION 0
   setenv VIZ_HOME $PWD

Then proceed with the next instructions.

GateWay HPC
^^^^^^^^^^^

Load next modules:

.. TODO: Update for IMASViz2.0
.. code-block:: console

    module load cineca
    module load imasenv
    module load itm-gcc/6.1.0
    module load itm-python/3.6
    module load itm-qt/5.8.0

ITER HPC
^^^^^^^^

Load next module:

.. code-block:: console

    module load IMAS/3.20.0-3.8.3

Running IMASViz
~~~~~~~~~~~~~~~

To run IMASViz, run the next commands in terminal:

.. code-block:: console

   python3 $VIZ_HOME/imasviz/VizGUI/QtVIZ_GUI.py

The main GUI window of IMAS_VIZ should display, as shown in the figure below:

.. image:: images/startup_window_default.png
   :align: center
   :scale: 80%

The description of the above input parameters is as follows:

+--------------------+-----------------------------------------------------------+
| **GUI Fields**     | **Description**                                           |
+====================+===========================================================+
| User name          | Creator/owner of the IMAS IDSs database                   |
+--------------------+-----------------------------------------------------------+
| IMAS database name | IMAS database label, usually device/machine name of the   |
|                    | IMAS IDS database (i. e. iter, aug, west...)              |
+--------------------+-----------------------------------------------------------+
| Shot number        | Pulse shot number                                         |
+--------------------+-----------------------------------------------------------+
| Run number         | Pulse run number                                          |
+--------------------+-----------------------------------------------------------+


Latest documentation and manual
-------------------------------

The documentation provided on other sources (confluence pages etc.) than the
project repository might not be up to date. To get the latest documentation,
first obtain the IMASViz source code (see :ref:`source_code`).

Then navigate to

.. code-block:: console

    cd $VIZ_HOME/doc

and run

.. code-block:: console

    # for PDF documentation
    module load texlive
    make pdflatex
    xdg-open build/latex/IMASViz.pdf
    # for HTML documentation
    make html
    firefox build/html/index.html

.. Note:: Additional prerequisites for generating the documentation:
          **Sphinx** and **Sphinx RTD** theme (listed in
          :ref:`IMASViz_requirements`)
