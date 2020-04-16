.. _plugins_qtdesigner:

Developing a custom user interface (UI) plugins with Qt designer
================================================================

**Qt Designer** is a tool for designing and building **Qt-based graphical user**
**interfaces**. It allows the user to design custom widgets, dialogs, main
windows, etc. using on-screen forms and a user-friendly simple drag-and-drop
interface. It also provides the user with a convenient ability to preview the
designs to ensure they work as intended.

In general, Qt Designer mainly offers basic Qt widgets such as
:guilabel:`Push Button`, :guilabel:`Line Edit`, :guilabel:`List Widget`, etc.
This list of the **Qt Designer** widgets can be
extended by writing so-called **Qt designer plugins** (do not confuse with
**IMASViz plugins**!). Normally this is done using C++ but PyQt5 also allows
you to write Qt Designer plugins in Python3 programming language.

Such designer plugin is used to pass a custom widget source code
(written in Python3) to Qt Designer. This way the widget becomes available
within the Qt designer where it can be interactively moved, designed, connected
to signals and slots and more.

.. note::
   For more information on Qt Designer and PyQt5 based plugins and widgets
   check `this link <http://pyqt.sourceforge.net/Docs/PyQt5/designer.html>`_.

In this HOWTO section it will be described how to:
  #. Develop a **custom PyQt5 widget**
  #. Pass the **custom PyQt5 widget** class to **Qt designer** as a
     **Qt designer plugin**
  #. Use the **custom PyQt5 widget** as a **Qt designer plugin** within
     the Qt Designer
  #. Design of a custom **user interface (UI) plugin** (which includes
     the custom **Qt designer plugin**) with Qt designer
  #. Use the **UI plugin** in a standalone way as a
     **PyQt5 application**
  #. Use the **UI plugin** in **IMASViz**

.. Warning::
   Qt version of used PyQt5 (compiled with Qt) and Qt designer must match!

For the purposes of this HowTo section, a widget source code for the
**Magnetics IDS overview Plugin** was developed and it is available in the
IMASViz source code (VizPlugins/viz_example). As it is mainly intended only
as an example of a plugin (including an example of the widget source code), it
is referred to mainly as an **Example Plugin** (same goes for source files -
exampleWidget.py and exampleplugin.py, introduced later in this howTo section).

As and addition, below is a short demonstration video of
**SOLPS overview Plugin**, showing an example of the processes listed in
points **3-6**. More on this plugin (as IMASViz plugin) can be found in section
:ref:`IMASViz_plugins`.

.. only:: html

   .. raw:: html

      <video controls width="600" src="../_static/QtDesigner_and_IMASViz_plugin_short_demo.mp4"></video>


.. only:: latex

   .. TODO: requires the .mp4 file to be in the same directory as the .pdf file

   `Local Video Link <QtDesigner_and_IMASViz_plugin_short_demo.mp4>`_.

Custom PyQt5 widget creation (code development)
-----------------------------------------------

This section describes and demonstrates how to write a complete custom PyQt5
widget that handles data stored within the **Magnetics IDS**. Later in this
HowTo section, the same widget will be then used to create
**Magnetics IDS overview Plugin** using Qt designer.

The main final features of this custom plugin will be:

-
  - **Opening and reading the contents a specified IDS** (specified by the set
    case parameters)

  **OR**

  - **reading the contents of an IDS which was passed to the plugin by "host"
    application**, and
- convenient plotting of all **flux_loop** and **poloidal field probe** (AoS)
  signals found in the **Magnetics IDS** (arrays of values).

In this case, the whole widget source code is written in Python3 file named
**exampleWidget.py**.

The final code can be already observed and compared here:
:ref:`exampleWidget_code`.

.. Note::
   It is highly recommended to have the finished code opened on the side while
   going through this HowTo section to better understand the whole code.

.. Note::
   It is recommended to have at least basic knowledge from programming
   (especially with Python programming language) before proceeding with the
   widget development instructions. A complete beginner might find those
   instructions a bit overwhelming.

The steps are split into the next subsections:

  1. Code header

  2. Import statements

  3. Widget Class definition

      3.1 Widget Constructor definition

      3.2 Widget base "set" and "get" routines

      3.3 Widget custom routines

  4. PlotCanvas Class definition

      4.1 PlotCanvas Constructor definition

      4.2 PlotCanvas custom plotting routines

  5. Running the code as the main program (standalone)

Code header
^^^^^^^^^^^

The header of the custom PyQt5 widget source code should contain the basic
information about the code itself:
- the source code (.py) filename,
- short description and the purpose of this script
- authors name and
- authors contact (e-mail is most convenient).

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 1
   :lines: 1-16
   :linenos:

Documentation should be as important to a developer as all other facets of
development. **Every code should include documentation** (in the forms of a
header, code comments, etc.). It either explains what the code does, how it
operates, how to use it etc. Documentation is an important part of software engineering.

