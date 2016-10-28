##########
Technologies and Infrastructure
##########

One of the major challenges of developing software is the rapidly changing landscape 
of software infrastructure. Much of this infrastructure is independent of astronomy, 
and it is often better to rely on existing solutions than invent new ones. Ideally 
one would like to select building blocks that are stable and well tested, 
without being on their way to obsolescence. It is silly for astronomers to build most of 
this infrastructure themselves, but some of it is astronomy specific.

In discussing infrastructure, it is important to consider the following:

- Building on infrastructure that is produced elsewhere creates a software dependency. 
- Supporting multiple infrastructures that accomplish similar purposes 
  (such as many different file formats or GUIs) can be costly. 

In this section we discuss various items that can be categorized as infrastructure 
(because, ideally, they play no role in the numerical computations). In some cases, 
the roadmap presents choices that have already been made. In other cases, we discuss 
the current menu of choices to identify where further work needs to be done to make informed decisions.

Data Formats
------------

Currently, most astronomical images and spectra are stored and transported 
in FITS format. 
A variety of formats are used for tabular data, including ASCII files with a variety of 
conventions for metadata, FITS tables, VO tables, and databases. 

Relatively Certain
^^^^^^^^^^^^^^^^^^

For the support of JWST observers, the data analysis tools should at a minimum 
support the following formats for "non-tabular" data:

- `FITS <http://fits.gsfc.nasa.gov>`_

For the support of JWST observers, the data analysis tools should at a minimum support the following formats for tabular data:

- ASCII Tables
  - All of the following (already supported by astropy.io):
    - AASTex: AASTeX deluxetable used for AAS journals
    - Basic: basic table with customizable delimiters and header configurations
    - Cds: CDS format table (also Vizier and ApJ machine readable tables)
    - CommentedHeader: column names given in a line that begins with the comment character
    - Daophot: table from the IRAF DAOphot package
    - FixedWidth: table with fixed-width columns (see also Fixed-width Gallery)
    - Ipac: IPAC format table
    - Latex: LaTeX table with datavalue in the tabular environment
    - NoHeader: basic table with no header where columns are auto-named
    - Rdb: tab-separated values with an extra line after the column definition line
    - SExtractor: SExtractor format table
    - Tab: tab-separated values

Under Consideration
^^^^^^^^^^^^^^^^^^^

FITS has fairly serious limitations for some types of metadata. 
** Perry, add a few words here about what is under consideration as an alternative. ** 

`HDF5 <http://www.hdfgroup.org>`_ is a format capable of handling complex data. It is 
has seen limited use in astronomy, but is widely used outside of astronomy.
It is of potential interest for all kinds of data. For tabular data in particular,
the `pytables <http://www.pytables.org/moin>`_ package offers very efficient ways
of querying and doing numerical computations on large datasets.

`Relational databases. <http://net.tutsplus.com/tutorials/tools-and-tips/relational-databases-for-dummies/>`_
Python currently offers several ways to interface with
relational databases, so nothing needs to be added to enable queries and 
database manipulation. One could imagine there would be interest in developing
a simplified query interface to popular astronomical databases, but so far
we have no specific use cases for this roadmap. Ureka is currently bundling
the following python database libraries:

- MySQLdb
- PostgreSQL
- SQLite3

`JSON <http://www.json.org>`_ is a lightweight data-interchange format that is increasingly used for transporting
information from browswer queries. Libraries in python exist for reading and writing
JSON. It would be worth having the the ability in astropy to import JSON tables, although
the immediate pressure for this is low.

Data Abstraction
----------------

The concept in this roadmap is that 
most numerical calculations are done at root level by calls to numpy, 
(although when performance considerations 
arise, wrapping C code, using `numba <http://http://numba.pydata.org>`_ or 
`bottleneck <https://pypi.python.org/pypi/Bottleneck>`_ are under consideration).
Regardless of the guts of the calculation, there is usually a need to pass metadata
from one routine to the next, and it can be inefficient to read and write 
files from disk if the only purpose is to carry around metadata. Furthermore, it
is often useful to have an array of measurements accompanied by other arrays
such as uncertainties or flags, and to pass these associated arrays as one
entity between different routines. 

