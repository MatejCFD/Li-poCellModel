# Thevenin lithium-polymer battery cell mathematical model

When it comes to modelling the behavior of a complex electro-chemical dynamical system, three approaches are often utilized. 

1) PBM - physics based modelling
2) ECM - equivalent circuit model
3) Neural network approach

PBM requires us to precisely define and determine relationship between variables of interest and how they evolve with time and/or space. This almost always translates into a set of PDEs. This approach is fairly complex. 

ECM uses and combines discrete, analogue electronic circuit elements in order to as closely as possible replicate the behavior of a said dynamical system. It is obvious that, for the case of modelling a lithium-polymer battery cell, there exist a myriad of models. Some being more complex, others being simpler to implement at the cost of somewhat reduced accuracy. The Thevenin battery cell model corresponds to a middle ground. It is easy to implement, as it consists of only one R-C circuit and one resistor. The goal is thereafter to determine the parameters of the electronic components introduced, in our case one resistance $R_{1}$, one capacitance $C_{1}$ and one additional resistance $R_{0}$. These parameters are usually obtained through various physical testing e.g. charging, discharging and pulsing of the battery cell. 

One shortcoming of the Thevenin model is that it doesn't capture the hysteresis effect. 

## Equations which describe the Thevenin ECM
Looking at the image which depicts the Thevenin ECM, we can derive the following equations of state.
<p align="center">
  <img src="https://user-images.githubusercontent.com/116648366/233140703-d0fdce34-658c-4fab-ad55-62d64a61fcee.jpg">  
</p>

$\dot{z}(t) = - \frac{\eta(t)i(t)}{Q}$

$\frac{di_{R_{1}}(t)}{dt} = - \frac{1}{R_{1}C_{1}}i_{R_{1}}(t) + \frac{1}{R_{1}C_{1}}i(t)$

$v(t) = v(z(t)) - i_{R_{1}}(t) - i(t)R_{0}$

The previous 3 equations can be discretized, and consequently represented in the discrete time domain in the following manner. 

$z(k+1) = z(k) - \frac{\Delta \eta(k) i(k)}{Q}$

$i_{R_{1}}(k+1) = e^{\frac{-1}{R_{1}C_{1}}}i_{R_{1}}(k) + e^{\frac{-1}{R_{1}C_{1}}}i(k)$

$v(k) = v(z(k)) - R_{1}i_{R_{1}}(k) - R_{0}i(k)$

### Variable description

| Symbol | Variable | Unit |
| -------- | ------- | ------- |
| $z(t)$, $z(k)$ | State of charge (SoC) | % |
| $\eta(t)$, $\eta(k)$ | Coulombic efficiency | % |
| $i(t)$, $i(k)$ | Current | [A] |
| $v(t)$, $v(k)$ | Open circuit voltage (OCV) | [V] |
| $v(z(t))$, $v(z(k))$ | Voltage dependent on SoC | [A] |


## Simulink implementation

Simulink implementation can be seen at the beggining of this README file. It contains fairly common blocks, combined with 1D lookup tables which contain the cell voltage and state of charge relationships (obtained from physical testing). 

<p align="center">
  <img src="https://user-images.githubusercontent.com/116648366/233144184-de7085e0-13fe-4237-b95e-ea6bd70cdc64.jpg">  
</p>