No matter what the code contains, chances are that someday other
users will try to understand and use it. Taking that extra time to write a
proper description of the contents of the code will save huge amounts of time
and effort for everybody to understand the code.

.. _plugins_qtdesigner_import_statement:

Import statements
^^^^^^^^^^^^^^^^^

The custom PyQt5 widget requires additional sources - modules.

The ones required in this case are:

- The common system, OS and logging modules:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 18
   :lines: 18-23
   :linenos:

- PyQt5 modules:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 24
   :lines: 24-26
   :linenos:

- Matplotlib modules and setting matplotlib to use the Qt rendering:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 27
   :lines: 27-32
   :linenos:
   :emphasize-lines: 3

- IMAS modules:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 33
   :lines: 33-34
   :linenos:

.. _plugins_qtdesigner_widget_class:

Widget class
^^^^^^^^^^^^

This section describes and demonstrates how to define a new widget class in
Python3.

Class definition
""""""""""""""""

The initial and important part of this code is the definition of a new class
inheriting from the PyQt5 **QWidget** class. In this case, the class is named
:guilabel:`exampleWidget`.

This class will later fully define the QWidget (contents, design, functions
related to the widget and more).

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 36
   :lines: 36-39
   :linenos:

.. note::
   Do not forget to describe the class - what is its purpose etc.

Here also a new PyQt signal is set, which will be needed later in code.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 41
   :lines: 41-42
   :linenos:

In short, the signal on its own does not perform any action. Instead, it is
``connected`` to a ``slot``. The slot can be any callable Python function.
When the signal gets emitted the connected slot (Python function) gets called.

.. note::
   More on signal and slots:
   `Link <https://www.tutorialspoint.com/pyqt/pyqt_signals_and_slots.html>`_

.. _widget_constructor:

Constructor definition
""""""""""""""""""""""

In short, constructors are generally used for initiating an object. The task of
constructors is to initialize the data members of the class when an object of
the class is created.

In the case of this custom widget, the constructor required two additional
arguments:

- :guilabel:`parent` (can be either Qt object, later our case QMainWindow), and

- :guilabel:`ids` (an IDS object).

Both arguments are set as **None** (default values).

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 44
   :lines: 44-57
   :linenos:

And the :guilabel:`ids` object is set with:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 65
   :lines: 65-67
   :linenos:

Regarding the :guilabel:`ids` object, the main idea is to make our widget
capable of performing in two different ways. Either:

- use IDS object passed to the widget (in which case **ids != None**),
- if no IDS object was passed (**ids == None**), open IDS and create a new
  :guilabel:`ids` object.

For example, in **IMASViz** there is at least one IDS open at the time and thus
have the :guilabel:`ids` object available. Instead of opening the IDS again,
the :guilabel:`ids` object can be just passed to the custom widget as an
argument and the widget can continue to use it.

If there is no IDS object available (meaning no IDS is already being
opened), an IDS must be opened thus creating an object referring to the IDS.
In the constructor we define a dictionary labeled as :guilabel:`self.idsParameters`
which should contain all IDS parameters for IDS (will be used later to open the
needed IDS):

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 69
   :lines: 69-83
   :linenos:

Constructor should contain also a check if the widget is being run in a desktop
environment. This is mandatory as this is a widget which deals with GUI and
visualization. The code should not be run from a "terminal-only" environment (for
example ``ssh user@host`` etc.).

In this case we define a function named :guilabel:`checkDisplay()`:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 188
   :lines: 188-193
   :linenos:

and execute it in the constructor:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 61
   :lines: 61-63
   :linenos:

Lastly, a widget layout and its contents need to be defined (plot canvas and
matplotlib navigation toolbar):

.. note::
   The **PlotCanvas** class definition and the definition of its routines
   will be done in the following sections.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 85
   :lines: 85-93
   :linenos:

Base "set" and "get" routines
"""""""""""""""""""""""""""""

For setting and getting/returning the IDS case parameters, the definition of
a few :guilabel:`set/get` routines is required.

The :guilabel:`set` routines must be set as **slots (@pyqtSlot)**. This clearly
marks the function as a slot for PyQt5 and it also increases its speed and
performance when being executed as slots in PyQt5 applications.

The :guilabel:`get` routines are simple functions which return one variable
value.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 146
   :lines: 146-186
   :linenos:

Custom functions (routines)
"""""""""""""""""""""""""""

The first "bundle" of functions deals with IDSs:

1. :guilabel:`openIDS`: for opening the IDS (using the IDS case parameters,
defined with the :guilabel:`self.idsParameters` dictionary),

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 125
   :lines: 125-136
   :linenos:

2. :guilabel:`setIDS`: for setting the IDS object (:guilabel:`self.ids`). Here also the
**emit signal** statement is included. This way every time this function will be
called/executed, this signal will get emmited. Later in the plugin this signal
will be used to initiate the execution of certain functions on signal-emit.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 138
   :lines: 138-141
   :linenos:

