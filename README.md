**Travis**
:-------:
[![Build Status](https://travis-ci.com/johncwok/SpectralEnvelope.jl.svg?branch=master)](https://travis-ci.com/johncwok/SpectralEnvelope.jl)


# Spectral Envelope
A fast and easy to use julia implementation of the spectral envelope method, used in categorical data analysis.

The **spectral envelope** is a tool to study cyclic behaviors in categorical data. It is more efficient than the traditional approach of attributing a different number to each category before computing the power-spectral density.<br/>

For each frequency in the spectrum, the **spectral envelope** finds an optimal real-numbered mapping that maximizes the power-spectral density at this point. Hence the name: no matter what mapping is choosen for each category, the power-spectral density will always be bounded by the spectral envelope.

### Installation and import :
```Julia
# installing the module
Using Pkg
Pkg.clone(“https://github.com/johncwok/SpectralEnvelope.jl.git”)
# importing the module
Using SpectralEnvelope
```
## Usage :
```spectral_envelope 
spectral_envelope(ts; m = 3)

  Input
    -ts : array containing the time series to be analysed.
    -m : the smoothing parameter. It’s value corresponds to how many neighboring points 
        are to be taken in account in the smoothing. Defaults to 3.
  Returns 
    -freq : An array containing the frequency of the power-spectrum (or spectral envelope)
    -se : the value of the spectral envelope for each frequency value.
    -eigvec : Array containing the optimal real-valued mapping for each frequency point.
    -categories : the categories which are present in the data.
```
To use the spectral envelope, call the function ```spectral_envelope```, you can then easily plot the results and extract the mapping for a given frequency.
```Julia
f, se, mappings, categories = spectral_envelope(data; m = 4)
#plotting the results
Using Plots
plot(f, se)
```
<img src=https://user-images.githubusercontent.com/34754896/81937423-e5031f00-95f3-11ea-986d-bb5a3689639f.png width = "600">

To get the optimal mappings for a given frequency more easily, you can use the ```get_mapping(goal,f,se,mappings,categories)``` (you can use spalting for a more concise call) :
```Julia
f,se,mappings,categories =spectral_envelope(data; m =0)
get_mapping(goal,f,se,mappings,categories)
# using spalting : get_mapping(goal, spectral_envelope(data;m=0)…)
>> position of peak: 0.3338  strength of peak: 0.8067 
 ["A : -0.3959646304003992", "G : -0.3930054480816879", "T : 0.8282155690720968", "C : 0.1326439759143416"]
```
The function scans the vincinity of the provided goal frequency and returns the mappins for the found maxima. It also prints the positions and intensity of the peak so that you may control that you actually identified the desired peak and not a nearby sub-peak.
In this example, we see that at the frequency ~0.33, the codons A and G have an equivalent mapping and so they have the same function (from the point of view of the time-series).

### Remark :
The spectral envelope method was defined by David S. Stoffer in *DAVID S. STOFFER, DAVID E. TYLER, ANDREW J. MCDOUGALL, Spectral analysis for categorical time series: Scaling and the spectral envelope*.\
He also provided an R implementation in his book *Time series analysis and its applications*, however, its is not very user-friendly and extracting the optimal mappings with it is really not straightforward.