There is therefore the need to have a *data abstraction layer* that is distinct
from the disk file format and that is optimized for in-memory operations. 

There are several concepts for data abstraction that are likely to underpin 
the data analysis software developed in this roadmap:

- `astropy.nddata <https://astropy.readthedocs.org/en/v0.2.4/nddata/index.html>`_ provides a class and related tools to manage n-dimensional
  array-based data. 
- `astropy.table <https://astropy.readthedocs.org/en/v0.2.4/table/index.html>`_ provides facilities for working with tables that are independent
  of the disk-storage format and handle missing data, descriptions, units and
  column formatting.
- stpipe.models provides a class and tools associated with model fitting.
- `astropy.wcs <https://astropy.readthedocs.org/en/v0.2.4/wcs/index.html>`_ provides utilities for managing world-coordinate-system transformations.


Parameter Handling
------------------

The current concept is to use `configobj <https://pypi.python.org/pypi/configobj>`_ 
to handle file-oriented input of parameters. 

Scientific and Numerical Libraries
----------------------------------

Python is attractive as the main language for the data analysis tools in part
because it already has a rich library of numerical, scientific and statistical 
tools. 

Relatively Certain
^^^^^^^^^^^^^^^^^^

The current plan is to build around the following libraries:

- `numpy <http://www.numpy.org>`_ N-dimensional array processing.
- `scipy <http://www.scipy.org>`_ Scientific computing library.
- `astropy <http://www.astropy.org>`_ Astronomical utilities (The software in this roadmap will have strong dependencies on astropy,
  building on its core library. The vision is that most or all of the tools discribed here will become part of astropy itself.)

Under Consideration
^^^^^^^^^^^^^^^^^^^

There continues to be rapid evolution outside of astronomy in the numerical computing
for python. Building around these libraries could reduce the coding effort needed for
developing the analysis tools and could improve their performance. But each library creates
a dependency, so we will have to choose carefully and probably not let all flowers bloom 
in this area. The following are under consideration:

- `scikits <http://scikits.appspot.com>`_ These are add-on packages for scipy that are in various states of development. 
  Two of these are already well developed and of particular interest:  `scikit-image <http://scikit-image.org>`_, focusing 
  primarily on image filtering, segmentation, and morphology, 
  and `scikit-learn <http://scikit-learn.org/stable/>`_, focusing on machine learning.
- `cython <http://www.cython.org>`_ Compiler for for adding C extensions within python code.
- `numba <http://numba.pydata.org>`_ Offers significant peformance enhancements to numpy using a just-in-time specializing 
  compiler to `LLVM <llvm.org>`_, which is itself a compiler).
- `bottleneck <https://pypi.python.org/pypi/Bottleneck>`_ a collection of fast numpy array functions written in Cython.
- `pytables <http://www.pytables.org/moin>`_ a package for working with hdf5 files and efficiently manipulating 
  extremely large amounts of data. 
- `pandas <http://pandas.pydata.org>`_ Focuses primarily on statistical analysis of tabular data, with similar basic
  functionality to the R statistical language (but without the huge package of contributed libraries).
- `fftw <http://www.fftw.org>`_ Fast Fourier Transforms (faster than other options currently available in numpy or scipy).

Ureka is currently buldling the following additional libraries, not mentioned above, in its distribution. 
(Does any code in Ureka depend on these, other than scipy??):

- `BLAS <http://www.netlib.org/blas/>`_ Basic linear algebra. Bundled with the standard scipy distribution.
- `LAPACK <http://www.netlib.org/lapack/>`_ Linear algebra package. Bundled with the standard scipy distribution.
- `GSL <http://www.gnu.org/software/gsl/>`_ The GNU Scientific Library. 
- `SymPy <http://sympy.org/en/index.html>`_ Symbolic mathematics library (Computer Algebra System). 


Physical Units and Constants
----------------------------

The current concept is to continue to develop `astropy.units <https://astropy.readthedocs.org/en/v0.2.4/units/index.html>`_ to 
handle physical quantities, and to continue to develop 
`astropy.time <https://astropy.readthedocs.org/en/v0.2.4/time/index.html>`_ to handle time.


Interprocess Communications
---------------------------