3. :guilabel:`getIDS`: for getting/returning the IDS object
(:guilabel:`self.ids`).

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 143
   :lines: 143-144
   :linenos:

The second "bundle" of functions deals with executing the plotting procedures
to populate the **matplotlib canvas**. At this point in this tutorial, the
**PlotCanvas** class is not yet defined. This will be done in the next HowTo
section. The functions needed are:

- :guilabel:`plotFluxAoS`: for plotting all **Flux_loop** signal arrays values,
  and
- :guilabel:`plotBPolAoS`: for plotting all **poloidal field probe** signal
  arrays values

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 95
   :lines: 95-123
   :linenos:

PlotCanvas
^^^^^^^^^^

This section describes and demonstrates how to define a new matplotlib
FigureCanvas class in Python3.

Class definition
""""""""""""""""

Second main part this code (with the first being the definition of the
:guilabel:`exampleWindget` class) is the definition of a new class inheriting
from the matplotlib :guilabel:`FigureCanvas` class. In this case, the class
is named :guilabel:`PlotCanvas`.

This class will later fully define the matplotlib plot frame (canvas) and
functions related to it.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 195
   :lines: 195-198
   :linenos:

Constructor definition
""""""""""""""""""""""

In this case the constructor takes 4 additional arguments:

- :guilabel:`parent` (our custom QWidget),
- :guilabel:`width` (canvas width),
- :guilabel:`height` (canvas height), and
- :guilabel:`dpi` (dots per inch).

The :guilabel:`parent` argument is set as **None**, :guilabel:`width` to **5**,
:guilabel:`height` to **5** and :guilabel:`dpi` to **100** (default values).

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 200
   :lines: 200-207
   :linenos:

Next, a figure object :guilabel:`fig` is set:

.. Note::
   **Figure** routine is taken from the import statement (see
   :ref:`plugins_qtdesigner_import_statement`).

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 109
   :lines: 109-210
   :linenos:

Next, the (by class) inherited :guilabel:`FigureCanvas` constructor is executed.
The :guilabel:`fig` object is passed to it as an argument. This way the
**figure** is embedded within the **matplotlib canvas**.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 211
   :lines: 211-212
   :linenos:

Next, the **parent** of the :guilabel:`FigureCanvas` is set:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 213
   :lines: 213-214
   :linenos:

Lastly, the :guilabel:`FigureCanvas` **size policy** is set.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 215
   :lines: 215-218
   :linenos:

The whole :guilabel:`PlotCanvas` constructor code:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 200
   :lines: 200-218
   :linenos:


Custom plotting functions
"""""""""""""""""""""""""

There are two plotting functions required:

1. :guilabel:`plotFluxAoS`, for plotting all **flux_loop** signal arrays, and
2. :guilabel:`plotBPolAoS`, for plotting all **poloidal field probe** signal
   arrays values.

Both are very similar, the only difference between them is which data is
extracted from the IDS and used for plotting. Because of this similarity, only
the function :guilabel:`plotFluxAoS` will be described in depth.

The function :guilabel:`plotFluxAoS` requires only one argument: the IDS object.
The function must also set the provided :guilabel:`ids` object to
:guilabel:`self.ids` object.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 220
   :lines: 220-233
   :linenos:

Next, figure subplot must be set:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 234
   :lines: 234-235
   :linenos:

Next, the time values must be extracted and assigned to :guilabel:`time_values`
array. The time values will correspond to plot X-axis, thus, for easier
representation, a new array :guilabel:`x` can be defined and the same values
assigned to it.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 236
   :lines: 236-238
   :linenos:

Next, looping through all structured of the **flux_loop** AoS is required.
Each array values (Y-axis values) need to be extracted and then together with
the previously set time values (X-axis) a new plot can be added to the
matplotlib figure. Because of the loop, this gets repeated until no more AoS
arrays are left.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 239
   :lines: 239-247
   :linenos:

Next, few additional modifications are required:

- enabling plot grid,
- setting X-axis, Y-axis label,
- setting plot title,
- enabling legend, and
- drawing the plot.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 248
   :lines: 248-256
   :linenos:

Final :guilabel:`plotFluxAoS` code:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 220
   :lines: 220-256
   :linenos:

As already stated, :guilabel:`plotBPolAoS` function code is almost identical
to :guilabel:`plotFluxAoS` code.

Final :guilabel:`plotBPolAoS`:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 258
   :lines: 258-293
   :linenos:

At this point the **exampleWidget.py** code is finished and ready for use.

Running the code as the main program (standalone)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To run this example widget in a standalone way, few more lines must be added to
the **exampleWidget.py**.

This part of the code contains setting the :guilabel:`QApplication`,
:guilabel:`QMainWindow`, initiating the :guilabel:`exampleWidget` class,
reading the IDS (parameters are set in the :guilabel:`exampleWidget`
constructor) and executing the plotting procedures.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :lineno-start: 295
   :lines: 295-317
   :linenos:

