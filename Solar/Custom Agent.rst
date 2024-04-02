============
Custom Agent
============

This agent takes information from a custom environment and based off the XXX. The agent will compare the values and will tell battery whether to charge(+) or discharge(-) at varying degrees based on severity of values calculated. It only has one action of charge but depending on the sign of its value will it charge or discharge. In special cases it will look at the capcitance value. 

Current Version is to make reward zero from a negative reward. It works when there is a visible notice to the amount of iterations it takes to get a reward value greater than or equal to 0. 
