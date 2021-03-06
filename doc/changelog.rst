.. _changelog:

Changelog
=========


.. _changelog-0.6:

Version 0.6
-----------

**Release date**: yyyy-mm-dd

**doi**:

**Breaking changes**

* Remove functions ``extract``, ``vfilter``, and ``vremove`` from
  ``fatiando.mesher``. They were only used in certain runs of
  ``fatiando.gravmag.harvester`` and don't fit the overall design of the
  ``mesher`` package. They will be replaced by better designed methods in the
  ``PrismMesh`` class.
  (`PR 383 <https://github.com/fatiando/fatiando/pull/383>`__)
* The ``seismic_image`` and ``seismic_wiggle`` functions from ``fatiando.vis``
  don't specify default values for the ``dt`` argument anymore. This parameter
  is specific to the dataset and shouldn't have a default value.
  (`PR 366 <https://github.com/fatiando/fatiando/pull/366>`__)
* Refactor ``fatiando.datasets``. The ``load_surfer`` function now returns a
  dictionary with the data and takes different arguments. The functions for
  loading CRUST2.0 were removed.
  (`PR 355 <https://github.com/fatiando/fatiando/pull/355>`__)
* Removed unused functions from ``fatiando.utils``: ``normal``.  ``vecnorm``.
  ``vecmean``.  ``vecstd``.  ``sec2hms``.  ``sec2year``. and ``year2sec``.
  (`PR 361 <https://github.com/fatiando/fatiando/pull/361>`__)
* Remove function ``fromimage`` from ``fatiando.utils`` and method ``img2prop``
  from ``fatiando.mesher.SquareMesh``. They were replaced by
  ``fatiando.datasets.from_image``.
  (`PR 363 <https://github.com/fatiando/fatiando/pull/363>`__)

**Bug fixes**

* Fixes bug in ``fatiando.gravmag.imaging``, replacing
  ``transform._getfreqs(x, y, data, shape)`` with
  ``transform._fftfreqs(x, y, shape, shape)`` because that function was renamed.
  (`PR 347 <https://github.com/fatiando/fatiando/pull/347>`__)

**New features and improvements**

* Center the colorbar on zero for ``seismic_image`` plots. Makes the plots look
  nicer when using divergent colormaps like ``'RdBu'``.
  (`PR 366 <https://github.com/fatiando/fatiando/pull/366>`__)
* Replace Cython coded functions in ``fatiando.gravmag.sphere`` with pure
  Python + numpy code. Optimized the numpy code to get ~4x speedup in the
  gravity function and ~2x in the magnetic functions over the Cython code.
  As a bonus, reached 100% test coverage for this module.
  (`PR 364 <https://github.com/fatiando/fatiando/pull/364>`__)
* Replace Cython coded functions in ``fatiando.gravmag.polyprism`` with pure
  Python + numpy code. Functions run as fast or faster than their Cython
  versions thanks to some optimizations (combine logarithm computations).
  (`PR 368 <https://github.com/fatiando/fatiando/pull/368>`__)
* New function ``from_image`` in ``fatiando.datasets`` that creates a template
  for a model from an image file.
  (`PR 363 <https://github.com/fatiando/fatiando/pull/363>`__)
* Added class ``RickerWavelet`` to ``fatiando.seismic`` to generate and sample
  the wavelet. Lays the ground work to expand later with more wavelets.
  (`PR 362 <https://github.com/fatiando/fatiando/pull/362>`__)
* Added functions ``lame_lambda`` and ``lame_mu`` to ``fatiando.seismic`` that
  calculate the Lamé parameters :math:`\lambda` and :math:`\mu` from the P and
  S velocities and density.
  (`PR 359 <https://github.com/fatiando/fatiando/pull/359>`__)
* Added option ``restrict`` to function ``harvest()`` in
  ``fatiando.gravmag.harvester``. The option takes a string list with
  directions in which the harvester will be restricted in seed growth.
  (`PR 314 <https://github.com/fatiando/fatiando/pull/314>`__)
* Added option ``below`` to ``carvetopo()`` function of
  ``fatiando.mesher.PrismMesh``. If set to ``True``, prisms below the input
  surface will be masked.
  (`PR 313 <https://github.com/fatiando/fatiando/pull/313>`__)
