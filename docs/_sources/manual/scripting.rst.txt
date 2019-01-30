.. _scripting:

Scripting
=========

There are two main methods to open and browse IMAS IDS database using
:guilabel:`IMASViz`. First is the standard way of running IMASViz
(as described in :ref:`getting_started`) and setting the parameters in the GUI,
the second is through a script. Furthermore, some GUI actions (like node
selection, plot action commands etc.) can be performed through scripting too.
This way a simple IMASViz session can be set and populated on run.

This section describes the second method: how to create and use
**user-made** Python3 scripts that run and populate :guilabel:`IMASViz`.

:guilabel:`IMASViz scripting` can be seen as an
advanced alternative to the :guilabel:`Apply Configuration` features
(described in :ref:`apply_selection_from_configuration` and
:ref:`apply_multiplot_configuration`).

The procedure below can be used with either **IMASViz module** and
**IMASViz from source** but the environment **must be set accordingly to the
method the IMASViz is used (!)** (as described in
:ref:`running_on_gateway_as_a_module` and :ref:`running_from_source`).

.. _scripting_adding_pythonpath:

Adding IMASViz Path to PYTHONPATH
---------------------------------

.. warning:: Before proceeding, make sure that the environment is properly
             pre-set! This way also the **VIZ_HOME** system variable, required
             in this manual section, is available.

The IMASViz home directory can be added to **PYTHONPATH** system variable
by running in the terminal the command below (use the command that matches
your shell - **c-shell** or **bash**):

.. note:: **PYTHONPATH** is a "list" of paths that tell Python where to look
          for sources, libraries etc.

.. code-block:: console

    # c-shell (csh)
    setenv PYTHONPATH ${VIZ_HOME}:${PYTHONPATH}
    # bash
    export PYTHONPATH=${VIZ_HOME}:${PYTHONPATH}

.. _scripting_create:

Creating A Script
-----------------

This subsection will cover the basic procedure of writing a Python3 script that
can be used with :guilabel:`IMASViz`. Few such working script examples
are shown in section :ref:`scripting_examples`. The same examples can be found
in project GIT repository
`here <https://git.iter.org/projects/VIS/repos/viz/browse/imasviz/VizExamples?at=refs%2Fheads%2Fviz2.0_develop>`_.


1. First, the imports are required:
    - constants, functions, and methods of the Python interpreter,
    - PyQt5 classes and routines, and
    - IMASViz classes and routines.

    .. code-block:: Python

        # !/usr/bin/python
        # A module providing a number of functions and variables that can be used to
        # manipulate different parts of the Python runtime environment.
        import sys
        # PyQt library imports
        from PyQt5.QtWidgets import QApplication
        # IMASViz source imports
        from imasviz.Viz_API import Viz_API
        from imasviz.VizDataSource.QVizDataSourceFactory import QVizDataSourceFactory
        from imasviz.VizUtils.QVizGlobalOperations import QVizGlobalOperations
        from imasviz.VizGUI.VizGUICommands.VizMenusManagement.QVizSignalHandling \
            import QVizSignalHandlingds.VizMenusManagement.QVizSignalHandling \
                import QVizSignalHandling

2. Set object managing the PyQt GUI application's control flow:

    .. code-block:: Python

        app = QApplication(sys.argv)


3. Check if necessary system variables are set

    .. code-block:: Python

        QVizGlobalOperations.checkEnvSettings()

4. Set Application Program Interface

    .. code-block:: Python

        api = Viz_API()

5. Set data source retriever/factory

    .. code-block:: Python

        dataSourceFactory = QVizDataSourceFactory()

6. Load IMAS database and build the data tree view

    .. code-block:: Python

        f1 = api.CreateDataTree(dataSourceFactory.create(shotNumber=52344,
                                                 runNumber=0,
                                                 userName='g2penkod',
                                                 imasDbName='viztest'))

7. Add the build data tree view (DTV) to a list (!)

    .. code-block:: Python

        f = [f1]

8. Set the list of node paths

    .. code-block:: Python

        pathsList1 = []
        for i in range(0, 5):
            pathsList1.append('magnetics/flux_loop(' + str(i) + ')/flux/data')

9. Select signals corresponding to the list of node paths

    .. code-block:: Python

        api.SelectSignals(f1, pathsList1)

10. Show the data tree window

    .. code-block:: Python

        f1.show()

11. Plot selected nodes

    .. code-block:: Python

        f = [f1]
        api.PlotSelectedSignalsFrom(f)


