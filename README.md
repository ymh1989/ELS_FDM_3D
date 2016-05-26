##Dynamic Linked Library for pricing 3-asset ELS by Finite difference method##

###Introduction###
In this repo, I'll introduce pricing module for 3-asset stepdown Equity-linked securities (ELS). ELS are auto-callable options whose return on investment is dependent upon the path of the underlying asset, especillay securities. Nowadays, ELS are composed of a few stock index such as the HSCEI(Hong Kong) and EUROSTOXX50(Europe)[1]. In South Korea, ELS have been famous financial product after its release. Recently, most ELS have more than three underlying asset to increase the coupon rate.

Despite its popularity in South Korea, the curse of dimensionality which is lead to a tremendous computational cost is main difficulty to price and calculate hedging parameters(greeks). There are two main method, Monte Carlo simulation(MCS) and Finite diffrence method(FDM), for calculating price and greeks. First, although MCS has a advantage of implementing for complicated its payoff, unstable result of greeks and compuational cost are main drawbacks. Next, FDM generates stable solution over the its computational domain. However, complicated payoff such as discountinous payoff and path-dependancy control are main challenge for stable solution. See [2] for more detail on this point.

The main purpose of this repo is to treat the a few problem as follow.
- Curse of dimensionality
- Discontinous payoff

###Numerical Method###
First, in order to cure the curse of dimensionality, I introduce the non-uniform mesh for FDM. One of existing method for non-uniform is to concentrate the mesh in important area one by one. In this repo, I present a different method for automatic non-uniform mesh construciton by using `asinh (inverse hyperbollic sine)`[3]. This method does not only to concentrate the points in important areas, but also to lie midway between two grids points for increases the accuracy of the FDM[1] at the same time. Second, I introduce the smoothing method to treat numerical difficulty of discountinous payoff. Several methods are already introduced in `smoothing_payoff_FDM`[4] repo. In addition, I apply the `sub-time step` at observation dates of ELS. At first, I consider the Rannacher timestepping[5-6] that is combination of fully implicit and Crank-Nicolson(CN) method with sub-time steps. In spite of merit, it is hard for Ranncher scheme to employ in multidimensional cases due to the property of CN method. Therefore, I apply some trick for time stepping which supplements sub-time steps at observation dates. By so doing, the solutions around each observation date are more accurate.

As a method for FDM, I employ a Operator Spliting Method(OSM). Alternative Direction Implicit(ADI) has a advantage in unconditionally stable solution and second order in time and space. However, ADI with tiny time step has a huge weakness about oscillation of solution as well as greeks[7]. That is why I apply the OSM.

###Environment###
- CPU : Intel(R) Core(TM) i5-6400 @ 2.7GHZ
- RAM : DDR3L 16GB PC3-12800
- Windows 10 Enterprise 64bit
- Microsoft Visual Studio Community 2015, Microsoft Office 2016
** Lower version of MS Office will cause a error, because I complied in the latest version of VS. If you have a trouble with this issue, please contact me.

###Implementation###
In order to the easy accessibility of program, I make a Dynamic Linked Library(DLL) file. By using dll, you can easily calculation about 3-asset ELS with `Microsoft Excel`.

###Results###


###Future work###
- Better smoothing method for discontinous payoff
- Examine the stability of greeks.
- Parallelization for better performance

###Reference###

\[1\] Yoo, Minhyun, et al. "A COMPARISON STUDY OF EXPLICIT AND IMPLICIT NUMERICAL METHODS FOR THE EQUITY-LINKED SECURITIES." 호남수학학술지 37.4 (2015): 441-455.

\[2\] Jeong, Darae., Wee, In-Suk, and Kim, Junseok. "An operator splitting method for pricing the ELS option." Journal of Korean Society for Industrial and Applied Mathematics 14 (2010): 175-187.

\[3\] Tavella, Domingo, and Curt Randall. Pricing financial instruments: The finite difference method. Vol. 13. John Wiley & Sons, 2000.

\[4\] Venemalm, Johan. "State Equidistant and Time Non-Equidistant Valuation of American Call Options on Stocks With Known Dividends." (2014).

\[5\] Rannacher, Rolf. "Finite element solution of diffusion problems with irregular data." Numerische Mathematik 43.2 (1984): 309-327.

\[6\] Giles, Michael B., and Rebecca Carter. "Convergence analysis of Crank-Nicolson and Rannacher time-marching." (2005).

\[7\] Jeong, Darae, and Junseok Kim. "A comparison study of ADI and operator splitting methods on option pricing models." Journal of Computational and Applied Mathematics 247 (2013): 162-171.