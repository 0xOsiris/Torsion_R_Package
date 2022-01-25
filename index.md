## Torsion of Parametric Curve R Package

### Installation
```markdown
> library(devtools)
> install_github("LeytonTaylor/TorsionR");
> library(TorsionR)
```

### Usage
```markdown
@param alpha A vector of 3 expressions defining the parametric curve
@param t Evaluation parameter
@return Scalar value Torsion of alpha at time t
@details alpha must be a vector of 3 parametric expressions with param 't'
@examples 
> tAlpha <- torsion(c(expression(1 + t + cos(t)*sin(t),1 + 0.1*sin(t) + cos(t) * 0.1,0.1*sin(t))), 2)
```

### Components

## Torsion Calculation

```markdown

torsion <- function(alpha,t){

  alphaPrimeMatrix <- d_util$alphaDerivativeMat(alpha,t)
  alphaP <- c(eval(alphaPrimeMatrix[2,1]),eval(alphaPrimeMatrix[2,2]),eval(alphaPrimeMatrix[2,3]))
  alphaDP <- c(eval(alphaPrimeMatrix[3,1]),eval(alphaPrimeMatrix[3,2]),eval(alphaPrimeMatrix[3,3]))
  alphaTP <- c(eval(alphaPrimeMatrix[4,1]),eval(alphaPrimeMatrix[4,2]),eval(alphaPrimeMatrix[4,3]))
  normCross <-xprod_util$xprod(alphaP,alphaDP)
  normAD <-norm(as.matrix(normCross),"F")
  num <- normCross * alphaTP
  numFinal <- sum(num)
  tAlpha = numFinal /(normAD^2)

  return(tAlpha)
  
 }


```
## Cross product calculation
```markdown

#' @export
xprod_util = new.env()
xprod_util$xprod <- function(...) {
  args <- list(...)

  if (length(args) == 0) {
    stop("No data supplied")
  }
  len <- unique(sapply(args, FUN=length))
  if (length(len) > 1) {
    stop("All vectors must be the same length")
  }
  if (len != length(args) + 1) {
    stop("Must supply N-1 vectors of length N")
  }

  m <- do.call(rbind, args)
  sapply(seq(len),
         FUN=function(i) {
           det(m[,-i,drop=FALSE]) * (-1)^(i+1)
         })
}


```

## Derivative Matrix
```markdown
#' @export
d_util = new.env()
d_util$alphaDerivativeMat <- function(alpha, t){

  alphaP <- c(D(alpha[1],'t'), D(alpha[2],'t'),D(alpha[3],'t'))

  alphaDP <- c(dd_util$DD(alpha[1],'t',2),dd_util$DD(alpha[2],'t',2),dd_util$DD(alpha[3],'t',2))
  alphaTP <- c(dd_util$DD(alpha[1],'t',3),dd_util$DD(alpha[2],'t',3),dd_util$DD(alpha[3],'t',3))

  alphaPrimeMatrix <-matrix(
    c(alpha,alphaP,alphaDP, alphaTP),
    nrow=4,
    ncol=3,
    byrow=TRUE

  )

  dimnames(alphaPrimeMatrix)=list(c("alpha","alphaP","alphaDP", "alphaTP"),c("X","Y","Z"))

  return(alphaPrimeMatrix)

}


```

## High Order Derivative Utility function
```markdown
#' @export
dd_util = new.env()
dd_util$DD <- function(expr, name, order=1) {
  if(order < 1) stop("'order' must be >= 1")
  if(order == 1)
    return(D(expr, name))
  else dd_util$DD(D(expr, name), name, order - 1)}

```



