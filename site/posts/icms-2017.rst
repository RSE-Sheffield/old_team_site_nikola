<!--
.. title: Computational Mathematics with Jupyter workshop
.. author: Will Furnass
.. slug: icms-2017
.. date: 2017-03-12 22:00:00 UTC
.. tags: conference
.. category:
.. link:
.. description:
.. type: text
-->

Back in mid-January three members of the University of Sheffield's Research Software Engineering Team 
(me, `Mike Croucher <link://section_index/contact>`_ and `Tania Allard <link://slug/tania_allard>`_) 
spent a week at a `Computational Mathematics with Jupyter workshop`_, hosted at Edinburgh's `International Centre for Mathematical Sciences`_.

This brought together the many members of the consortium working on the OpenDreamKit_ Horizon 2020 European Research Infrastructure project.  
The overall aim of the project is broad (to further the open-source computational mathematics ecosystem) 
so it was unsurprising that the collective experience of the attendees was too.  
The attendees generally fell into one of the following four camps: 

* Researchers interested in solving `Group Theory`_ and `Semigroup`_ problems using the `GAP`_ software, 
  some of which were involved with developing a `Jupyter kernel`_ for GAP;
* Others interested in the `SageMath`_ computational mathematics ecosystem 
  (which is particularly strong for computational algebra) and 
  working on a Jupyter kernel for it;
* Research Software Engineers working on interactive widgets, visualisation tools and workflow tools for `Jupyter`_;
* People with experience/interest in using computational mathematics tools for teaching purposes.

.. figure:: /images/icms-2017-attendees.jpg
   :align: center
   :alt: The attendees of the workshop
   :scale: 100 %

   The attendees of the workshop.

The structure was different from conferences I'd attended previously: 
for each of the five days we listened and debated presentations in the morning then
busied ourselves with `code sprints`_ in the afternoons.

Correctness, sustainability and human fallibility
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Our own Mike Croucher kicked things off by asking 
`Is your research software correct? <http://mikecroucher.github.io/MLPM_talk/>`_ 
in which he presented **Croucher's Law**:

    I can be an idiot and **will** make mistakes.

with the corollary that

    You are no different!

He argued, convincingly, that in our research we therefore need to put in place safeguards 
to lessen the chance and impact of mistakes, and 
proposed the following as partial solutions:

* Automate (aka learn to program)
* Write code in a (very) high-level language
* Get some training
* Use version control
* Get a code buddy (Maybe an `RSE <http://rse.shef.ac.uk/contact/>`_!)
* Share your code and data openly
* Use literate computing technologies
* Write tests
* Cite code 
    
`Raniere Silva`_ from the `Software Sustainability Institute`_ followed on with a complementary talk on how to make computational mathematics software more sustainable.  He commented that odd numerical bugs can easily creep in over time (e.g. differing floating point behaviour between Python 2 and 3) but that we can maintain confidence in software using version control, continuous integration, good documentation, tutorials, knowledge bases, instant messaging and by developing communities around the software we value.

`Alexander Konovalov <https://blogs.cs.st-andrews.ac.uk/alexk/>`_ then talked about a particular case of making research software more sustainable and portable: he's been using `Docker`_ containers to run `GAP`_.  This lead into a discussion on whether Docker is a sensible solution for archiving/reproducing workflows: will it be around in ten years' time?  Those interested in that particular issue might benefit from attending the forthcoming `Software Sustainability Institute`_ workshop on `Docker Containers for Reproducible Research <https://www.software.ac.uk/c4rr>`_.  

