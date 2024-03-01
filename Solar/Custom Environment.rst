==================
Custom Environment
==================

The easiest way to add support for a custom environment is to edit
``rl_zoo3/import_envs.py`` and register your environment here. Then, you
need to add a section for it in the hyperparameters file
(``hyperparams/algo.yml`` or a custom yaml file that you can specify
using ``--conf-file`` argument).


============================
Registering Your Environment
============================
A. Create Your Custom Environment
Ensure your custom environment follows the OpenAI Gym interface. Including properties such as observation_space and action_space. 

+ reset()
+ step(action)
+ render()
+ close()


B. Modify 'import_envs.py'
Locate the 'rl_zoo3/import_envs.py' file in the RL Baselines3 Zoo repository.
Edit this file to import your custom environment module. You will need to add a line that imports your environment, similar to how Gym environments are imported.

Assuming 'my_custom_env.py' is in the same directory as 'import_envs.py' or is otherwise in your Python path: 

.. code-block:: python

   from my_custom_env import MyCustomEnv

Register your environment with Gym. This can usually be done within the same 'import_envs.py' file or within your environment's module. The registration looks something like this:

.. code-block:: python

  import gym
  gym.envs.registration.register(
    id='MyCustomEnv-v0',  # Use an env ID that follows Gym's naming conventions
    entry_point='my_custom_env:MyCustomEnv',  # Format is 'module_name:ClassName'
  )

===========================
Update Hyperparameter Files
===========================

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

=======================
Running your Experiment
=======================

Now that you've registered your environment and defined its hyperparameters, you can run experiments using the RL Baselines3 Zoo command-line interface. Specify your environment using the '--env' argument and the algorithm with '--algo'. If you created a custom hyperparameters file, use the '--conf-file' argument to specify it.

Example command:

.. code-block:: python

  !python -m rl_zoo3.train --algo a2c --env MyCustomEnv-v0 --n-timesteps 100000

This setup allows you to integrate custom environments into RL Baselines3 Zoo, enabling experimentation with different RL algorithms provided by the framework. Remember to ensure that your custom environment is accessible (e.g., by placing it in the correct directory or adjusting your Python path) when running experiments in environments like Google Colab.
