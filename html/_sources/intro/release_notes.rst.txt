.. _IMASViz_release_notes:

=============
Release notes
=============

-----------
Version 2.0
-----------

Released:

/

Changes:

- **Full GUI migration from wxPython and wxmPlot to PyQt and pyqtgraph Python**
  **libraries**
- Basic plot feature performance improved greatly.
  Quick comparison for plotting 17 plots to a single panel using default
  plotting options:

   - wxPython IMASViz: ~13s
   - PyQt5 IMASViz:  less than 1s (more than **13x speed improvement**!)

- Superior plot export possibilities
- GUI improvements
- Database tree browser interface display improvements
- Added first 'node contents display' feature (displayed in the
  :guilabel:`Node Documentation` Widget)
- Reduced the number of separate windows, introduce docked widgets
- Introduce first GUI icons
- MultiPlot feature relabeled to TablePlotView
- SubPlot feature relabeled to StackedPlotView
- Add support for IMAS versions 3.20.0, 3.21.0 and 3.21.1
- Included **documentation + manual** (~60 pages in PDF) in a form of
  reStructuredText source files for document generation (single source can be
  generated into multiple formats e.g. PDF, HMTL...)
- In-code documentation greatly improved and extended
- and more...

Short summary of files and line changes count (ignoring generated files and
scripts):

- 193 commits,
- 268 files changed,
- 13316 insertions(+),
- 10162 deletions(-)

.. git log $from_commit..$to_commit --pretty=oneline | wc -l
.. git diff --stat $from_commit $to_commit -- . ':!*enerated*' ':!*.xml'

.. from_commit = d25c4b8bddf
.. to_commit = d9253fedf12d63761299a61c6930bc77f0d9b90c

.. Note::
   The migration to PyQt5 due to IMASViz containing a large code sets is not
   yet fully complete.
   List of known features yet to migrate to IMASViz 2.0:
   ``Equilibrium plugin``,
   ``Add selected nodes to existing TablePlotView``, and
   ``StackedPlotView manager``.

A quick GUI comparison between the **previous** and the **new** IMASViz GUI is
shown below.

Overview of IMASViz 1.2 GUI:

.. image:: images/GUI_overview_old.png
   :align: center
   :width: 550px

Overview of IMASViz 2.0 GUI:

.. image:: images/GUI_overview_2.0.png
   :align: center
   :width: 550px

-----------
Version 1.2
-----------

Released:

24.8.2018

Changes:

- New functionality: selection command of nodes belonging to same parent AOS
  (Array of Structures)
- MultiPlot and SubPlot design improvements
- Adding support for IMAS versions 3.19.1

-----------
Version 1.1
-----------

Released:

8.6.2018

Changes (since March 2017):

- Bugs fixes & performance improvement
- Code migration to Python3
- GUI improvements
- UDA support for visualizing remote shots data
- Reuse of plots layout (multiplots customization can be saved as a script file
  to be applied for any shot)
- A first plugins mechanism has been developed which allows developers to
  integrate their plugins to IMASViz
- The 'Equilibrium overview plugin' developed by Morales Jorge has been
  integrated into IMASViz
- Concerning UDA, WEST shots can be accessed if a SSH tunnel can be established
  to the remote WEST UDA server.
- Introducing MultiPlot and SubPlot features
- Add support for IMAS version 3.18.0


.. - From our first tests, SSH tunnel cannot be established from the Gateway. The issue will be analyzed during this Code Camp.