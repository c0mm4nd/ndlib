******************
Dynamic Network SI
******************

The SI model was introduced in 1927 by Kermack [#]_.
 
In this model, during the course of an epidemics, a node is allowed to change its status only from **Susceptible** (S) to **Infected** (I).

The model is instantiated on a graph having a non-empty set of infected nodes.

SI assumes that if, during a generic iteration, a susceptible node comes into contact with an infected one, it becomes infected with probability β: once a node becomes infected, it stays infected (the only transition allowed is S→I).

The dSI implementation assumes that the process occurs on a directed/undirected dynamic network.

--------
Statuses
--------

During the simulation a node can experience the following statuses:

===========  ====
Name         Code
===========  ====
Susceptible  0
Infected     1
===========  ====

----------
Parameters
----------

=====  =====  ===============  =======  =========  =====================
Name   Type   Value Type       Default  Mandatory  Description
=====  =====  ===============  =======  =========  =====================
beta   Model  float in [0, 1]           True       Infection probability
=====  =====  ===============  =======  =========  =====================

The initial infection status can be defined via:

    - **percentage_infected**: Model Parameter, float in [0, 1]
    - **Infected**: Status Parameter, set of nodes

The two options are mutually exclusive and the latter takes precedence over the former.

-------
Methods
-------

The following class methods are made available to configure, describe and execute the simulation:

^^^^^^^^^
Configure
^^^^^^^^^
.. autoclass:: ndlib.models.dynamic.DynSIModel.DynSIModel
.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.__init__(graph)

.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.set_initial_status(self, configuration)
.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.reset(self)

^^^^^^^^
Describe
^^^^^^^^

.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.get_info(self)
.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.get_status_map(self)

^^^^^^^^^^^^^^^^^^
Execute Simulation
^^^^^^^^^^^^^^^^^^
.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.iteration(self)
.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.execute_snapshots(bunch_size, node_status)
.. automethod:: ndlib.models.dynamic.DynSIModel.DynSIModel.execute_iterations(node_status)

-------
Example
-------

In the code below is shown an example of instantiation and execution of an DynSI simulation on a dynamic random graph: we set the initial set of infected nodes as 5% of the overall population and a probability of infection of 1%.

.. code-block:: python

    import networkx as nx
    import dynetx as dn
    import ndlib.models.ModelConfig as mc
    import ndlib.models.dynamic.DynSIModel as si

    # Dynamic Network topology
    dg = dn.DynGraph()

    for t in past.builtins.xrange(0, 3):
        g = nx.erdos_renyi_graph(200, 0.05)
        dg.add_interactions_from(g.edges(), t)

    # Model selection
    model = si.DynSIModel(dg)

    # Model Configuration
    config = mc.Configuration()
    config.add_model_parameter('beta', 0.01)
    config.add_model_parameter("percentage_infected", 0.1)
    model.set_initial_status(config)

    # Simulate snapshot based execution
    iterations = model.execute_snapshots()

    # Simulation interaction graph based execution
    iterations = model.execute_iterations()


.. [#] W. O. Kermack and A. McKendrick, “A Contribution to the Mathematical Theory of Epidemics,” Proceedings of the Royal Society of London. Series A, Containing Papers of a Mathematical and Physical Character, vol. 115, no. 772, pp. 700–721, Aug. 1927.