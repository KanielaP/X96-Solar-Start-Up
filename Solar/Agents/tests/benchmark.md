
## Performance of Trained Agents

This is not the final performance, but rather our starting results. 
It uses our custom environments and uses rlzoo agents. 

*NOTE: this is not a quantitative benchmark as it corresponds to only one run.
This Benchmark is meant to record initial results to be able to edit and
optimize future tests*


"M" stands for Million (1e6)

|  algo  |             env_id              |mean_reward|std_reward|n_timesteps|eval_timesteps|eval_episodes|eps_timesteps|
|--------|---------------------------------|----------:|---------:|-----------|-------------:|------------:|------------:|
|a2c     |DemandReponseSimpleNoExport-v0   |     12.833|  -439.956|2M         |         25000|           80|         8760|
|a2c     |DemandResponseSimpleWithExport-v0|    455.277|    35.122|2M         |         25000|           80|         8760|
|ppo     |DemandReponseSimpleNoExport-v0   |     12.613|  -248.139|2M         |         25000|           80|         8760|
|ppo     |DemandResponseSimpleWithExport-v0|    484.125|    33.395|2M         |         25000|           80|         8760|
|ppo_lstm|DemandReponseSimpleNoExport-v0   |   -324.236|    26.139|2M         |         25000|           80|         8760|
|ppo_lstm|DemandResponseSimpleWithExport-v0|    482.195|    34.074|2M         |         25000|           80|         8760|
|tcq     |DemandReponseSimpleNoExport-v0   |    -90.439|     9.927|1M         |         25000|           40|         8760|
|tcq     |DemandResponseSimpleWithExport-v0|        N/A|       N/A|N/A        |           N/A|          N/A|         8760|
|trpo    |DemandReponseSimpleNoExport-v0   |    -83.114|    12.576|2M         |         25000|           80|         8760|
|trpo    |DemandResponseSimpleWithExport-v0|    486.459|    34.240|2M         |         25000|           80|         8760|