The inter-process communication IPC infrastructure handles data transport between simultaneously-running, 
independent processes. Examples include passing cursor positions from a GUI to a task
that might perform some analysis of the pixels near the cursor, or passing positions
of sources to a routine that might mark them on an image display. 

Relatively Certain
^^^^^^^^^^^^^^^^^^

The following IPC protocols are likely to be supported:

- `SAMP <http://www.ivoa.net/documents/SAMP/>`_ The Virtual Observatory messaging protocol.
- `OMQ <http://zeromq.org>`_ The protocol used by ipython.


Under Consideration
^^^^^^^^^^^^^^^^^^^

The following are under consideration:

- `XPA <http://hea-www.harvard.edu/RD/xpa/env.html>`_ The protocol used by the SAO ds9 image display. Since
  ds9 now can communicate using SAMP, it is unclear if XPA support is needed.
- `REST <http://rest.elkstein.org>`_ A standard protocol for client-server communications, popular in web services.
- `AMPQ <http://www.amqp.org>`_ another popular message-passing protocol. Quoting from the 
  `zeromq website <http://zeromq.org/docs:welcome-from-amqp>`_
  AMQP and 0MQ have quite different goals. AMQP aims to commoditize existing enterprise messaging patterns, 
  while 0MQ aims to create messaging patterns that can succeed at Internet scale...0MQ acts more like
  low-latency products.


Multiprocessing
---------------

Multiprocessing involves making use of either multiple cores on a single computer, or multiple
computers on a network. For typical JWST tasks, it is likely that the work is 
`embarassingly parallel <http://en.wikipedia.org/wiki/Embarrassingly_parallel>`_ in the sense
that the tasks can be accomplished little or no communication between tasks running
in parallel. There are numerous ways of dealing with such problems that do not
require any astronomy-specific software development. Examples include:

- `Condor <http://research.cs.wisc.edu/htcondor/>`_ a queuing system for distributed computing;
- `Ipython parallel computing <http://ipython.org/ipython-doc/dev/parallel/>`_, which supports a 
  variety of different parallel and distributed cmoputing models;
- The python `multiprocessing module <http://docs.python.org/2/library/multiprocessing.html>`_ which can create
  pools of subprocesses that can inter-communicate. 

Thread-based parallelism is not always efficient in python due to the 
`Global Interpreter Lock <https://wiki.python.org/moin/GlobalInterpreterLock>`_. Many
of the subtleties are addressed in `this article by Jesse Noller <http://jessenoller.com/blog/2009/02/01/python-threads-and-the-global-interpreter-lock>`_.

For the purposes of this roadmap, 
the brief summary is that the data-analysis software in this roadmap will 
be amenable to multiprocessing using standard tools. In the initial implementation,
it is unlikely that the tools themselves will use multiprocessing internally. 
However, multiprocessing may be one of the ways to address performance issues if re-coding in a 
faster language (e.g. C) doesn't solve the problem.

Special-Purpose Hardware
------------------------

Examples of special-purpose hardware include Graphics Processing Units (GPUs),
and Accelerated Processing Units (APUs). It is currently not envisioned 
that any the software in this roadmap will require the use of such hardware.
It is conceivable that routines could be developed where the use of a GPU
is an option if it is available, but that is not a near-term priority.

GUI Frameworks
--------------

The aim is to separate the computational layer from the GUI layer so that different
user interfaces can be used to access the same functionality. There will nevertheless
be default graphical user interfaces for inputting parameters or otherwise
controlling task execution, and interacting with graphics and images. Standard
libraries will be used for developing the interfaces and there will be a 
*gui abstraction layer* to allow different GUIs to interact with the software 
tools with a simple change of user preferences.

Relatively Certain
^^^^^^^^^^^^^^^^^^

While not really a GUI per se, the `ipython notebook <http://ipython.org/notebook.html>` provides
a full-featured environment for interactive scripting, and it will be a goal to make the
data analysis tools work well in this environment and to use it for many of the 
tutorials.

Under Consideration
^^^^^^^^^^^^^^^^^^^

The GUI frameworks under consideration are:

- `Qt <http://qt-project.org>`_ A popular open-source cross-platform framework with python bindings.
- `glue <http://www.glueviz.org>`_ A python library for exploring relationships within and among related data sets.


Software Distribution
---------------------

