Domestic heat pumps use electricity to transfer heat from the cold outdoors to the warmer indoors. Because they capture heat from the outside air rather than generating it directly, they can be more efficient than traditional electric heating.

In this exercise you will calculate the electrical power and energy consumption of a heat pump over a typical winter day. All hourly values will be stored in 1D NumPy arrays with 24 elements — one per hour.

Make sure to complete and run the code cells in order, as later cells depend on the results of earlier ones. There is a sample solution at the end of the page.

# Step 1: Temperatures

The exterior temperature (K) varies sinusoidally throughout the day, with a minimum of 260K at 6am and a maximum of 280K at 6pm. It can be modelled as:

$$T_{out}(t) = 270 + 10\sin\left(\frac{\pi}{12}(t - 6)\right)$$

where $t$ is the hour of the day (0–23). The interior temperature is held constant at 293K.

In the cell below, create arrays `t_out`{.python} and `t_in`{.python} covering all 24 hours. Use `np.arange`{.python} to generate the time values, `np.sin`{.python} for the exterior temperature, and `np.full`{.python} for the interior temperature.

```py-cell
import numpy as np
import math

# Exterior temperature (K)
t_out =

# Interior temperature (K)
t_in =

print("Outdoor Temperature (K):", t_out)  # Should be 24 values. First should be 260
print("Indoor Temperature (K):", t_in)    # Should be 24 values, all 293
```

# Step 2: Electrical power

When approximations are made, the electrical power required to run the heat pump at each hour is:

$$P_{elec} = UA(T_{in} - T_{out})\left(1 - \frac{T_{out}}{T_{in}}\right)$$

where:

- $U$ = 2 W/m²K (overall heat transfer coefficient)
- $A$ = 100 m² (surface area of the house)
- $T_{in}$, $T_{out}$ are the interior and exterior temperatures (K)

```py-cell
U = 2    # Heat transfer coefficient (W/m²K)
A = 100  # Heat transfer surface area (m²)

# Electrical power (W)
p_elec =

print("Electrical Power (W) each hour:", p_elec)  # Should be 24 values. First should be ~743
```

# Step 3: Electrical energy

To convert power (W) to energy (J) for each 1-hour period, multiply by the number of seconds in an hour:

$$E_{elec} = 3600 \, P_{elec}$$

```py-cell
# Energy used in each of the 24 1-hour periods (J)
e_elec =

print("Electrical Energy (J) each hour:", e_elec)  # Should be 24 values. First should be ~2,676,040
```

# Step 4: Total cost

Electricity costs 25.73 p/kWh, which is equivalent to $7.14 \times 10^{-8}$ £/J. Calculate the total cost of electricity used over the full 24-hour period.

```py-cell
# Total 24-hour cost (£)
total_cost =

print("Total 24-hour cost (£):", total_cost)  # Should be a single value of ~£2.44
```

# Sample Solution

> [!HIDDEN]
>
> # Step 1: Temperatures
> 
> `np.arange(0, 24)`{.python} generates the 24 hourly time values. The exterior temperature formula uses `np.sin`{.python}, which applies element-wise to the time array. `np.full(24, 293)`{.python} creates a constant array of 293 K for the interior temperature.
> 
> ```py-cell
> import numpy as np
> import math
> t = np.arange(0, 24)
>
> t_out = 270 + 10 * np.sin(math.pi / 12 * (t - 6))
> t_in = np.full(24, 293)
>
> print("Time (hours):", t)
> print("Outdoor Temperature (K):", t_out)
> print("Indoor Temperature (K):", t_in)
> ```
> 
> ## Step 2: Electrical power
> 
> The electrical power formula involves element-wise arithmetic between two arrays (`t_in`{.python} and `t_out`{.python}) and scalar multiplication. Because all operations are element-wise, the result is a 24-element array.
> 
> ```py-cell
> U = 2    # Heat transfer coefficient (W/m²K)
> A = 100  # Heat transfer surface area (m²)
> 
> p_elec = U * A * (t_in - t_out) * (1 - t_out / t_in)
>
> print("Electrical Power (W) each hour:", p_elec)
> ```
> 
> ## Step 3: Electrical energy
> 
> Multiplying the power array by the scalar 3600 uses broadcasting to apply the conversion to every element.
> 
> ```py-cell
> e_elec = 3600 * p_elec
>
> print("Electrical Energy (J) each hour:", e_elec)
> ```
> 
> ## Step 4: Total cost
> 
> Summing the energy array with `.sum()`{.python} gives the total energy over 24 hours. Multiplying by the cost per joule gives the total cost as a single scalar value.
> 
> ```py-cell
> total_cost = e_elec.sum() * 7.14e-8
>
> print("Total 24-hour cost (£):", total_cost)