The code can now be run from the terminal with the next command:

.. code-block:: console

    python3 exampleWidget.py

.. Note::
   Make sure that the IDS with the specified case parameters exists (done in
   :ref:`widget_constructor` using :guilabel:`idsParameters` dictionary)!

.. figure:: images/exampleWidget_standalone_run_flux_loop.png
  :align: center
  :width: 550px

  **exampleWidget.py**: Plotting all **magnetics IDS** arrays (17) of AoS
  **flux_loop** found in IDS (on GateWay HPC) shot: 52344;   run: 0;
  user: g2penkod; device: viztest.

.. figure:: images/exampleWidget_standalone_run_bpol_probe.png
  :align: center
  :width: 550px

  **exampleWidget.py**: Plotting all **magnetics IDS** arrays (ABOUT 130) of AoS
  **bpol_probe** found in IDS (on GateWay HPC) shot: 52344;   run: 0;
  user: g2penkod; device: viztest.

.. _exampleWidget_code:

Full final code of the example PyQt5 widget
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleWidget.py
   :language: python
   :linenos:

Passing custom PyQt5 widget to Qt designer
------------------------------------------

In order to pass the **custom PyQt5 widget** to **Qt designer**, a separate
Qt plugin Python file is required, written in Python3 programming language.
**The name of this file is of major importance** as if set improperly the
Qt designer will not recognize it!
The name of this file should end with **plugin.py** (**case
sensitive!**). In this case, the file is named **exampleplugin.py**. it must
be placed in the same directory as the widget source code - **exampleWidget.py**.

This plugin *.py* file for Qt designer follows a certain template which can be
used and slightly modified as required..

The whole code is shown below.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :linenos:

Below are listed lines of the Qt plugin code, which must be modified for any
new widget, in order to properly refer to the widget source code - in this case
**exampleWidget** (exampleWidget.py).

1. Import statement:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 13
   :lines: 13
   :linenos:

2. Class label:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 15
   :lines: 15
   :linenos:

3. Class constructor:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 18
   :lines: 18-19
   :linenos:
   :emphasize-lines: 2

4. Returning custom widget object on :guilabel:`createWidget`.

.. Note::
   If widget constructor requires arguments they must be included here! In this
   case :guilabel:`parent` and :guilabel:`ids`.

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 21
   :lines: 21-22
   :linenos:
   :emphasize-lines: 2

5. Name:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 24
   :lines: 24-25
   :linenos:
   :emphasize-lines: 2

6. Group:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 27
   :lines: 27-28
   :linenos:
   :emphasize-lines: 2

7. Tool tip:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 33
   :lines: 33-34
   :linenos:
   :emphasize-lines: 2

8. XML attribute definition:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 42
   :lines: 42-43
   :linenos:
   :emphasize-lines: 2

9. Include file:

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 45
   :lines: 45-46
   :linenos:
   :emphasize-lines: 2

10. Icon - pixmap (optional):

.. literalinclude:: ../../../../imasviz/VizPlugins/viz_example/exampleplugin.py
   :language: python
   :lineno-start: 48
   :lines: 48-72
   :linenos:

With the source and plugin code (.py files) completed they are ready to be
used in Qt designer.

To achieve that, first the location of the necessary files must be provided to
the Qt designer. This is done by adding a path to the ``$PYQTDESIGNERPATH``
system environment variable.

.. code-block:: console

    # Bash shell
    export PYQTDESIGNERPATH=/path/to/source/files:${PYQTDESIGNERPATH}
    # C-shell
    setenv PYQTDESIGNERPATH /path/to/source/files:${PYQTDESIGNERPATH}

in this case

.. code-block:: console

    # Bash shell
    export PYQTDESIGNERPATH=$VIZ_HOME/imasviz/VizPlugins/viz_example:${PYQTDESIGNERPATH}
    # C-shell
    setenv PYQTDESIGNERPATH $VIZ_HOME/imasviz/VizPlugins/viz_example:${PYQTDESIGNERPATH}


With this step completed the PyQt5 widget, now Qt Designer plugin, is ready to
be used within **Qt Designer**.

.. _exampleWidget_creating_app:

Creating a custom application/plugin with Qt Designer
-----------------------------------------------------

In this subsection, the process of creating a custom plugin/application GUI is
presented.
In **Qt Designer**, the GUI design and layout can be done conveniently with
mouse drag-and-drop, popup-menu configurations and more.

.. note::
   A good video presentation how to use Qt Designer is available in
   :ref:`plugins_qtdesigner`.


The figure below presents the final look at the end of the plugin GUI design
procedure.

.. figure:: images/QtDesigner_examplePlugin_final.png
  :align: center
  :width: 500px

  Final design of the example plugin, indented for plotting all slices of
  **flux loop** and **poloidal field** data found in **magnetics IDS**.

Before continuing, the environment must be set properly.
On the **GateWay**, a few modules are required to be loaded first and the
**LD_LIBRARY_PATH** environment variable must be extended:

