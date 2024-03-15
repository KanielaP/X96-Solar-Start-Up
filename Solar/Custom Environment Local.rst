==================
Custom Environment
==================

The easiest way to add support for a custom environment is to edit
``rl_zoo3/import_envs.py`` and register your environment here. Then, you
need to add a section for it in the hyperparameters file
(``hyperparams/algo.yml`` or a custom yaml file that you can specify
using ``--conf-file`` argument). This can be done two ways Locally and Google Colab.

==================
Setting Up Locally
==================

Local setup would be best for most systems and people who want to run custom environments. Most of our testing was done in the Google Colab version, but this works as well. 

----------------------------
Registering Your Environment
----------------------------
A. Create Your Custom Environment

The procedure of setting up the custom environment is adapted from the instructions `here <https://gymnasium.farama.org/tutorials/gymnasium_basics/environment_creation/>`_ .

Install and add your custom environment in 'gym-examples' from `here <https://drive.google.com/drive/folders/12_E0PUNEwcEwghveHbzdA8hf44UtYe60>`_ . 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Check if the Custom Environment Passes the Basic Test
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the custom environment conforms to the rules of Gymnasium, we should be able to 'make()' the environment.

.. code-block:: python

   env = gym.make('gym_demand_response/DemandResponseSimpleNoExport-v0')
   print(env)


B. Modify 'import_envs.py'

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Register the Custom Environment in RL Baselines3 Zoo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We have verified that the custom environment is registered in Gymnasium and has passed the basic test.

Since we train the algorithm in RL Baselines3 Zoo, we also need to register the custom environment in RL Baselines3 Zoo, too. This is done by two steps.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Step 1: Update import_envs.py
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We first update import_envs.py:

- Navigate to rl-baselines3-zoo/rl-zoo/
- Add the following lines in import_envs.py:

.. code-block:: python

   import sys
   
   path_to_add = '/content/drive/MyDrive/UHM/RL Demand Response/gym-examples'
   if path_to_add not in sys.path:
       sys.path.append(path_to_add)
   
   import gym_demand_response

An example of the modified 'import_envs.py' is `here <https://drive.google.com/file/d/14uRasiWwBM8gawfPhNzgx4NHv-5LXLs4/view>`_ .

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Step 2: Add the Custom Environment in the yml File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Let's say we want to train the A2C algorithm. We need to add configurations of the training process under the custom environment.
- Navigate to rl-baselines3-zoo/hyperparams
- Find the .yml file for the algorithm we want to use (e.g., a2c.yml if we want to train an agent using the A2C algorithm)
- Add the following code:

.. code-block:: python

   gym_demand_response/DemandResponseSimpleNoExport-v0:
     n_envs: 8
     n_timesteps: !!float 5e5
     policy: 'MlpPolicy'
     ent_coef: 0.0

his tells the A2C algorithm to use these hyperparameters when training under the custom environment 'gym_demand_response/DemandResponseSimple-v0'.

An example of the modified 'a2c.yml' is `here <https://drive.google.com/file/d/12nfaBPK4FY_HEcmO-fyHsyuV0Hh-zgsm/view>`_ .

Now we are ready to train the A2C algorithm in our custom environment!

---------------------------
Update Hyperparameter Files
---------------------------

A. Locate the Hyperparameters File

Hyperparameters for different algorithms are defined in YAML files located in the hyperparams directory. Each algorithm has its own file, named <algo>.yml, where <algo> is the algorithm's name, such as a2c, ppo, etc.

B. Add Your Custom Environment Section

- Open the YAML file for the algorithm you want to use with your custom environment. If you plan to test multiple algorithms, you'll need to update each of their YAML files accordingly.
- Add a section for your custom environment where you define the hyperparameters specific to running your environment with the algorithm. Use the environment ID you registered earlier (e.g., MyCustomEnv-v0).

For example, to add a section for MyCustomEnv-v0 in the a2c.yml file:

.. code-block:: python

  MyCustomEnv-v0:
  n_timesteps: 100000
  policy: 'MlpPolicy'
  lr_schedule: 'constant'
  # Add other hyperparameters here, following the structure used for other environments

-----------------------
Running your Experiment
-----------------------

Now that you've registered your environment and defined its hyperparameters, you can run experiments using the RL Baselines3 Zoo command-line interface. Specify your environment using the '--env' argument and the algorithm with '--algo'. If you created a custom hyperparameters file, use the '--conf-file' argument to specify it.

Example command:

.. code-block:: python

  !python -m rl_zoo3.train --algo a2c --env MyCustomEnv-v0 --n-timesteps 100000

This setup allows you to integrate custom environments into RL Baselines3 Zoo, enabling experimentation with different RL algorithms provided by the framework. Remember to ensure that your custom environment is accessible (e.g., by placing it in the correct directory or adjusting your Python path) when running experiments in environments like Google Colab.