12. Plot data from the first data source (f1) to Table Plot View

    .. code-block:: Python

        QVizSignalHandling(f1.dataTreeView).onPlotToTablePlotView(all_DTV=False)

13. Plot data from the first data source (f1) to Stacked Plot View

    .. code-block:: Python

        QVizSignalHandling(f1.dataTreeView).onPlotToStackedPlotView(all_DTV=False)

14. Keep the application running

    .. code-block:: Python

        sys.exit(app.exec_())


The final script is available below.

    .. code-block:: Python

        # !/usr/bin/python
        # A module providing a number of functions and variables that can be used to
        # manipulate different parts of the Python runtime environment.
        import sys
        # PyQt library imports
        from PyQt5.QtWidgets import QApplication
        # IMASViz source imports
        from imasviz.Viz_API import Viz_API
        from imasviz.VizDataSource.QVizDataSourceFactory import QVizDataSourceFactory
        from imasviz.VizUtils.QVizGlobalOperations import QVizGlobalOperations
        from imasviz.VizGUI.VizGUICommands.VizMenusManagement.QVizSignalHandling \
            import QVizSignalHandling

        # Set object managing the PyQt GUI application's control flow and main
        # settings
        app = QApplication(sys.argv)

        # Check if necessary system variables are set
        QVizGlobalOperations.checkEnvSettings()

        # Set Application Program Interface
        api = Viz_API()

        # Set data source retriever/factory
        dataSourceFactory = QVizDataSourceFactory()

        # Load IMAS database and build the data tree view
        f1 = api.CreateDataTree(dataSourceFactory.create(shotNumber=52344,
                                                        runNumber=0,
                                                        userName='g2penkod',
                                                        imasDbName='viztest'))

        # Add data tree view frame to list (!)
        f = [f1]

        # Set the list of node paths that are to be selected
        pathsList1 = []
        for i in range(0, 5):
            pathsList1.append('magnetics/flux_loop(' + str(i) + ')/flux/data')

        # Select signal nodes corresponding to the paths in pathsList
        api.SelectSignals(f1, pathsList1)

        # Show the data tree view window
        f1.show()

        # Plot signal nodes
        # Note: Data tree view does not need to be shown in order for this
        #       routine to work
        api.PlotSelectedSignalsFrom(f)

        # Plot data from the data source to Table Plot View
        QVizSignalHandling(f1.dataTreeView).onPlotToTablePlotView(all_DTV=False)

        # Plot data from the data source to Stacked Plot View
        QVizSignalHandling(f1.dataTreeView).onPlotToStackedPlotView(all_DTV=False)

        # Keep the application running
        sys.exit(app.exec_())

Running the script
------------------

With the environment set (done in :ref:`scripting_adding_pythonpath`) and
script completed (done in :ref:`scripting_create`), the script can be run
using the basic Python3 terminal command:

.. code-block:: console

    python3 <path_to_script>/<script_name>.py

By running this script all :guilabel:`Data Tree Views`, :guilabel:`Plot Widgets`
and :guilabel:`MultiPlot Views`, previously set in the script, should show,
as shown in the figure below.

.. figure:: images/scripting_run_result.png
  :align: center
  :width: 550px

  The result of running the script example:
  :guilabel:`Data Tree View (DTV)`, :guilabel:`Plot Widget`,
  :guilabel:`Table Plot View` and :guilabel:`Stacked Plot View`
  containing multiple plots.


.. _scripting_examples:

Script examples
---------------

Few complete script examples are shown below. The same examples can be found
in project GIT repository
`here <https://git.iter.org/projects/VIS/repos/viz/browse/imasviz/VizExamples?at=refs%2Fheads%2Fviz2.0_develop>`_.

Example 1
~~~~~~~~~

.. literalinclude:: ../../../imasviz/VizExamples/example1.py
   :language: python
   :linenos:

Example 2
~~~~~~~~~

.. literalinclude:: ../../../imasviz/VizExamples/example2.py
   :language: python
   :linenos:


Example 2b
~~~~~~~~~~

.. literalinclude:: ../../../imasviz/VizExamples/example2b.py
   :language: python
   :linenos:


Example 3
~~~~~~~~~

.. literalinclude:: ../../../imasviz/VizExamples/example3.py
   :language: python
   :linenos:


Example 4
~~~~~~~~~

.. literalinclude:: ../../../imasviz/VizExamples/example4.py
   :language: python
   :linenos:

