.. _plugin_simple_plot_example:

Developing a simple plugin for plotting data from IDSs
======================================================

.. note::
    Before proceeding with the plugin development instructions make sure that
    you have **cloned IMASViz GIT repository** (for more information see section
    :ref:`running_from_source`)!

This section will cover step by step instructions on how to:

 - write a very simple introductory plugin example (~80 lines of code),
 - how to register the plugin in IMASViz, and
 - how to run the plugin in IMASViz.

This simple plugin can be considered also as a starting template for more
complex plugins with advanced design and functionalities.

The main basic steps which will be covered in this tutorial are as follows:

1. Adding IMASViz home directory to :envvar:`$PYTHONPATH`
2. Setting plugin source structure
3. Writing the code:

   a) Import statements
   b) Class (prescribed name)
   c) Class inheritance
   d) Mandatory method functions

4. Registering plugin in IMASViz

.. note::
    Complete code of the simple plugin (named as **simplePlotPluginExample**) is
    available in IMASViz code source in
    :file:`$VIZ_HOME/imasviz/VizPlugins/viz_simple_plot_example`.

Adding IMASViz sources to $PYTHONPATH
-------------------------------------

In order to have IMASViz sources at our disposal the ``$VIZ_HOME`` must be added
to :envvar:`$PYTHONPATH` environment variable. This can be achieved by running
the next command in the terminal:

.. code-block::

    # bash
    export PYTHONPATH=${VIZ_HOME}:${PYTHONPATH}
    # tcsh
    setenv PYTHONPATH ${VIZ_HOME}:${PYTHONPATH}

.. _plugin_simple_plot_example_setting_dir:

Setting plugin source structure
-------------------------------

All plugin source files must be stored in a separate directory under
:file:`$VIZ_HOME/imasviz/VizPlugins/<folder_name>`. The name of the directory
should start with "**viz_**".

For the purposes of this tutorial create a new directory with label
**viz_my_example** to distinguish it from other plugins. The full path is then
:file:`$VIZ_HOME/imasviz/VizPlugins/viz_my_plugin`.

Inside the newly created directory create a new Python script file. In this case
name it as :file:`myPlugin.py`. The script can be left empty for now.

Python code
-----------

This subsection will cover the contents of the plugin main Python script file
:file:`$VIZ_HOME/imasviz/VizPlugins/viz_my_plugin/myPlugin.py` (previously
created in :ref:`plugin_simple_plot_example_setting_dir`).

Required import statements
^^^^^^^^^^^^^^^^^^^^^^^^^^

As the first entry in the Python script, the next modules need to be imported
with the use of the import statements.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    # modules for event logging system and for operating system dependent
    # functionality
    import logging, os
    # IMASViz plugin sources
    from imasviz.VizPlugins.VizPlugin import VizPlugin
    # Matplotlib library
    import matplotlib.pyplot as plt

Prescribed class name
^^^^^^^^^^^^^^^^^^^^^

The plugins main Python file must contain a class with the same name as the
name of the Python file. In this case, a class **myPlugin**.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    class myPlugin():

Inheritance
^^^^^^^^^^^

The class must inherit from **VizPlugin class** from the :file:`VizPlugin.py`.
This is required for IMASViz to be able to gather necessary information
required for properly running the plugin.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    class myPlugin(VizPlugin):

Mandatory method functions
^^^^^^^^^^^^^^^^^^^^^^^^^^

The plugin class must contain 5 mandatory method functions (besides constructor):

- **execute(self, vizAPI, pluginEntry)**
- **getEntries(self)**
- **getPluginsConfiguration(self)**
- **getAllEntries(self)**
- **isEnabled(self)**

Constructor
"""""""""""

In this case, leave the constructor empty.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    def __init__(self):
        pass

execute()
"""""""""

The :guilabel:`execute()` function consists of three parts:

1. Obtaining data source from IMASViz
2. Checking if the IDS data was already fetched
3. Extracting and plotting the data from the IDS

**1. Obtaining data source (IDS object) from IMASViz:**

This is done through **vizAPI** - **the IMASViz Application Program Interface
(API)** and its **GetDataSource** function.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    # Get dataSource from the VizAPI (IMASViz Application Program Interface)
    # Note: instance of "self.datatreeView" is provided by the VizPlugins
    # through inheritance
    dataSource = vizAPI.GetDataSource(self.dataTreeView)
    # Get case parameters (shot, run, machine user) from the dataSource
    shot = dataSource.shotNumber
    run = dataSource.runNumber
    machine = dataSource.imasDbName
    user = dataSource.userName
    occurrence = 0

    # Displaying basic case information
    print('Reading data...')
    print('Shot    =', shot)
    print('Run     =', run)
    print('User    =', user)
    print('Machine =', machine)

**2. Checking if the IDS data was already fetched**

With the use of functions provided by **vizAPI** we can be check if the
case (IDS) data was already fetched (loaded in memory) while running IMASViz.
In case the data was not yet fetched it can be done with the use of the
**LoadIDSData** function (with the use of this function also the IMASViz data
tree view browser gets updated automatically).

The IDS object is then obtained with the use of **getImasEntry()** function
for given occurrence (default occurrence value is 0).

In your :file:`myPlugin.py` file add:

.. code-block:: python

    # Check if the IDS data is already loaded in IMASviz. If it is not,
    # load it
    if not vizAPI.IDSDataAlreadyFetched(self.dataTreeView, 'magnetics', occurrence):
        logging.info('Loading magnetics IDS...')
        vizAPI.LoadIDSData(self.dataTreeView, 'magnetics', occurrence)

    # Get IDS object
    self.ids = dataSource.getImasEntry(occurrence)

**3. Extracting and plotting the data from the IDS**

With the IDS object available its contents can be easily accessed (following the
structure defined by the :guilabel:`Data Dictionary`). The data can be then
plotted with the use of the :guilabel:`Matplotlib` Python library
(`link <https://matplotlib.org/>`_).

This plugin example will read some simple data from the **Magnetics IDS**
and plot it using **Matplotlib** plitting utilities:

- **time values** (stored in ``magnetics.time`` node) -> **X axis**
- **poloidal field probe values** (stored in ``magnetics.bpol_probe`` array of
  structures (AOS). The values are stored in
  ``magnetics.bpol_probe[i].field.data`` where :math:`i` is the array index)
  -> **Y axis**

In your :file:`myPlugin.py` file add:

.. code-block:: python

    # Get some data from the IDS and pass it to plot (using matplotlib)
    # - Set subplot
    fig, ax = plt.subplots()
    # - Extract X-axis values (time)
    time_values = self.ids.magnetics.time
    x = time_values
    # - Get the size of AoS (number of arrays)
    num_bpol_probe_AoS = len(self.ids.magnetics.bpol_probe)
    # - For each array extract array values and create a plot
    for i in range(num_bpol_probe_AoS):
        # - Extract array values
        y = self.ids.magnetics.bpol_probe[i].field.data
        # - Set plot (line) defined by X and Y values +
        # - set line as full line (-) and add legend label.
        ax.plot(x, y, '-', label='bpol_probe[' + str(i) + ']')
    # - Enable grid
    ax.grid()
    # - Set axis labels and plot title
    ax.set(xlabel='time [s]', ylabel='Poloidal field probe values',
           title='Poloidal field probe')
    # - Enable legend
    ax.legend()
    # - Draw/Show plots
    plt.show()

getEntries()
""""""""""""

The :guilabel:`getEntries()` method function provides IMASViz the information to
which IDS the plugin is associated. While in the IMASViz tree view browser,
the plugin will be then accessible by right-clicking on here defined IDS
(the option for running this plugin gets shown in the popup menu).

In this case, as the plugin deals with the data stored in **Magnetics IDS**,
this option should be set to ``"magnetics"`` as shown in the code part below.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    def getEntries(self):
        if self.selectedTreeNode.getIDSName() == "magnetics":
            return [0]

getPluginsConfiguration()
"""""""""""""""""""""""""


The :guilabel:`getPluginsConfiguration()` method function provides additional
configurations to IMASViz. In this case no additional configurations are
required -> the function returns value **None**.

In your :file:`myPlugin.py` file add:

.. code-block:: python

    def getPluginsConfiguration(self):
        return None

getAllEntries()
"""""""""""""""

The :guilabel:`getAllEntries()` method function provides IMASViz 'cosmetic'
information (e.g. label which should be shown in the popup menu etc.).

In your :file:`myPlugin.py` file add:

.. code-block:: python

    def getAllEntries(self):
        # Set a text which will be displayed in the pop-up menu
        return [(0, 'My plugin...')]


isEnabled()
"""""""""""

Through the :guilabel:`isEnabled()` method function the custom plugin can be
either **enabled** (returns ``True``) or **disabled** (returns ``False``).

In your :file:`myPlugin.py` file add:

.. code-block:: python

    def isEnabled(self):
        return True

Full Python code
^^^^^^^^^^^^^^^^

Below is a full code in :file:`myPlugin.py` which is done by following the
steps in the previous subsections.

