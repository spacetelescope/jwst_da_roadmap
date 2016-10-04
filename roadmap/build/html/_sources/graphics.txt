##########################
Graphics and Visualization
##########################

2D image display
----------------

Probably the most popular 2D image display tool in use by the US community
is `ds9 <http://hea-www.harvard.edu/RD/ds9/site/Home.html>`_, from SAO, which
is a stand-alone application actively maintained at SAO. It has the
ability to communicate with other tasks using 
`XPA <http://hea-www.harvard.edu/RD/xpa/index.html>`_ or 
`SAMP <http://www.ivoa.net/documents/SAMP/>`_. Other sophisticated image display 
utilities for astronomical images include:

*  `Aladin <http://aladin.u-strasbg.fr>`_
* `ximtool <http://iraf.noao.edu/iraf/web/projects/x11iraf/ximtool.ps>`_
* `GAIA::SkyCat <http://star-www.dur.ac.uk/~pdraper/gaia/gaia.html>`_

The default will be to support image tools that communicate using SAMP. SAMP
is a message-passing protocol, but doesn't specify the functions that should
be supported by the tools on either end or how to invoke those functions.
Ds9, for example, has 1150 different ``xpaget`` and ``xpaset`` commands, supporting
96 different areas of functionality. It would take a considerable effort
to implement all of this functionality afresh in a new tool. Nevertheless
there are some limitations of ds9 that make it worth considering. In particular,
it is not particularly fast at loading images or zooming and panning, and
is not designed to deal with very large images that do not fit entirely in memory.
Both are in contrast to many tools available on the web that pan and zoom
quite fast in extremely large images. 

Because there are good image viewers available, building a new 2D image
viewer is not currently a high priority for STScI. However, it would be very useful
to maintain a wish-list of features to include in any such tool (in addition
to supporting all of ds9's features).

3D image display
----------------

Observers using the MIRI or NIRSpec spectrographs on JWST will need a 
full-featured 3D image display tool. This must interact with analysis tools
and 2D graphics tools and provide a variety of options for data selection
and visualization. Under consideration are:

* ds9
* `glue <http://www.glueviz.org/en/latest/>`_
* join forces with radio astronomers to provide a tool to support both communities.


Interactive 2D & 3D graphics
----------------------------

2D graphics
^^^^^^^^^^^

Within python, the standard is `matplotlib <http://matplotlib.org>`_, which has
had heavy STScI involvement (and some funding) in its development. It supports
multiple "backends" to build in device and GUI independence. Efforts are underway
to make it possible to plot interactively in a web browser. Most users 
interact with matplotlib via `pyplot <http://matplotlib.org/api/pyplot_api.html>`_,
which provides a MATLAB-like plotting framework. 

Relatively certain
++++++++++++++++++

For 2D graphics, the plan is to continue to improve matplotlib and use 
it as the basis for both interactive and publication-quality graphics.

Under consideration
+++++++++++++++++++

Matplotlib's greatest weakness is probably speed, which makes it not particularly 
suitable to video frame-rate animations or real-time updates. While most
astronomers don't need these capabilities for day-to-day data analysis, 
they are occasionally needed. For speed, it is useful to consider hardware-accelarated
graphics, such as that provided by `NodeBox for OpenGL <http://www.cityinabottle.org/nodebox/>`_.
Another potential weakness of Matplotlib is that it is was not originally 
engineered to operate in a web browser. The `Bokeh <http://www.continuum.io/developer-resources>`_
visualization library, under development at Continuum Analytics, aims to implement
the `Grammar of Graphics <http://www.cs.uic.edu/~wilkinson/TheGrammarOfGraphics/GOG.html>`_, which
has become popular in statistical packages such as `R <http://www.r-project.org>`_.

There is also some interest in having the capability of making dynamic
graphs, such as those provided by the `d3 javascript library <http://d3js.org>`_.


3D graphics
^^^^^^^^^^^

Relatively certain
++++++++++++++++++

Matplotlib provides some 3D capabilities within the 
`mplot3d <http://matplotlib.org/mpl_toolkits/mplot3d/tutorial.html>`_ package.

Under consideration
+++++++++++++++++++

There are other options for 3D visualization as well, including:

* Mayavi
* `VPython <http://www.vpython.org>`_ (licensing issues?).
* `PyQtgraph <http://www.pyqtgraph.org>`_ 2D and 3D visualization.
* `Mayavi <http://docs.enthought.com/mayavi/mayavi/>`_ 3D visualization.
* `VTK <http://vtk.org>`_ 3D computer graphics with python wrappers.
* `Paraview <http://http://www.paraview.org>`_ An open-source python-scriptable 3D visualization application aimed at very large datasets.
* `yt <http://yt-project.org>`_ A volumetric data-analysis program for astrophysical simulations.

Publication-quality graphics
----------------------------

The plan is to continue to improve matplotlib and use it as the basis for
publication-quality graphics.

Easy-to-construct widgets
-------------------------

Easy-to-construct web graphics
------------------------------