.. code-block:: console

    # On the GateWay
    module load itm-python3.6
    module load itm-qt/5.8.0
    setenv LD_LIBRARY_PATH ${ITM_PYTHON_PREFIX}/lib:${LD_LIBRARY_PATH}

The Qt designer can be then run by executing the next command in the terminal:

.. code-block:: console

    # On the GateWay
    designer

A startup window will appear, as shown in the figure below.

.. Warning::
   Qt version of used PyQt5 (compiled with Qt) and Qt designer **must match**!
   Qt designer with Qt version X will not be able to find a plugin which
   source (widget code) was written using PyQt5 compiled with Qt version Y!
   The environment-setting instructions above were tested on the **GateWay**
   in the time of writing this HowTo section.

GUI design procedure
^^^^^^^^^^^^^^^^^^^^

1. First, a new **Main Window** must be created. This is done by selecting
   The **Main Window** option from the list of :guilabel:`templates/forms` and
   clicking the :guilabel:`Create` button, as shown in the figure below.

   .. figure:: images/QtDesigner_startup.png
     :align: center
     :width: 550px

     Qt designer startup window.

   After this is done, on the far left side of the window lays a **Widget Box**
   which displays a
   collection of all available widgets. On the bottom of the list, a group
   **IMASViz** containing widget labeled **exampleWidget** can be found. This is
   the custom widget which was developed through the first half of this manual
   section. The group **IMASViz** was defined in the **plugin.py** file
   (**def group**).

   .. figure:: images/QtDesigner_witdgetBox_customWidgets.png
     :align: center
     :width: 200px

     Custom widget (``exampleWidget``) in Qt designer.

2. Next, drag and drop **exampleWidget** to :guilabel:`MainWindow`. The result should be
   similar as in the figure below.

.. figure:: images/QtDesigner_examplePlugin_step_1.png
  :align: center
  :width: 500px

  **exampleWidget** within :guilabel:`MainWindow`.

3. Next, drag and drop 2x :guilabel:`Group Box` and 2x :guilabel:`vertical spacer`.

.. figure:: images/QtDesigner_examplePlugin_step_2.png
  :align: center
  :width: 500px

  Added 2x :guilabel:`Group Box` and 2x :guilabel:`vertical spacer` to :guilabel:`MainWindow`.

4. In top :guilabel:`Group Box`:

   4.1. Add 5x :guilabel:`Label`,  4x :guilabel:`LineEdit` and 1x
   :guilabel:`Push Button` widgets.

        .. figure:: images/QtDesigner_examplePlugin_groupbox_top_1.png
          :align: center
          :width: 200px

          Rough :guilabel:`Group Box` design and layout.

   4.2. Right click within the box and select :guilabel:`Layout` ->
   :guilabel:`Lay Out in a Grid`.

        .. figure:: images/QtDesigner_examplePlugin_groupbox_top_2.png
          :align: center
          :width: 500px

        .. figure:: images/QtDesigner_examplePlugin_groupbox_top_3.png
          :align: center
          :width: 200px

          Grid layout in the top :guilabel:`Group Box`.


   4.3. Set suitable texts to :guilabel:`groupbox`, :guilabel:`Label` and
   :guilabel:`Push Button` widgets. Set default values to :guilabel:`LineEdit`
   widgets.

        .. Note::
           Top :guilabel:`Label` widget contains the next text:

           **Notes: \\n - If disabled on start, the
           IDS is already available from other sources (application etc.) \\n - Below
           are the default parameters for the benchmark IDS case (GateWay)**

           **\\n** are required to achieve line breaks.

        .. figure:: images/QtDesigner_examplePlugin_groupbox_top_4.png
          :align: center
          :width: 200px

          Top :guilabel:`Group Box` with set labels and default values.

   4.4. Select :guilabel:`Group Box` and change its next properties in the
   :guilabel:`Property Editor` found on the right side of the Qt Designer
   application:

        - QGroupBox -> checkable = True (check)

        .. figure:: images/QtDesigner_examplePlugin_groupbox_top_5.png
          :align: center
          :width: 400px

          :guilabel:`Group Box` property ``checkable`` found in the
          :guilabel:`Property Editor`.

        - QWidget -> sizePolicy -> Horizontal policy = Minimum
        - QWidget -> maximumSize -> Width = 175
        - Layout -> layoutLeftMargin = 0
        - Layout -> layoutTopMargin = 0
        - Layout -> layoutRightMargin = 0
        - Layout -> layoutBottomMargin = 0
        - Layout -> layoutHorizontalSpacing = 0
        - Layout -> layoutVerticalSpacing = 0

   4.5. Select the top :guilabel:`Label` widget and change its next properties in the
   :guilabel:`Property Editor`:

        - QFrame -> frameShape = StyledPanel
        - QLabel -> wordwrap = True (checked)

        .. Note::
           Manually (with mouse cursor) resize the :guilabel:`Group Box` and
           the :guilabel:`Label` to see the whole text of the top label.

   4.6 Set next properties to all :guilabel:`LineEdit` widgets:

       - QWidget -> sizePolicy -> Horizontal policy = Minimum
       - QWidget -> sizePolicy -> Vertical policy = Fixed

       .. figure:: images/QtDesigner_examplePlugin_groupbox_top_final.png
           :align: center
           :width: 200px

           Finished top :guilabel:`Group Box`.