* Added two new functions to ``fatiando.datasets``. ``fetch_hawaii_gravity``
  loads raw gravity and topography data for Hawaii. The dataset is packaged
  with Fatiando and users don't have to download it separately. ``check_hash``
  verifies that the hash of a file is same as a known (recorded) hash. Used to
  check data files for corruption.
  (`PR 355 <https://github.com/fatiando/fatiando/pull/355>`__)
* More rigorous regression tests for the ``fatiando.gravmag`` forward modeling
  functions. Tests compare the current output against a saved output that if
  known to be correct. This helps ensure that the values don't change suddenly
  but in a consistent way that we wouldn't catch otherwise (like by a constant
  value).
  (PRs `364 <https://github.com/fatiando/fatiando/pull/364>`__,
  `395 <https://github.com/fatiando/fatiando/pull/395>`__,
  `396 <https://github.com/fatiando/fatiando/pull/396>`__)

**Development/maintenance**

* Turn ``fatiando.mesher`` module into a package (as was done for ``gridder``).
  The tests were moved into ``fatiando/mesher/tests``. Nothing changes from the
  user perspective.
  (`PR 383 <https://github.com/fatiando/fatiando/pull/383>`__)
* Better internal organization of the ``fatiando.gridder`` module. Users won't
  have to modify theirs existing code. These changes only impact the developer
  side. Mainly, the module is now a package with functions divided into small
  modules, making it easier to read and modify the code. Also implemented many
  new tests, reaching 100% coverage, and several new gallery plots.
  (`PR 297 <https://github.com/fatiando/fatiando/pull/297>`__)
* Reorganized ``fatiando.datasets`` as a package and reached 100% test
  coverage. This change doesn't impact users directly.
  (`PR 355 <https://github.com/fatiando/fatiando/pull/355>`__)


.. _changelog-0.5:

Version 0.5
-----------

**Release date**: 2016-11-27

**doi**: `10.5281/zenodo.157746 <https://doi.org/10.5281/zenodo.157746>`__

**Breaking changes**

* Moved function ``fatiando.utils.circular_points`` to
  ``fatiando.gridder.circular_scatter`` module because ``gridder`` is where
  point generation functions live. The function now returns the x, y coordinate
  arrays instead of a list of points. You can transform x, y to points by
  ``points = numpy.transpose([x, y])``. Deleted the function
  ``utils.connect_points`` because it was only used in a single place and could
  be substituted by two list comprehensions. Deleted function
  ``utils.random_points`` because it has the exact same functionality as
  ``gridder.scatter``.
  (`PR 317  <https://github.com/fatiando/fatiando/pull/317>`__)
* Remove unused module ``fatiando.gravmag.half_sph_shell``. It was used to test
  ``fatiando.gravmag.tesseroid`` but has been replaced by a full spherical
  shell solution that is coded in the tests. It serves no purpose so it should
  be removed to avoid having to maintain it.
  (`PR 288 <https://github.com/fatiando/fatiando/pull/288>`__)
* Rename the Euler deconvolution classes to ``EulerDeconv`` (old ``Classic``
  class, a terrible name choice in retrospect), ``EulerDeconvMW`` for the
  moving window solver, and ``EulerDeconvEW`` for the expanding window solver.
  These names are more unique and will not clash with any other class. This is
  crucial to establish a nice API for ``fatiando.gravmag``.
  (`PR 286 <https://github.com/fatiando/fatiando/pull/286>`__)

**Bug fixes**

* Fixes bug in ``fatiando.gravmag.tesseroid`` when running with the latest
  numba (0.28). The ``looplift`` argument to ``numba.jit`` doesn't seem to work
  anymore. The workaround was to move array allocations out of the jit compiled
  functions.
  (`PR 328 <https://github.com/fatiando/fatiando/pull/328>`__)

**New features and improvements**

* Change the behavior of ``gravmag.transform.upcontinue``. Instead of raising
  an error when 'height' <= 0 (downward continuation) it now warns users that
  in this case the computation is unstable.
  (`PR 337 <https://github.com/fatiando/fatiando/pull/337>`__)
* Add functions ``power_density_spectra`` and ``radial_average_spectrum`` to
  ``fatiando.gravmag.transform`` to calculate the radial average power density
  spectrum of gridded potential field data.
  (`PR 303 <https://github.com/fatiando/fatiando/pull/303>`__)
* Add copy method to ``fatiando.mesher`` objects.
  (`PR 301  <https://github.com/fatiando/fatiando/pull/301>`__)
* Enable ``fatiando.mesher.PointGrid`` to have points at different depths by
  passing it an array as the ``z`` argument.
  (`PR 283 <https://github.com/fatiando/fatiando/pull/283>`__)
* Started an example gallery (`matplotlib style
  <http://matplotlib.org/gallery.html>`__) using the Sphinx plug-in
  `sphinx-gallery <http://sphinx-gallery.readthedocs.io/>`__.
  (`PR 282 <https://github.com/fatiando/fatiando/pull/282>`__)
* Added several functions for padding arrays of arbitrary dimension.
  ``fatiando.gridder.pad_array`` pads an array with a variety of padding and
  taper options.  ``fatiando.gridder.unpad_array`` returns the original,
  unpadded array.  ``fatiando.gridder.pad_coords`` pads the coordinate vectors
  associated with the arrays padded above. Added Kass in the contributors.
  (`PR 239 <https://github.com/fatiando/fatiando/pull/239>`__)
* Added function for tilt derivative filter for gravmag data.
  ``fatiando.gravmag.transform.tilt`` returns a value between -90 and 90
  degrees, with the 0 value being located over or nearly over the edge of a
  given anomaly.
  (`PR 261 <https://github.com/fatiando/fatiando/pull/261>`__)

**Deprecation**

* Warn users when importing ``fatiando.vis.myv`` that this module will be
  removed in version 0.7. In version 0.6, we'll add 3D plotting functionality
  with matplotlib's ``mpl3d`` or another suitable replacement. Users will be
  encouraged to switch to the new replacement. The ``fatiando.vis.myv`` might
  be kept as an optional module.
  (`PR 336 <https://github.com/fatiando/fatiando/pull/336>`__)
* Warn users when importing ``fatiando.vis.mpl`` that this module will be
  removed in version 0.6. Using this module as a replacement for
  ``matplotlib.pyplot`` is **strongly discouraged**. The custom plotting
  functions, like ``seismic_wiggle``, will be kept and moved to a new module.
  (`PR 335 <https://github.com/fatiando/fatiando/pull/335>`__)

**Development/maintenance**

* Warn users that the code in ``fatiando.seismic.wavefd`` is experimental and
  may not provide accurate results.
  (`PR 319  <https://github.com/fatiando/fatiando/pull/319>`__)
* Implement unit tests for the ``fatiando.seismic.srtomo`` module. Reached 100%
  test coverage. Now examples are only in the cookbook.
  (`PR 316  <https://github.com/fatiando/fatiando/pull/316>`__)
* Move from ``distutils`` to ``setuptools`` in ``setup.py``, as recommended in
  the `Python Packaging User Guide <https://packaging.python.org/>`__.
  (`PR 294 <https://github.com/fatiando/fatiando/pull/294>`__)
* Replace `nose <http://nose.readthedocs.io/>`__ with `py.test
  <http://pytest.org/>`__ as our unit testing framework. Tests are now located
  in the package ``fatiando.tests`` and installed with Fatiando. This means
  that we can test an installed version of Fatiando, not just the code in the
  repository.
  (`PR 290 <https://github.com/fatiando/fatiando/pull/290>`__)
* Added back-end support for decorators from `duecredit
  <https://github.com/duecredit/duecredit/>`__ to be added to methods. This
  allows a report for per-method citations based on the methods used in a given
  script. Currently only implemented for `gravmag/magdir` but will be added to
  all methods in time.
  (`PR 293 <https://github.com/fatiando/fatiando/pull/293>`__)
* Better navigation for long pages in the docs by adding a sidebar with links
  to subsections.
  (`PR 275 <https://github.com/fatiando/fatiando/pull/275>`__)


.. _changelog-0.4:

Version 0.4
-----------

**Release date**: 2016-04-05

**doi**: `10.5281/zenodo.49087 <https://doi.org/10.5281/zenodo.49087>`__

**Changes**:

* **New** obtain a synthetic convolutional seismogram in
  ``fatiando.seismic.conv``. It can be given a depth model that will be
  converted to a time model before generating the synthetic seismogram.
  (`PR 190 <https://github.com/fatiando/fatiando/pull/190>`__)
* **Refactor** ``fatiando.inversion``. Completely redesigned classes make
  implementing new inversions simpler. Subclassing ``Misfit`` is simpler, with
  fewer parameters necessary. The usage of existing inversions has changed
  little. A **new dependency** ``future`` was added to ease the transition to
  support Python 3.
  (`PR 127 <https://github.com/fatiando/fatiando/pull/127>`__)
* Fix the broken software carpentry links in ``develop.rst``.
  (`PR 245 <https://github.com/fatiando/fatiando/pull/245>`__)
* Fix the doctest for ``fatiando.gravmag.tensor.center_of_mass``.
  (`PR 242 <https://github.com/fatiando/fatiando/pull/242>`__)
* **BUG FIX**: Tesseroid computations failed (silently) when tesseroids were
  smaller than 1e-6 degrees on a side (~ 10 cm). Code now ignores these
  tesseroids on input and warns the user about it. If a tesseroid becomes
  smaller than this during adaptive discretization, the tesseroid effect will
  be computed without division.  The user will be warned when this happens.
  (`PR 228 <https://github.com/fatiando/fatiando/pull/228>`__)
* **New** reduction to the pole and upward continuation with FFT in
  ``fatiando.gravmag.transform``. The pole reduction allows both remanent and
  induced magnetization. Upward continuation is more stable and faster than the
  old space domain approach that was implemented.
  (`PR 156 <https://github.com/fatiando/fatiando/pull/156>`__)
* **IMPORTANT BUG FIX**: Fixed wrong ordering of nodes in
  ``fatiando.mesher.PointGrid``. The order of nodes had the same problem as the
  regular grids (fixed in
  `196 <https://github.com/fatiando/fatiando/pull/196>`__). This was not caught
  before because ``PointGrid`` didn't use ``gridder.regular`` to generate its
  internal regular grid. This is an example of why reuse is a good thing! Tests
  now should catch any future problems.
  (`PR 209 <https://github.com/fatiando/fatiando/pull/209>`__)
* **IMPORTANT BUG FIX**: ``fatiando.gridder.regular`` and many other places in
  Fatiando were using the wrong convention for x, y dimensions.
  x should point North and y East. Thus, a data matrix (regular grid) should
  have x varying in the lines and y varying in the columns. This is
  **opposite** what we had. This fix also changes the ``shape`` argument to be
  ``(nx, ny)`` instead of ``(ny, nx)``. **Users should be aware of this and
  double check their code.**
  (`PR 196 <https://github.com/fatiando/fatiando/pull/196>`__)
* More stable derivatives in ``fatiando.gravamag.transform``. The horizontal
  derivatives default to central finite-differences for greater stability. The
  FFT based derivatives use a grid padding to avoid edge effects.
  Thanks to `Matteo Niccoli <https://mycarta.wordpress.com/>`__ for suggesting
  this fix.
  (`PR 196 <https://github.com/fatiando/fatiando/pull/196>`__)
* **Renamed** ``fatiando.gravmag.fourier.ansig`` to
  ``fatiando.gravmag.transform.tga``
  (`PR 186 <https://github.com/fatiando/fatiando/pull/186>`__)
* **Remove** ``fatiando.gravmag.fourier`` by moving relevant functions into
  ``fatiando.gravmag.transform``.
  (`PR 186 <https://github.com/fatiando/fatiando/pull/186>`__)
* **New** ``seismic_wiggle`` and ``seismic_image`` plotting functions for
  seismic data in ``fatiando.vis.mpl``
  (`PR 192 <https://github.com/fatiando/fatiando/pull/192>`__)
* **Remove** OpenMP parallelism from the ``fatiando.gravmag`` Cython coded
  forward modeling. Caused the majority of our install problems and didn't
  offer a great speed up anyway (< 2x). Can be replaced by ``multiprocessing``
  parallelism without the install problems
  (`PR 177 <https://github.com/fatiando/fatiando/pull/177>`__)