Jupyter: what is it and how can we diff/merge/test Notebooks?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We were then given what was pitched as a **'general introduction' to Jupyter** 
but ended up covering much more ground than anticipated, 
largely due to the speaker being `Thomas Kluyver`_, one of the core IPython developers
(who happens to have gained his PhD from the University of Sheffield).  
Thomas talked about the most significant features 
(literate programming environments; 
the power and versatility of using the browser as a `REPL`_; 
Jupyter's client-server architecture) 
but also touched upon 
**various tools and platforms that have built on Jupyter** including:

* `JupyterHub`_: a multi-user hub which "spawns, manages, and proxies multiple instances of the single-user Jupyter Notebook server".  One of the University of Sheffield's deliverables for the OpenDreamKit project is to get this running on our own computer clusters so users dictate what resources (e.g. cores, memory, GPUs) they want when they start a single-user Notebook session;
* `tmpnb`_: a JupyterHub-like system for launching temporary single-user Notebook sessions in the cloud (which are each backed by a Docker container).  tmpnb powers `https://try.jupyter.org/ <https://try.jupyter.org/>`_ (hosted by RackSpace), which allows people to briefly test out Jupyter without installing anything locally;
* `binder`_: a tool for turning a GitHub repository into a collection of interactive Notebooks, configuring the required environment/dependencies using a Dockerfile, Python requirements.txt file or Conda environment file;
* `nbgrader`_: a tool for distributing coding and/or free text Notebook-based assignments then automatically or manually grading them.  This can integrate with JupyterHub;
* `nbconvert`_: convert a Notebook to HTML/Markdown/PDF/scripts or a custom format (using a `Jinja2`_ template).  It is used by `nbviewer.jupyter.org <nbviewer.jupyter.org>`_ to create online static HTML views of Notebooks.  nbconvert is also used by GitHub for rendering Notebooks;
* `nbparameterise`_:  Often you want design Notebooks to demonstrate/explore the impact of a small number of key variables.  This project of Thomas's allows such variables to be set in the first code cell then the entire Notebook can be run non-interactively and rendered to HTML;
* `jupyterlab`_: This will be the next iteration of Jupyter's UI: rather than exclusively displaying a terminal, Notebook or file editor in the Jupyter interface, instead a multi-tab and multi-pane interface allows you to view and interact with several of these things at once.  It will therefore look and feel a bit more like Spyder/R Studio/MATLAB but this is no bad thing as all of those make good use of screen real estate provided by the wide monitors we all have these days.

.. figure:: /images/jupyterlab.png
   :align: center
   :alt: JupyterLab: the future of the Jupyter interface
   :scale: 100 %

   JupyterLab: the future of the Jupyter interface.

Given the enormity of the Jupyter ecosystem and how quickly it has grown 
it was great to hear from a core developer which related projects he thinks are the most significant and interesting!

Next up, Vidar Fauske gave `this talk <http://opendreamkit.org/meetings/2017-01-16-ICMS/talks/nbdime.pdf>`_ on `nbdime`_, a new tool for `merging <https://en.wikipedia.org/wiki/Merge_(version_control)>`_ and `diffing <https://en.wikipedia.org/wiki/Diff_utility>`_ Jupyter Notebooks.  
The backstory is that for some time we've been recommending Jupyter to those wanting to start using Python or R in their research and we've also been telling everyone to use version control but the diffing and merging tools typically used with version control systems don't work well with Notebooks as they 

* Operate on lines without consideration of whether a file has a nested structure (JSON in the case of Notebooks);
* Base64-encoded binary objects in Notebooks are naively treated in just the same way as text;
* No logic for omitting certain entities (execution counters; cell outputs) from version control (although the wonderful `nbstripout`_ can handle both of these cases when triggered by a `git hook`_).

Ultimately visualising the differences between two Notebooks and merging Notebooks in sensible, useful ways really requires that the tools that perform these functions have some understanding of the structure and purpose of Notebooks: **nbdime** has that awareness:

* The major unit for merging/diffing is the **cell**, the line.
* Input cell merging is string merging whereas
* Cell outputs are treated as atomic: they match or they don't.
* Execution counts are sensibly ignored by default.

nbdime provides a core library, plus command-line and browser interfaces for diffing and merging.

Overall, I'm massively excited about nbdime for facilitating much slicker Notebook-based version controlled workflows and hope it sees widespread adoption and promotion by the likes of `Software Carpentry <https://software-carpentry.org/lessons/>`_.

.. figure:: /images/nbdiff-example.png
   :align: center
   :alt: nbdime's nbdiff tool for viewing the differences between two Notebooks
   :scale: 100 %

   nbdime's nbdiff tool for viewing the differences between two Notebooks.

Hans Fangohr then introduced `nbval`_, a new tool for **automating the valdation of Jupyter Notebooks**.  This could give researchers greater confidence in their workflows: **does a demonstrative Notebook still give the same answers if re-run after making changes to the Notebook's environment (e.g. the package dependencies)?**

nbval, a `pytest`_ plug-in, works as follows: it creates a copy of a Notebook file, executes the copy in the current Python environment, saves the copy Notebook with its new cell outputs then compares the outputs of the two Notebooks.  There are some nice features to control the granularity of testing: flags can be set so certain cells are run but not tested; `regexes <https://en.wikipedia.org/wiki/Regular_expression>`_ can be used to ignore oft-changing output strings (e.g. paths, timestamps, memory addresses).  Images and LaTeX can't be handled yet.

Again, I'm exited about this new tool: being able to package both workflow documentation and regression/ acceptance tests as Notebooks is a great idea.  Note that at present both nbdime and nbval include mechanisms for comparing Notebooks but are presently separate projects.  It will be interesting to see if there's any convergence in future.

Interactive widgets in Notebooks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
We were treated to two talks on the `ipywidgets`_ package, which provides **Python and Javascript-backed widgets for interacting with Notebooks** e.g. sliders for assessing the impact of model parameters on trends in embedded `matplotlib`_ plots.

First, `Jeroen Demeyer <https://github.com/jdemeyer>`_ introduced us to the high-level ``interact`` Python decorator function and ``interactive`` class one can use to control function inputs using a HTML+Javascript widget.  He then went on to explain how one can manually reproduce the magic of these mechanisms: you instantiate some (typed) input widgets and output widgets, add them to an on-screen container then associate each input widget with a callback.

Next, `Sylvain Corlay <http://www.proba.jussieu.fr/pageperso/corlay/>`_ talked about the ipywidgets ecosystem and the future direction of the project.  He mentioned several projects that have built on ipywidgets, all of which sound exciting but none of which I'd heard of before this!

* `bqplot <https://github.com/bloomberg/bqplot>`_: a `matplotlib <http://matplotlib.org/>`_ alternative that supports the same API, uses custom ipywidgets and behind the scenes uses `d3.js <https://d3js.org/>`_ for low-level drawing;
* `pythreejs <https://github.com/jovyan/pythreejs>`_: this exposes the API of the `three.js` Javascript/WebGL 3D library to Python; this is a low-level API, not a Python plotting library.  
* `ipyleaflet <https://github.com/ellisonbg/ipyleaflet>`_: a GIS plotting library that uses ipywidgets and the `Leaflet <http://leafletjs.com/>`_ Javascript library.
* `widget-cookiecutter <https://github.com/jupyter-widgets/widget-cookiecutter>`_: a template for creating custom ipywidgets.

The current version of ipywidgets, released since the workshop, includes some interesting developments: much more of the code is now written in Javascript (actually Typescript) rather than Python so widgets state is maintained in JavaScript-land: widgets can therefore now be rendered and manipulated without a Jupyter kernel!  See `this statically-rendered Notebook <http://nbviewer.jupyter.org/github/ipython/ipywidgets/blob/master/docs/source/examples/Widget%20List.ipynb>`_ on GitHub as an example.  Another advantage of migrating the bulk of the code to Javascript is that the widgets should be usable with kernel languages other than Python such as R (once people have written language-specific ipywidgets backends).

Separate to ipywidgets, we were also introduced to `SciviJS`_, a tool currently being developed by `Martin Renou`_ at `LogiLab`_ for **visualising 3D mesh-based geometries in a Juypter Notebook**.  It uses also uses `WebGL`_ / `three.js`_ for rendering so is rather performant.  I can see some ex-colleagues in civil engineering really liking this.  Check out the `online demo <https://demo.logilab.fr/SciviJS/>`_.

Numbas for online computer-aided assessment (CAS)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`Numbas`_ is a open web-based system for formative and summative maths and science tests.  It is being developed by `Christian Lawson-Perfect`_ from the University of Newcastle's `Maths and Stats E-Learning unit <http://www.ncl.ac.uk/maths/outreach/elearning/#overview>`_.  It's very different to teaching environments that use Jupyter (e.g. `SageMathCloud`_) as almost all the code is self-contained HTML+Javascript that is run on the client (for scalability and resilience) and it is for generating closed tests (rather than open mathematical exercises).  Looks very attractive and intuitive from the user's perspective!

Christian also mentioned `Up For Grabs`_, a site of projects wanting help on simpler tasks.  He says it's a good and simple way of getting less experienced developers involved with open-source projects.  As a project maintainer you upload some blurb about your project and tell the site which GitHub Issue tag(s) indicate smaller tasks that are 'up for grabs'.

Case studies of Jupyter usage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Hans Fangohr <http://www.southampton.ac.uk/~fangohr/>`_ from the `University of Southampton <http://cmg.soton.ac.uk/>`_ reported on using Python and Jupyter to encapsulate multi-stage micro-magnetism modelling workflows: 
his team have been able to automate the generation of input files and processing of output files for/from old but robust modelling software (`OOMMF <http://math.nist.gov/oommf/>`_); 
Jupyter then further masks away the complexities of running models.

`Mark Quinn <http://www.sheffield.ac.uk/physics/contacts/mark-quinn>`_ then talked about the impact that `SageMathCloud`_, an online teaching environment which uses Jupyter, has had on the teaching of physics, astronomy and coding at the University of Sheffield.  He's been working with Mike Croucher to develop SageMathCloud courses for the Physics department with the goal of introducing effective programming tuition early in undergraduate Physics degree programmes.  He's now quite a fan of the used coding environment (Jupyter) and SageMathCloud's courseware tools (chat facilities and mechanisms for setting and grading assignments) but has now been using it long enough to identify some challenges/issues too (e.g. students getting confused about the order of execution of cells; students opening many notebooks at once (each of which has a resource footprint).

Mark is involved with the Shepherd Group, who research the efficacy of teaching methods and are based in the same Physics department.  They've recently been studying the impact of using the Jupyter Notebook to undergraduate students who had and hadn't studied Physics at A-Level.  They tested students (at different levels of `Bloom's Taxonomy <https://en.wikipedia.org/wiki/Bloom%27s_taxonomy>`_) before and after teaching and concluded that the Notebooks were suitable for aiding students, regardless of whether they had a Physics background.  Hopefully the `Software Sustainability Institute`_ can lend their support to pedagogical studies of this nature in future.

Other talks
^^^^^^^^^^^
I should note that there were also a number of other talks that focussed on the `GAP`_ and `SageMath`_ computational mathematics software packages: 
I've deliberately not mentioned them here 
so as not to expose my lack of understanding of group theory and semi-groups 
and also this post is long enough already!  
See the `full programme`_ for info on things I've neglected plus links to the presentations.

Coding Sprints (inc. Jupyter Interactions gallery)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After lunch on each of the five days of the workshop we worked on various coding sprints: 
some wrote a Jupyter kernel for GAP, 
others worked on SageMath, 
whilst me and a few others started work on `Jupyter Interactions`_.  
This sprint was suggested by Mike Croucher: 
he thought it would be nice to have a curated set of Notebooks that 
demonstrate how to use different `ipywidgets`_ to manipulate various mathematical objects.  
Several of us wrote Notebooks while 
`Christian Lawson-Perfect`_ quickly put together a very nice `gallery-generating front end <https://github.com/christianp/jupyter-interactions-site>`_:

.. figure:: /images/jupyter-interactions.png
   :align: center
   :alt: Jupyter Interactions Notebook gallery
   :scale: 100 %

   Jupyter Interactions Notebook gallery.

See the `end result here <https://mikecroucher.github.io/jupyter-interactions/>`_.  
Should you wish to submit your own Jupyter Interactions Notebook then 
please `submit a pull request <https://github.com/mikecroucher/jupyter-interactions/pulls>`_!

Culture
^^^^^^^

This was the first time I'd been to a conference where the emphasis was very much on sharing ideas and working together: 
the academic conferences I'd attended prior to this had previously had an air of competition about them.  
Looking forward to meeting up with the OpenDreamKit gang again!

.. _binder: http://mybinder.org/ 
.. _Christian Lawson-Perfect: http://somethingorotherwhatever.com/
.. _code sprints: https://www.drupal.org/sprints
.. _Computational Mathematics with Jupyter workshop: http://opendreamkit.org/meetings/2017-01-16-ICMS/
.. _Docker: http://www.docker.com/ 
.. _full programme: http://opendreamkit.org/meetings/2017-01-16-ICMS/programme/
.. _GAP: https://www.gap-system.org/
.. _git hook: https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
.. _Group Theory: https://en.wikipedia.org/wiki/Group_theory
.. _International Centre for Mathematical Sciences: http://www.icms.org.uk/
.. _ipywidgets: https://ipywidgets.readthedocs.io/en/latest/
.. _Jinja2: http://jinja.pocoo.org/
.. _Jupyter Interactions: https://github.com/mikecroucher/jupyter-interactions
.. _Jupyter kernel: http://jupyter.readthedocs.io/en/latest/projects/kernels.html
.. _Jupyter: http://jupyter.readthedocs.io/
.. _JupyterHub: https://jupyterhub.readthedocs.io/en/latest/
.. _jupyterlab: https://github.com/jupyterlab/jupyterlab
.. _LogiLab: https://www.logilab.org/
.. _Martin Renou: https://twitter.com/Renou_Martin
.. _matplotlib: http://matplotlib.org/
.. _nbconvert: https://nbconvert.readthedocs.io/en/latest/
.. _nbdime: https://nbdime.readthedocs.io/en/latest/
.. _nbgrader: https://nbgrader.readthedocs.io/en/stable/
.. _nbparameterise: https://github.com/takluyver/nbparameterise
.. _nbstripout: https://github.com/kynan/nbstripout
.. _nbval: https://github.com/computationalmodelling/nbval
.. _Numbas: http://www.numbas.org.uk/ 
.. _OpenDreamKit: http://opendreamkit.org/
.. _pytest: http://doc.pytest.org/en/latest/
.. _Raniere Silva: https://www.software.ac.uk/raniere-silva
.. _REPL: https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop
.. _SageMath: http://www.sagemath.org/
.. _SageMathCloud: https://cloud.sagemath.com/
.. _SciviJS: https://www.logilab.org/blogentry/8541176
.. _Semigroup: https://en.wikipedia.org/wiki/Semigroup
.. _Software Sustainability Institute: https://www.software.ac.uk/
.. _Thomas Kluyver: https://twitter.com/takluyver
.. _three.js: https://threejs.org/
.. _tmpnb: https://github.com/jupyter/tmpnb
.. _Up For Grabs: http://up-for-grabs.net/
.. _WebGL: https://www.khronos.org/webgl/