Under Consideration
^^^^^^^^^^^^^^^^^^^

STScI has recently developed `Ureka <http://ssb.stsci.edu/ureka/>`_ as a unified distribution 
mechanism for STScI- and Gemini-developed python tools, as well as IRAF, pyraf, STSDAS, 
and a variety of supporting libraries. This is still in beta testing as of September 2013.

There are two other distribution mechanisms worthy of consideration:

- `Anaconda <https://store.continuum.io/cshop/anaconda/>`_ the distribution mechanism 
  used by `Continuum Analytics <http://www.continuum.io>`_. This distribution already 
  includes many of the open-source scientific packages and libraries under consideration as 
  dependencies for the data-analysis tools.
- `Canopy <https://www.enthought.com/products/canopy/>`_ the distribution mechanism used
  by `Enthought <https://www.enthought.com>`_. This distribution already includes
  many of the same libraries as Anaconda, and also includes astropy. 


Documentation
-------------

Relatively Certain
^^^^^^^^^^^^^^^^^^

The current plan is to use `sphinx <http://sphinx-doc.org>`_ for documentation. 
This is already in use for astropy and STScI software and is widely used in 
the python community, as well as for other languages. It produces web pages
as well as pdf, ebooks, and other formats and is able to import the same
docstrings used to drive the `help` for the command-line interface to the
data-analysis tools or for mouseovers in ipython notebooks. This roadmap 
has been written using sphinx. 

It is likely that the documentation will use locally-developed and 
open-source extensions and styles for sphinx, such as
`numpydoc <https://pypi.python.org/pypi/numpydoc>`_ and
`sphinxcontrib-programoutput <http://pythonhosted.org/sphinxcontrib-programoutput/>`_.

Under Consideration
^^^^^^^^^^^^^^^^^^^

The Ureka distribution currently includes the following in addition to sphinx and numpydoc,
possibly for support of code that was written before sphinx appeared on the scene:

- `Epydoc <http://epydoc.sourceforge.net>`_.
- `Pygments <http://pygments.org>`_.
- `Docutils <http://docutils.sourceforge.net>`_.
- `Jinja2 <http://jinja.pocoo.org/docs/>`_.

Testing
-------

Relatively Certain
^^^^^^^^^^^^^^^^^^
What's the plan? Nose?

Under Consideration
^^^^^^^^^^^^^^^^^^^


The Ureka distribution currently includes the following:

- Pandokia 
- py 
- PyTest 
- unittest2 
- shUnit2
- `nose <http://nose.readthedocs.org>`_.

Graphics and Image Displays
---------------------------

Relatively Certain
^^^^^^^^^^^^^^^^^^

For 2-D line graphics, the plan is to use `matplotlib <http://matplotlib.org>`_, a 
full-featured library that produces publication-quality graphics as well as providing
the hooks for developing interactive GUIs or widgets and for animations. It also
some 3D graphics and image display.  It is probably not the platform for
interactive image display as a replacement for ds9. 

Independent of whether any new image-display tools are developed, 
`ds9 <http://hea-www.harvard.edu/RD/ds9/site/Home.html>`_ will be supported.
In particular, it will be possible to load and manipulate images from memory and 
interact with cursors and regions to perform data analysis.

Under Consideration
^^^^^^^^^^^^^^^^^^^

The following are under consideration:

- `glue <http://www.glueviz.org>`_ A python library for exploring relationships within and among related data sets.
- `Mayavi <http://mayavi.sourceforge.net>`_ A python library for 3D visualization built on the `Visualization Toolkit (VTK) <http://www.vtk.org>`_.
- `ginga <https://pypi.python.org/pypi/ginga>`_ A FITS image viewer under development at the Subaru observatory.
- `APLpy <http://aplpy.github.io>`_, an astropy-affiliated package, is layered on top of matplotlib 
  and provides routines for making full-color images and making various kinds of astronomical 
  graphic overlays on images (e.g.  coordinage grids and beam sizes). 


The following are currently bundled with Ureka:
- `graphviz <http://www.graphviz.org>`_ visualization software for network diagrams, flowcharts and other structural information.
- `PIL <http://effbot.org/imagingbook/pil-index.htm>`_ The Python Imaging Library, mostly useful for supporting standard non-FITS formats.