5. In bottom :guilabel:`Group Box`:

   5.1. Add 2x :guilabel:`Push Button` widget.

        .. figure:: images/QtDesigner_examplePlugin_groupbox_bottom_1.png
          :align: center
          :width: 200px

          Bottom :guilabel:`Group Box` template.

   5.2. Label the :guilabel:`Group Box` and :guilabel:`Push Button` widgets.

   5.3. Right click within the box and select :guilabel:`Layout` ->
   :guilabel:`Lay Out in a Grid`.

        .. figure:: images/QtDesigner_examplePlugin_groupbox_bottom_final.png
          :align: center
          :width: 200px

          Finished bottom :guilabel:`Group Box`.

6. Right click within the :guilabel:`MainWindow` and select :guilabel:`Layout` ->
:guilabel:`Lay Out in a Grid`.

   .. figure:: images/QtDesigner_examplePlugin_before_grid_layout.png
     :align: center
     :width: 400px

     :guilabel:`MainWindow` before setting grid layout.

   .. figure:: images/QtDesigner_examplePlugin_after_grid_layout.png
     :align: center
     :width: 400px

     :guilabel:`MainWindow` after setting grid layout.

7. Change :guilabel:`MainWindow` properties:

   - QWidget -> windowTitle = Magnetics IDS Overview Plugin

8. Change **exampleWidget** properties:

   - QObject -> objectName = mainPluginWidget

   .. Warning::
      This property definition is crucial in the later sections in this HowTo
      manual when linking the plugin in IMASViz.

   - QWidget -> sizePolicy -> HorizontalPolicy = Expanding
   - QWidget -> sizePolicy -> VerticalPolicy = Expanding

.. _plugins_qtdesigner_signals_slots:

Edit signals/slots
^^^^^^^^^^^^^^^^^^

By editing **signals/slots** the wanted actions such as plot execution etc. are
added to the widgets.

To edit **signals/slots**, in menubar, navigate to **Edit** ->
**Edit Signals/Slots**

.. figure:: images/QtDesigner_examplePlugin_signalSlots_1.png
  :align: center
  :width: 400px

  **Edit Signals/Slots** in menubar.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_2.png
  :align: center
  :width: 300px

  While in **Edit Signals/Slots mode**, hovering with the mouse over widgets
  marks them with slight red color.

Link **Line Edit** widgets (located in the top :guilabel:`Group Box`) signals to
**exampleWidget** slots. This is done by clicking on one of the **Line Edit**
widgets (in this case that one which holds the **Shot** value), holding and
dragging the shown arrow to **exampleWidget**, as shown in the figure below.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_3.png
  :align: center
  :width: 200px

Next, the **Configure connection editor** will be shown.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_4.png
  :align: center
  :width: 500px

  **Configure connection editor**, displaying list of available **Line Edit**
  (left) **signals** and available **exampleWidget** slots (right).

The slots are in this case actually functions/routines, which were defined
in the **exampleWidget** source code (done in section
:ref:`plugins_qtdesigner_widget_class`).

In this case, when editing the text in **Line Edit** and applying the changes
(pressing ``enter`` key etc.) the changed value must be passed to the
**exampleWidget**. This is done by selecting the suitable signal and slot from
the lists: **textEdited** and **setShot**, and pressing the
:guilabel:`OK` button.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_5.png
  :align: center
  :width: 500px

  Selection of **Line Edit** signal **textEdited** and **exampleWidget** slot
  **setShot**

After that, while in the **Edit Signals/Slots**, the start and the end point
of the red arrow will indicate which signal and slot are linked.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_6.png
  :align: center
  :width: 300px

  Red arrow indicating the **sender**, **sender signal**, **receiver** and
  **slot**.

The whole list of created slot/signal links is shown on the bottom right corner
of the Qt Designer application.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_7.png
  :align: center
  :width: 500px

  A list displaying a list of all created slot/signal lists, listing the
  **sender**, **sender signal**, **receiver** and **slot** for each link.

The same as the first link, few more links are required. Below is a detailed
table listing all necessary links.

For easier interpretation, the **sender** and
**receiver** in the table below are marked with their **text label** instead
of their **object name** (used in the Qt Designer list of signal/slots).