* Tesseroid forward modeling functions in ``fatiando.gravmag.tesseroid`` take
  an optional ``pool`` argument. Use it to pass an open
  ``multiprocessing.Pool`` for the function to use. Useful to avoid processes
  spawning overhead when calling the forward modeling many times
  (`PR 183 <https://github.com/fatiando/fatiando/pull/183>`__)
* **BUG FIX**: Avoid weird numba error when tesseroid has zero volume. Let to
  better sanitizing the input model. Tesseroids with dimensions < 1cm are
  ignored because they have almost zero gravitational effect
  (`PR 179 <https://github.com/fatiando/fatiando/pull/179>`__)
* Ported the tesseroid forward modeling code from Cython to numba. This is
  following the discussion on issue
  `#169 <https://github.com/fatiando/fatiando/issues/169>`__ to make installing
  less of burden by removing the compilation step. The numba code runs just as
  fast. New functions support multiprocessing parallelism.
  Thanks to new contributor Graham Markall for help with numba.
  (`PR 175 <https://github.com/fatiando/fatiando/pull/175>`__)
* Better documentation and faster implementation of
  ``fatiando.gravmag.tesseroid``
  (`PR 118 <https://github.com/fatiando/fatiando/pull/118>`__)
* **BUG FIX**: Replace ``matplotlib.mlab.griddata`` with
  ``scipy.interpolate.griddata`` in ``fatiando.gridder.interp`` to avoid
  incompatibilities when using ``matplotlib > 1.3``
  (at least in MacOS). Nearest neighbor interpolation method flagged as ``nn``
  was removed. Now it becomes only ``nearest``. Also replace ``matplotlib``
  with ``scipy`` in ``fatiando.mesher.PrismMesh.carvetopo``
  (`PR 148 <https://github.com/fatiando/fatiando/pull/148>`_)
* **New class** ``fatiando.gravmag.basin2d.PolygonalBasinGravity`` for 2D
  gravity inversion for the relief of a basin.
  (`PR 149 <https://github.com/fatiando/fatiando/pull/149>`__)
* Significant progress on the :ref:`Developer Guide <develop>`. From getting
  started to making a release on PyPI.
  (`PR 144 <https://github.com/fatiando/fatiando/pull/144>`__)
* **Removed** package ``fatiando.gui``. This was an experimental and temporary
  package to explore interactivity. Given new developments, like the
  IPython HTML widgets,
  it is no longer relevant. The package will be replaced by package specific
  ``interactive`` modules.
  From the original classes implemented in this package, only ``Moulder`` has
  been saved.
  (`PR 143 <https://github.com/fatiando/fatiando/pull/143>`__)
* Moved ``Moulder`` to the **new module** ``fatiando.gravmag.interactive``.
  Completely rewrote the application. It now allows editing, moving, and
  deleting polygons, persisting the application to a pickle file and reloading,
  etc.
  (`PR 143 <https://github.com/fatiando/fatiando/pull/143>`__)


Version 0.3
-----------

**Release date**: 2014-10-28

**doi**: `10.5281/zenodo.16205 <https://doi.org/10.5281/zenodo.16205>`__

**Changes**:

* **New module** ``fatiando.gravmag.normal_gravity`` to calculate normal
  gravity (the gravity of reference ellipsoids).
  (`PR 133 <https://github.com/fatiando/fatiando/pull/133>`_)
* Using `versioneer <https://github.com/warner/python-versioneer>`__ to manage
  version numbers. Access the version number + git commit hash from
  ``fatiando.__version__``.
  (`PR 117 <https://github.com/fatiando/fatiando/pull/117>`_)
* **BUG FIX**: ``fatiando.gravmag.prism``
  gravitational field functions give correct results in all sides of the prism.
  There were singularities due to log(0) and weird results because of arctan2.
  (`PR 113 <https://github.com/fatiando/fatiando/pull/113>`_)
* `PEP8 <https://www.python.org/dev/peps/pep-0008/>`__ compliance (started by
  @SamuelMarks).
  (`PR 115 <https://github.com/fatiando/fatiando/pull/115>`_)