.. code-block:: python
    :caption: myPlugin.py
    :linenos:

    # modules for event logging system and for operating system dependent
    # functionality
    import logging, os
    # IMASViz plugin sources
    from imasviz.VizPlugins.VizPlugin import VizPlugin
    # Matplotlib library
    import matplotlib.pyplot as plt

    class myPlugin(VizPlugin):

        def __init__(self):
            pass

        def execute(self, vizAPI, pluginEntry):
            """Main plugin function.
            """

            # Get dataSource from the VizAPI (IMASViz Application Program Interface)
            # Note: instance of "self.datatreeView" is provided by the VizPlugins
            # through inheritance
            dataSource = vizAPI.GetDataSource(self.dataTreeView)
            # Get case parameters (shot, run, machine user) from the dataSource
            shot = dataSource.shotNumber
            run = dataSource.runNumber
            machine = dataSource.imasDbName
            user = dataSource.userName
            occurrence = 0

            # Check if the IDS data is already loaded in IMASviz. If it is not,
            # load it
            if not vizAPI.IDSDataAlreadyFetched(self.dataTreeView, 'magnetics', occurrence):
                logging.info('Loading magnetics IDS...')
                vizAPI.LoadIDSData(self.dataTreeView, 'magnetics', occurrence)

            # Get IDS
            self.ids = dataSource.getImasEntry(occurrence)

            # Displaying basic information
            print('Reading data...')
            print('Shot    =', shot)
            print('Run     =', run)
            print('User    =', user)
            print('Machine =', machine)

            # Get some data from the IDS and pass it to plot (using matplotlib)
            # - Set subplot
            fig, ax = plt.subplots()
            # - Extract X-axis values (time)
            time_values = self.ids.magnetics.time
            x = time_values
            # - Get the size of AoS (number of arrays)
            num_bpol_probe_AoS = len(self.ids.magnetics.bpol_probe)
            # - For each array extract array values and create a plot
            for i in range(num_bpol_probe_AoS):
                # - Extract array values
                y = self.ids.magnetics.bpol_probe[i].field.data
                # - Set plot (line) defined by X and Y values +
                # - set line as full line (-) and add legend label.
                ax.plot(x, y, '-', label='bpol_probe[' + str(i) + ']')
            # - Enable grid
            ax.grid()
            # - Set axis labels and plot title
            ax.set(xlabel='time [s]', ylabel='Poloidal field probe values',
                   title='Poloidal field probe')
            # - Enable legend
            ax.legend()
            # - Draw/Show plots
            plt.show()

        def getEntries(self):
            if self.selectedTreeNode.getIDSName() == "magnetics":
                return [0]

        def getPluginsConfiguration(self):
            return None

        def getAllEntries(self):
            # Set a text which will be displayed in the pop-up menu
            return [(0, 'Magnetics overview (simple plot plugin example)...')]

        def isEnabled(self):
            return True

Registering plugin in IMASViz
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to register the plugin in IMASViz, a single entry is required in the
top part of the :file:`$VIZ_HOME/imasviz/VizPlugins/VizPlugin.py` file.

In the :guilabel:`RegisteredPlugins` dictionary add key and corresponding value
relevant for your plugin, e.g. :kbd:`'myPlugin' : 'viz_my_plugin.myPlugin'`.

Here the key must match the **py. file** and **class name** while the
corresponding value must match :kbd:`'<plugin_source_path>.<py_file_name.py>'`.

In this case, it should look something like this:

.. code-block:: python
    :emphasize-lines: 9

    RegisteredPlugins = {'equilibriumcharts':'viz_equi.equilibriumcharts',
                         'ToFuPlugin':'viz_tofu.viz_tofu_plugin',
                         'SOLPS_UiPlugin': '',
                         'CompareFLT1DPlugin':'viz_tests.CompareFLT1DPlugin',
                         'viz_example_plugin':'viz_example_plugin.viz_example_plugin',
                         'example_UiPlugin': '',
                         'simplePlotPluginExample' : 'viz_simple_plot_example.simplePlotPluginExample',
                         'ETSpluginIMASViz' : 'viz_ETS.ETSpluginIMASViz',
                         'myPlugin' : 'viz_my_plugin.myPlugin'
                         }

Executing the custom plugin in IMASViz
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To run the plugin in IMASViz while in IMASViz session with opened IDS database,
in :guilabel:`tree view browser`:

- on the IDS previously specified in :guilabel:`getEntries()`
  method function (**Magnetics IDS**) either:

  - hold shift key + right-click on the tree node. A popup menu including the
    menu action (with label previously specified in
    :guilabel:`getAllEntries()`) will be shown, or

   .. figure:: images/IMASViz_simple_plot_example_plugin_menu_1.png
     :align: center
     :scale: 90%

     Popup menu on shift + right-click on **Magnetics IDS** showing the
     available action for executing the plugin.

  - just right-click on the tree node. A popup menu including the
    :guilabel:`Plugin` selection will be shown. Hovering on this selection will
    shown the menu action (with label previously specified in
    :guilabel:`getAllEntries()`).

   .. figure:: images/IMASViz_simple_plot_example_plugin_menu_2.png
     :align: center
     :scale: 80%

     Popup menu on right-click on **Magnetics IDS** showing the
     available action for executing the plugin.

- click on the menu action. The plugin will be executed and the results (plot)
  will be shown in a matplotlib plot window.

   .. figure:: images/IMASViz_simple_plot_example_plugin_result.png
     :align: center
     :scale: 80%

     The result of the simple plot plugin execution: plotted poloidal field
     probe values (all available signals).
