****
SEIS
****


In the SEIR model, during the course of an epidemics, a node is allowed to change its status  from **Susceptible** (S) to **Exposed** (E) to **Infected** (I), then to **Removed** (R).

The model is instantiated on a graph having a non-empty set of infected nodes.

SEIR assumes that if, during a generic iteration, a susceptible node comes into contact with an infected one, it becomes infected after an exposition period with probability beta, than it can be switch to removed with probability gamma (the only transition allowed are S→I→R).


--------
Statuses
--------

During the simulation a node can experience the following statuses:

===========  ====
Name         Code
===========  ====
Susceptible  0
Exposed		 1
Infected     2
===========  ====

----------
Parameters
----------

======  =====  ===============  =======  =========  =====================
Name    Type   Value Type       Default  Mandatory  Description
======  =====  ===============  =======  =========  =====================
beta    Model  float in [0, 1]           True       Infection probability
lambda  Model  float in [0, 1]           True       Removal probability
alpha   Model  float in [0, 1]           True       Incubation period
======  =====  ===============  =======  =========  =====================

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
.. autoclass:: ndlib.models.epidemics.SEISModel.SEISModel
.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.__init__(graph)

.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.set_initial_status(self, configuration)
.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.reset(self)

^^^^^^^^
Describe
^^^^^^^^

.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.get_info(self)
.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.get_status_map(self)

^^^^^^^^^^^^^^^^^^
Execute Simulation
^^^^^^^^^^^^^^^^^^
.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.iteration(self)
.. automethod:: ndlib.models.epidemics.SEISModel.SEISModel.iteration_bunch(self, bunch_size)


-------
Example
-------

In the code below is shown an example of istantiation and execution of an SIR simultion on a random graph: we set the initial set of infected nodes as 5% of the overall population, a probability of infection of 1%, and a removal probability of 0.5%.

.. code-block:: python

    import networkx as nx
    import ndlib.models.ModelConfig as mc
    import ndlib.models.epidemics.SEISModel as seis

    # Network topology
    g = nx.erdos_renyi_graph(1000, 0.1)

    # Model selection
    model = seis.SEISModel(g)

    # Model Configuration
    cfg = mc.Configuration()
    cfg.add_model_parameter('beta', 0.01)
    cfg.add_model_parameter('lambda', 0.005)
    cfg.add_model_parameter('alpha', 0.05)
    cfg.add_model_parameter("percentage_infected", 0.05)
    model.set_initial_status(cfg)

    # Simulation execution
    iterations = model.iteration_bunch(200)