* Multithreaded parallelism with OpenMP in
  ``fatiando.gravmag.sphere``,
  ``fatiando.gravmag.polyprism`` and
  ``fatiando.gravmag.prism``.
  Speedups are range from practically none to over 3x.
  Works automatically.
  **Windows users will have to install an extra dependency!**
  See the :ref:`install instructions <install>`.
  (`PR 106 <https://github.com/fatiando/fatiando/pull/106>`_)
* Faster Cython implementations of
  ``fatiando.gravmag.sphere`` and
  ``fatiando.gravmag.polyprism``.
  Also separated gravmag forward modeling functions into "kernels" for gravity
  tensor components. This allows them to be reused in the magnetic field
  computations.
  (`PR 105 <https://github.com/fatiando/fatiando/pull/105>`_)
* Added ``xy2ne`` flag for ``square`` and ``points`` functions in
  ``fatiando.vis.mpl``.
  (`PR 94 <https://github.com/fatiando/fatiando/pull/94>`_)
* **New** class ``LCurve`` in ``fatiando.inversion.regularization`` for
  estimating the regularization parameter using an L-curve criterion.
  (`PR 90 <https://github.com/fatiando/fatiando/pull/90>`_)
* Added support for ``vmin`` and ``vmax`` arguments in
  ``fatiando.vis.mpl.contourf``.
  (`PR 89 <https://github.com/fatiando/fatiando/pull/89>`_)
* **New** module ``fatiando.gravmag.magdir`` for
  estimating the total magnetization vector of multiple sources.
  (`PR 87 <https://github.com/fatiando/fatiando/pull/87>`_)


Version 0.2
-----------

**Release date**: 2014-01-15

**doi**: `10.6084/m9.figshare.1115194 <https://doi.org/10.6084/m9.figshare.1115194>`__

**Changes**:

* Complete re-implementation of ``fatiando.inversion`` and all modules that
  depended on it. Inversion routines now have a standard interface.
  (`PR 72 <https://github.com/fatiando/fatiando/pull/72>`_)
* Added moving window solution for Euler deconvolution in
  ``fatiando.gravmag.euler``.
  (`PR 85 <https://github.com/fatiando/fatiando/pull/85>`_)
* Renamed the ``fatiando.io`` module to ``fatiando.datasets``
  (`PR 82 <https://github.com/fatiando/fatiando/pull/82>`_)
* ``fatiando.utils.contaminate`` can now take multiple data vectors and stddevs
* 2x speed-up of ``fatiando.gravmag.talwani`` with smarter numpy array usage.
  (`PR 57 <https://github.com/fatiando/fatiando/pull/57>`_)
* 300x speed-up of ``fatiando.seismic.ttime2d`` with new Cython code.
  (`PR 62 <https://github.com/fatiando/fatiando/pull/62>`_)
* Speed-up of ``fatiando.gravmag.tesseroid`` with better Cython code.
  (`PR 58 <https://github.com/fatiando/fatiando/pull/58>`_)
* Various tweaks to ``fatiando.vis.myv``.
  (`PR 56 <https://github.com/fatiando/fatiando/pull/56>`_ and
  `PR 60 <https://github.com/fatiando/fatiando/pull/60>`_)
* **New** gravity gradient tensor modeling with spheres in
  ``fatiando.gravmag.sphere``.
  (`PR 55 <https://github.com/fatiando/fatiando/pull/55>`_
  and `PR 24 <https://github.com/fatiando/fatiando/pull/24>`_,
  the first one by Vanderlei)
* **New** function ``fatiando.gridder.profile`` to extract a profile
  (cross-section) from map data.
  (`PR 46 <https://github.com/fatiando/fatiando/pull/46>`_)
* Better support for random numbers. ``contaminate`` function now guaranteed to
  use errors with zero mean. Can now control the random seed used in all
  functions relying on random numbers. (`PR 41
  <https://github.com/fatiando/fatiando/pull/41>`_)
* **New** scalar wave 2D finite differences modeling in
  ``fatiando.seismic.wavefd``.
  (`PR 38 <https://github.com/fatiando/fatiando/pull/38>`_ the first by Andre)
* **New** algorithms in ``fatiando.seismic.wavefd`` for elastic waves and a new
  scalar wave solver! Using staggered grid finite differences makes elastic
  wave methods are more stable.
  (`PR 52 <https://github.com/fatiando/fatiando/pull/52>`_)