+---------------------------------------------------+--------------------------------------+
| SENDER                                            | RECEIVER                             |
+---------------+-------------+---------------------+-----------------+--------------------+
| Type          | Label/Value | Signal              | Type            | Signal             |
+---------------+-------------+---------------------+-----------------+--------------------+
| Line Edit     | 52344       | textEdited(QString) | exampleWidget   | setShot(QString)   |
+---------------+-------------+---------------------+-----------------+--------------------+
| Line Edit     | 0           | textEdited(QString) | exampleWidget   | setRun(QString)    |
+---------------+-------------+---------------------+-----------------+--------------------+
| Line Edit     | g2penkod    | textEdited(QString) | exampleWidget   | setUser(QString)   |
+---------------+-------------+---------------------+-----------------+--------------------+
| Line Edit     | viztest     | textEdited(QString) | exampleWidget   | setDevice(QString) |
+---------------+-------------+---------------------+-----------------+--------------------+
| Push Button   | Open IDS    | clicked()           | exampleWidget   | openIDS()          |
+---------------+-------------+---------------------+-----------------+--------------------+
| Push Button   | Plot flux   | clicked()           | exampleWidget   | openIDS()          |
|               | loop        |                     |                 |                    |
+---------------+-------------+---------------------+-----------------+--------------------+
| Push Button   | Plot        | clicked()           | exampleWidget   | openIDS()          |
|               | poloidal    |                     |                 |                    |
|               | field       |                     |                 |                    |
+---------------+-------------+---------------------+-----------------+--------------------+
| exampleWidget | Open IDS    | idsSet()            | Group Box (top) | SetChecked(bool)   |
+---------------+-------------+---------------------+-----------------+--------------------+

The final list of necessary signal/slot links in Qt Designer for this case is
shown in the figure below.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_8.png
  :align: center
  :width: 500px

At this point, the plugin is completed.

Qt Designer Preview
^^^^^^^^^^^^^^^^^^^

The constructed plugin can be tested with the Qt Designer **Preview** option,
found in the **Form** menu.

.. figure:: images/QtDesigner_examplePlugin_signalSlots_9.png
  :align: center
  :width: 400px

By pressing first the :guilabel:`Open IDS` button, waiting for a moment until
the IDS data gets read, and then pressing the :guilabel:`Plot flux loop` button,
the plot panel is populated as shown in the figure below.

.. warning::
   This specified case is done for the GateWay HPC. Make sure, that the
   corresponding IDS exists and that it contains the right data!

.. figure:: images/QtDesigner_examplePlugin_signalSlots_10.png
  :align: center
  :width: 200px

  Plotting all **Flux loop** plots from the **magnetics IDS** with parameters
  (on GateWay HPC!) **Shot:** 52344, **Run:** 0, **user**: g2penkod,
  **device**: viztest.

Saving the Qt Designer form
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The created Qt Designer GUI form (**.ui** extension) can be saved by navigating
from menubar to **File** -> **Save**. The name can be set customly, in this
case it is saved as **examplePlugin.ui** (do not confuse it with
**examplePlugin.py**).

.. warning::
   **IMPORTANT:** **the .ui file must be saved in the same
   directory as the source files**, in this case **exampleWidget.py** and
   **examplePlugin.py**.

.. _exampleWidget_running_ui:

Running the plugin .ui
^^^^^^^^^^^^^^^^^^^^^^

The **.ui** plugin can be run using Python3 shell. Open Python3 shell and type
the next commands:

.. code-block:: python

   from PyQt5.QtWidgets import QApplication
   from PyQt5 import uic
   app = QApplication([])
   uiObj = uic.loadUi('SOLPSplugin.ui')
   uiObj.show()

A script example ``run_plugin_ui_standalone.py`` is available in the plugin
directory. It is run with the following command:

.. code-block:: console

   python3 run_plugin_ui_standalone.py

Adding plugin to IMASViz
------------------------

.. warning::
   Before proceeding, make sure that you made the step 8. in
   :ref:`exampleWidget_creating_app`. Setting the **objectName** of the
   custom widget to **mainPluginWidget** is mandatory!

The main idea for integration of plugin in IMASViz is to simplify the plugin
usage and to add further functionalities to IMASViz. By running the plugin
from IMASViz the IMASViz created **IDS object** is passed to the plugin, thus
opening and setting the IDS is not necessary (required when running the plugin
as a standalone application as shown in :ref:`exampleWidget_running_ui`).

To run the plugin from IMASViz it must be first added (registered) in IMASViz
``$VIZ_HOME/imasviz/VizPlugins/VizPlugins.py`` source file. This is done through
the next few steps:

1. Add plugin to a list of registered plugins

The **RegisteredPlugins** dictionary contains major plugin properties. The
relevant properties for **examplePlugin** are highlighted in the code block
below, where:

- **example_UiPlugin** is a dictionary key (dictionary within
  **RegisteredPlugins** dictionary).

.. warning::
   Mandatory: in case the plugin is created with the help of **Qt Designer** and
   **.ui** file is created, the key must contain suffix **_UiPlugin** in order
   for IMASViz to recognize and use it correctly!

- **UiFile** dictionary key, holding full **.ui** filename.
- **dir** dictionary key, holding the path to the dictionary where the **.ui**
  filename (and other plugin sources) are located.
