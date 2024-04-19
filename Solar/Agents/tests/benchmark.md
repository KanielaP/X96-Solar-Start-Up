
## Performance of Trained Agents

This is not the final performance, but rather our starting results. 
It uses our custom environments and uses rlzoo agents. 

*NOTE: this is not a quantitative benchmark as it corresponds to only one run.
This Benchmark is meant to record initial results to be able to edit and
optimize future tests*


"M" stands for Million (1e6)

|  algo  |            env_id             |mean_reward|std_reward|n_timesteps|eval_timesteps|eval_episodes|
|--------|-------------------------------|----------:|---------:|-----------|-------------:|------------:|
|a2c     |LunarLanderContinuous-v2       |     84.225|   145.906|5M         |        149305|          256|
|a2c     |MountainCar-v0                 |   -111.263|    24.087|1M         |        149982|         1348|
|ars     |Acrobot-v1                     |    -82.884|    23.825|500k       |        149985|         1788|
|ars     |Ant-v3                         |   2333.773|    20.597|75M        |        150000|          150|
|ddpg    |AntBulletEnv-v0                |   2399.147|    75.410|1M         |        150000|          150|
|ddpg    |BipedalWalker-v3               |    197.486|   141.580|1M         |        149237|          227|
|dqn     |Acrobot-v1                     |    -76.639|    11.752|100k       |        149998|         1932|
|dqn     |AsteroidsNoFrameskip-v4        |    782.687|   259.247|10M        |        607962|          134|
|ppo     |BipedalWalkerHardcore-v3       |    122.374|   117.605|100M       |        148036|          105|
|ppo     |BreakoutNoFrameskip-v4         |    398.033|    33.328|10M        |        600418|           60|
|ppo_lstm|MountainCarContinuousNoVel-v0  |     91.469|     1.776|300k       |        149882|         1340|
|ppo_lstm|PendulumNoVel-v1               |   -217.933|   140.094|100k       |        150000|          750|
|qrdqn   |Acrobot-v1                     |    -69.135|     9.967|100k       |        149949|         2138|
|qrdqn   |AsteroidsNoFrameskip-v4        |   2185.303|  1097.172|10M        |        599784|           66|
|sac     |Ant-v3                         |   4615.791|  1354.111|1M         |        149074|          165|
|sac     |AntBulletEnv-v0                |   3073.114|   175.148|1M         |        150000|          150|
|td3     |LunarLanderContinuous-v2       |    207.451|    67.562|300k       |        149488|          337|
|td3     |MountainCarContinuous-v0       |     93.483|     0.075|300k       |        149976|         2275|
|tqc     |HopperBulletEnv-v0             |   2662.373|   206.210|1M         |        149881|          151|
|tqc     |Humanoid-v3                    |   7239.320|  1647.498|2M         |        149508|          165|
|trpo    |Acrobot-v1                     |    -83.114|    18.648|100k       |        149976|         1783|
|trpo    |Ant-v3                         |   4982.301|   663.761|1M         |        149909|          153|