* **New** ``extrapolate_nans`` function in ``fatiando.gridder`` to fill NaNs
  and masked values in arrays using the nearest data point.
* ``interp`` function of ``fatiando.gridder`` has option to extrapolate values
  outside the convex hull of the data (enabled by default). Uses better cubic
  interpolation by default and returns 1D arrays like the rest of fatiando,
  instead of 2D.
  (`PR 44 <https://github.com/fatiando/fatiando/pull/44>`_
  and `PR 42 <https://github.com/fatiando/fatiando/pull/42>`_)
* **New** function to load a grid in Surfer format.
  (`PR <https://github.com/fatiando/fatiando/pull/33>`_ the first by Henrique)
* **New** module ``fatiando.gravmag.eqlayer`` for equivalent layer processing
  of potential fields.
* Refactored all magnetic modeling and inversion to use either scalar or vector
  magnetization.
* ``Seed`` class of ``fatiando.gravmag.harvester`` can now be used as a
  ``Prism`` object.
* ``fatiando.gravmag.harvester`` now supports data weights and magnetic data
  inversion.
* Removed module ``fatiando.logger``.
  (`PR 30 <https://github.com/fatiando/fatiando/pull/30>`_)


Version 0.1
-----------

**Release date**: 2013-04-12

**doi**: `10.5281/zenodo.16207 <https://doi.org/10.5281/zenodo.16207>`__

**Changes**:

* Change license to BSD (see the :ref:`license text <license>`).
* The API is now fully accessible by only importing ``fatiando``
* Added a Cookbook section to the documentation with all the
  sample scripts from the cookbook folder.
* Implemented "Robust 3D gravity gradient inversion by planting anomalous
  densities" by Uieda and Barbosa (2012) in ``fatiando.gravmag.harvester``
* Added harvester command line program that runs this new inversion
* Added magnetic total field anomaly function to ``fatiando.gravmag.prism``
* Added ``fatiando.vis.myv.savefig3d`` to save a Mayavi scene
* Added ``fatiando.vis.myv.polyprisms`` 3D plotter function for PolygonalPrism
* Added ``fatiando.vis.myv.points3d`` 3D plotter function for points
* Added gravity gradient tensor components and magnetic total field anomaly to
  ``fatiando.gravmag.polyprism``
* Added option to control the line width to ``prisms`` and ``polyprisms`` in
  ``fatiando.vis.myv``
* Added module ``fatiando.gravmag.tensor`` for processing gradient tensor data.
  Includes eigenvalues and eigenvectors, tensor invariants, center of mass
  estimation, etc.
* Added module ``fatiando.gravmag.imaging`` with imaging methods for potential
  fields
* Added module ``fatiando.gravmag.euler`` with Euler deconvolution methods for
  potential field data
* Added module ``fatiando.seismic.wavefd`` with 2D Finite Difference
  simulations of elastic seismic waves
* Added unit conversion functions to ``fatiando.utils``
* Added tesseroids forward modeling ``fatiando.gravmag.tesseroid``, meshing and
  plotting with Mayavi
* New ``fatiando.io`` module to fetch models and data from the web and convert
  them to useful formats (for now supports the CRUST2.0 global curstal model)
* If building inplace or packaging, the setup script puts the Mercurial
  changeset hash in a file. Then fatiando.logger.header
  loads the hash from file and put a "Unknown" if it can't read.
  This way importing fatiando won't fail if the there is no changeset
  information available.
* ``fatiando.mesher.PrismMesh.dump``: takes a mesh file, a property file and a
  property name. Saves the output to these files.
* Transformed all geometric elements (like Prism, Polygon, etc) into classes
* Ported all C extensions to Python + Numpy. This way compiling is not a
  prerequisite to installing
* Using `Cython <http://cython.org/>`_ for optional extension modules. If
  they exist, they are loaded to replace the Python + Numpy versions. This all
  happens at runtime.
* Move all physical constants used in ``fatiando`` to module
  ``fatiando.constants``
* Data modules hidden inside functions in ``fatiando.gravmag.basin2d``
* Functions in ``fatiando.gravmag.basin2d`` spit out Polygons instead of the
  vertices estimated. Now you don't have to build the polygons by hand.
