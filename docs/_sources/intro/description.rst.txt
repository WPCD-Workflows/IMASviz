.. _description:

===========
Description
===========

**IMASViz** is a post-processing tool for visualization and data analysis of
the static and dynamic data, stored within
**Integrated Modelling Analysis Suite** (**IMAS**) database pulse files e.g.
**Interface Data Structures** (**IDSs**). To perform those features it provides
a convenient Graphical User Interface (GUI) that allows easier and more
straightforward handling of the available signals. The main IMASViz features
are done through plotting the data, representing it in a form of plots/graphs.
While the tool itself is already available for use it is still under active
development, and various features, GUI improvements etc. are still being
implemented.

For additional support or if any issues are found with IMASViz please contact
the developers via e-mail or submit a ticket on
`JIRA ITER Webpage <https://jira.iter.org>`_.

While submitting the ticket please use the options listed below

+--------------+-------------------------+
| **Field**    | **Required Option**     |
+==============+=========================+
| Project      | IMAS (IMAS)             |
+--------------+-------------------------+
| Components   | VIZ                     |
+--------------+-------------------------+

Developers:

  - **Ludovic Fleury** (CEA Cadarache, Research Institute for Magnetics Fusion,
    e-mail: ludovic.fleury@cea.fr)
  - **Dejan Penko** (University of Ljubljana, Mech.Eng., LECAD Lab,
    e-mail: dejan.penko@lecad.fs.uni-lj.si)

.. Plugin developers

..   - ‘Equilibrium plugin’: Jorge Morales (CEA Cadarache, Research Institute for Magnetics Fusion)
..   - ‘Tofu plugin’: Didier Vezinet (CEA Cadarache, Research Institute for Magnetics Fusion)

The tool uses the following Python packages:

**1. PyQt5**

    PyQt5 is a comprehensive set of Python bindings for Qt v5. It is
    implemented as more than 35 extension modules and enables Python
    to be used as an alternative application development language to C++
    on all supported platforms including iOS and Android.

    Qt is set of cross-platform C++ libraries that implement high-level APIs for
    accessing many aspects of modern desktop and mobile systems.

    For more on **PyQt5** see
    `PyQt5 webpage <https://pypi.org/project/PyQt5/>`_.
    For more on **Qt** see `Qt webpage <https://www.qt.io/>`_.

**2. pyqtgraph**

    Pyqtgraph is a graphics and user interface library for Python that provides
    functionality commonly required in engineering and science applications. Its
    primary goals are a) to provide fast, interactive graphics for displaying
    data (plots, video, etc.) and b) to provide tools to aid in rapid application
    development (for example, property trees such as used in Qt Designer).

    PyQtgraph makes heavy use of the Qt GUI platform (via PyQt or PySide) for its
    high-performance graphics and numpy for heavy number crunching. In particular,
    pyqtgraph uses Qt’s GraphicsView framework which is a highly capable graphics
    system on its own and brings optimized and simplified primitives to this
    framework to allow data visualization with minimal effort.

    For more on **pyqtgraph** see
    `pyqtgraph webpage <http://www.pyqtgraph.org/>`_.

**3. matplotlib**

    Matplotlib is a Python 2D plotting library which produces publication quality
    figures in a variety of hardcopy formats and interactive environments across
    platforms.

    For more on **matplotlib** see
    `matplotlib webpage <https://matplotlib.org/>`_.

**4. sphinx**

    Sphinx is a tool originally created as a Python documentation generator,
    but it allows also generating documentation in form of html, latex etc.

    For more on **Sphinx** see
    `Sphinx webpage <http://www.sphinx-doc.org/en/master/>`_.

.. TODO texlive, latexmk, libfreetype
.. sudo apt-get install latexmk texlive texlive-science texlive-formats-extra

IMASViz tool is available on **ITER git repository** (access permission is
required) under project **Visualization/VIZ**, branch **develop**.

Direct link to the **IMASViz** git.iter repository:
`IMASViz <https://git.iter.org/projects/VIS/repos/viz/browse>`_.