- **targetIDSroot** dictionary key, holding target **IDS** label.
- **targetOccurrence** dictionary key, holding target **IDS occurrence** integer.

.. code-block:: python
   :emphasize-lines: 8-13

   RegisteredPlugins = {'equilibriumcharts':'viz_equi.equilibriumcharts',
                        'SOLPS_UiPlugin': {
                            'UiFile': 'SOLPSplugin.ui',
                            'dir': os.environ['VIZ_HOME'] +
                                   '/imasviz/VizPlugins/viz_solps/',
                            'targetIDSroot' : 'edge_profiles',
                            'targetOccurrence' : 0},
                        'example_UiPlugin': {
                            'UiFile': 'examplePlugin.ui',
                            'dir': os.environ['VIZ_HOME'] +
                                   '/imasviz/VizPlugins/viz_example/',
                            'targetIDSroot': 'magnetics',
                            'targetOccurrence': 0}
                        }


2. Add plugin configuration

Each plugin can have its own specific configuration. In the case of the
**examplePlugin** there are no configurations required. Still, an empty
configuration must be provided, as highlighted in the code block below.

.. code-block:: python
   :emphasize-lines: 10

   RegisteredPluginsConfiguration = {'equilibriumcharts':[{
                                         'time_i': 31.880, \
                                         'time_e': 32.020, \
                                         'delta_t': 0.02, \
                                         'shot': 50642, \
                                         'run': 0, \
                                         'machine': 'west_equinox', \
                                         'user': 'imas_private'}],
                                     'SOLPS_UiPlugin':[{}],
                                     'example_UiPlugin':[{}]
                              }

3. Add necessary entries

The entries are mainly related to plugin identification and presentation in
IMASViz in terms of label in pop-up menus. The required entries are highlighted
in the code blocks below.

.. code-block:: python
   :emphasize-lines: 8-9

   EntriesPerSubject = {'equilibriumcharts': {'equilibrium_overview': [0],
                                              'overview': [0]},
                        'ToFuPlugin':        {'interferometer_overview': [0, 1],
                                              'bolometer_overview': [2, 3],
                                              'soft_x_rays_overview': [4, 5]},
                        'SOLPS_UiPlugin':    {'edge_profiles_overview':[0],
                                              'overview':[0]},
                        'example_UiPlugin':  {'magnetics_overview': [0],
                                              'overview': [0]}
                        }


.. code-block:: python
   :emphasize-lines: 6

   AllEntries = {'equilibriumcharts': [(0, 'Equilibrium overview...')],
                 'ToFuPlugin':        [(0, 'tofu - geom...'), (1, 'tofu - data'),
                                       (2, 'tofu - geom...'), (3, 'tofu - data'),
                                       (4, 'tofu - geom...'), (5, 'tofu - data')],
                 'SOLPS_UiPlugin':    [(0, 'SOLPS overview...')],
                 'example_UiPlugin':  [(0, 'Magnetics overview...')]
                 }

Now everything is ready to run the plugin from IMASViz.

Running the custom plugin in IMASViz
------------------------------------

When running the IMASViz, for the means of this manual, open the IDS with the
same case parameters as defined in :ref:`plugins_qtdesigner_signals_slots`.

.. figure:: images/examplePlugin_IMASViz_IDS_parameters.png
  :align: center
  :width: 500px

In the :guilabel:`tree window`, navigate to :guilabel:`magnetics`.
While holding **shift key** right click on the :guilabel:`magnetics` label and
in the popup menu select the :guilabel:`Magnetics overview...` option.

.. figure:: images/examplePlugin_IMASViz_DTV_popupmenu.png
  :align: center
  :width: 400px

On selection confirm, the **examplePlugin**, now referred to as
**Magnetics IDS Overview Plugin**, the plugin window is shown.

.. figure:: images/examplePlugin_IMASViz_window_1.png
  :align: center
  :width: 500px

  **Magnetics IDS Overview Plugin** on startup when run from within **IMASViz**.

It can be observed, that the top :guilabel:`Group Box` is disabled. This is due to our
code and signal/slots, done in the plugin development phase in previous
sections. This way if IDS object (now provided by IMASViz) is already provided
on plugin startup the IDS set/open/read procedures are not required, but if
needed are still functional and can be enabled with checking the checkbox. This
way the plugin can be conveniently used as standalone or within other
applications.

By pressing either :guilabel:`Plot flux loop` or :guilabel:`Plot poloidal field`
buttons the corresponding data, specified in the plugin code development phase,
are plotted.

.. figure:: images/examplePlugin_IMASViz_window_2.png
  :align: center
  :width: 500px

  **Magnetics IDS Overview Plugin** with plotted all **Flux loop** data from
  the **magnetics IDS** (parameters (on GateWay HPC!) **Shot:** 52344,
  **Run:** 0, **user**: g2penkod, **device**: viztest.
