# R Utility to Calculate Torsion of a Parametric Curve 

# Installation Instructions
#### > library(devtools)
#### > install_github("LeytonTaylor/TorsionR");
#### > library(TorsionR);

# Usage:
#### @param alpha A vector of 3 expressions defining the parametric curve
#### @param t Evaluation parameter
#### @return Scalar value Torsion of alpha at time t
#### @details alpha must be a vector of 3 parametric expressions with param 't'
#### @examples 
#### > tAlpha <- torsion(c(expression(1 + t + cos(t)*sin(t),1 + 0.1*sin(t) + cos(t) * 0.1,0.1*sin(t))), 2)
