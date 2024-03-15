==================
Custom Environment
==================

The easiest way to add support for a custom environment is to edit
``rl_zoo3/import_envs.py`` and register your environment here. Then, you
need to add a section for it in the hyperparameters file
(``hyperparams/algo.yml`` or a custom yaml file that you can specify
using ``--conf-file`` argument). This can be done two ways Locally and Google Colab.


=======================
Setting Up Google Colab
=======================

Most of our testing was through Google Colab which has a whole different setup to work properly and run your custom environment without needing to download or install much. Please download the repository into your google drive and follow the next steps in Google Colab. More detailed setup `here <https://colab.research.google.com/drive/1MYXZuM2l6txG5T7EHVmkTTzZJanou5y_?usp=sharing#scrollTo=XJy9QoDC7XA7>`_ .

--------------------
Install Dependencies
--------------------

.. code-block:: python

   !apt-get update && apt-get install swig cmake ffmpeg freeglut3-dev xvfb


-----------------------
Setup RL Baselines3 Zoo
-----------------------

- Mount the Google Drive to access it from the notebook 
- Go to the directory containing RL Baselines3 Zoo 

.. code-block:: python

   from google.colab import drive
   drive.mount('/content/drive')
   
   # Navigate to the directory
   %cd "/content/drive/My Drive/UHM/RL Demand Response"

- This should be Mounted at '/content/drive'
- Directory should be at '/content/drive/My Drive/UHM/RL Demand Response'

~~~~~~~~~~~~~~~~~~~~~~~~
Install pip Dependencies
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   %cd rl-baselines3-zoo/
   !pip install -r requirements.txt
   !pip install -e .[plots,tests]

----------------------------
Setup the Custom Environment
----------------------------

The procedure of setting up the custom environment is adapted from the instructions `here <https://gymnasium.farama.org/tutorials/gymnasium_basics/environment_creation/>`_ .


~~~~~~~~~~~~~~~~~~
Clone gym-examples
~~~~~~~~~~~~~~~~~~

We will add our custom environments in `gym-examples <https://github.com/Farama-Foundation/gym-examples>`_ . Clone and download into Google Drive. 

The folder 'gym-examples' should be at the same level of 'rl-baselines3-zoo', not inside 'rl-baselines3-zoo'. In this way, we will make minimal modifications to 'rl-baselines3-zoo'.

.. code-block:: python

   import os
   
   %cd "/content/drive/My Drive/UHM/RL Demand Response"
   
   # Check if the directory exists
   if not os.path.exists('gym-examples'):
    # Directory does not exist, so clone the repository
    !git clone https://github.com/Farama-Foundation/gym-examples
   else:
    # Directory exists, print a message
    print("The directory 'gym-examples' already exists.")


This code block is to check if folder is correctly installed. 

The 'gym-examples' package contains one environment 'GridWorld' in 'gym_examples'.

We need to add our own custom environment. Please copy and paste the folder which contains your environment in the folder 'gym_demand_response'.

Make sure that the structure and files in your Google Drive are the same as the folder provided `here <https://drive.google.com/drive/folders/12_E0PUNEwcEwghveHbzdA8hf44UtYe60>`_ . 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Add the Directory to the System Path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   import sys
   
   path_to_add = '/content/drive/MyDrive/UHM/RL Demand Response/gym-examples'  # You may need to change the directory according to your own Google Drive
   if path_to_add not in sys.path:
       sys.path.append(path_to_add)
   print(sys.path)


-----------------------------------------------------------
Verifying the Custom Environment is Registered in Gymnasium
-----------------------------------------------------------

Import the custom environment 'gym_demand_response' and list all the environment registered in gym.

You should be able to see the custom environment alphabetically sorted in the list.

.. code-block:: python

   import gymnasium as gym
   import gym_demand_response
   
   # Get the list of all registered environment IDs
   env_ids = [env_spec.id for env_spec in gym.envs.registry.values()]
   for env_id in sorted(env_ids):  # Sorting the list for easier reading
       print(env_id)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Check if the Custom Environment Passes the Basic Test
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the custom environment conforms to the rules of Gymnasium, we should be able to 'make()' the environment.

.. code-block:: python

   env = gym.make('gym_demand_response/DemandResponseSimpleNoExport-v0')
   print(env)



-----------------------------------------------------------
Register the Custom Environment in RL Baselines3 Zoo
-----------------------------------------------------------



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

----------------------------------------------
Train a RL Algorithm in the Custom Environment
----------------------------------------------

.. code-block:: python

   %cd "/content/drive/My Drive/UHM/RL Demand Response/rl-baselines3-zoo/"
   !python -m rl_zoo3.train --algo a2c --env gym_demand_response/DemandResponseSimpleNoExport-v0 --n-timesteps 100000

We continue training for another 1 million steps:

.. code-block:: python

   !python -m rl_zoo3.train --algo a2c --env gym_demand_response/DemandResponseSimpleNoExport-v0 --n-timesteps 1000000 -i logs/a2c/gym_demand_response-DemandResponseSimpleNoExport-v0_1/gym_demand_response-DemandResponseSimpleNoExport-v0.zip



----------------------------
Illustrate the Trained Agent
----------------------------


~~~~~~~~~~~~~~
Load the Model
~~~~~~~~~~~~~~

The model is saved in best_model.zip in the folder indicated at the end of the training.

.. code-block:: python

   from stable_baselines3 import A2C
   
   model_path = 'logs/a2c/gym_demand_response-DemandResponseSimpleNoExport-v0_1/best_model.zip'  # Adjust the path to where your model is saved
   model = A2C.load(model_path)


~~~~~~~~~~~~~~~~~~~~~
Illustrate the Policy
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   import numpy as np
   import matplotlib.pyplot as plt
   
   net_demands, actions = [], []
   
   num_time_steps = 1000
   
   for time_step in range(num_time_steps):
       random_battery_level = np.random.uniform(0.0, 27.0)     # random battery level
       random_temperature = np.random.uniform(25.0, 35.0)      # random temperature
       random_demand = np.random.uniform(0.0, 4.0)             # random household demand
       random_solar = np.random.uniform(0.0, 5.0)              # random solar production
       hour_of_day = 0                                         # start from midnight
   
       action, _states = model.predict(np.array([random_battery_level, random_temperature, random_demand, random_solar, hour_of_day]), deterministic=True)
       net_demands.append(random_demand - random_solar)  # Collect the net demand (i.e., demand - solar)
       actions.append(action)
   
   plt.scatter(net_demands, actions)
   plt.xlabel('Net Demand')
   plt.ylabel('Action Taken')
   plt.title('Action vs. Net Demand')
   plt.show()

