
# Graph my own model

**Using $(0,0)$ approximately as my left end point, the domain is $[0,280]$.**

Using the rule:
$$
y = A\cdot \ln(x+0.1) + B\cdot e^{(x+3)} + Cx + Dx^2 + E + U\cdot \sqrt{x}
$$

![Image](https://raw.githubusercontent.com/icaijy/md-image/main/Untitled.png)

[Desmos](https://www.desmos.com/calculator/ioxoio3elf) gives the regression function:
$$g(x)= -3.1401734205\cdot10^{-146}\cdot e^{(x+3)}+ 2.82976095867\ln(x+0.1)- 0.459895034255x+ 0.000320857875479x^{2}+ 6.54535870733+ 6.2354020108\sqrt{x}$$

The exponential term has coefficient $-3.14\times10^{-146}$, which is numerically negligible over the domain $[0,280]$. Although it appears in the regression output, it does not contribute meaningfully to the curve shape and can be removed for simplification.

---

## Function selection strategy (how the model was constructed)

This model is not chosen randomly; it is built using a **layered function design approach**, combining a polynomial backbone with several nonlinear basis functions to capture different behaviours of the data.

### 1. Polynomial backbone (global trend control)

The terms:
$$Cx + Dx^2 + E$$

form the core structure of the model.

- $Cx$ captures the **overall linear trend**
  - controls global increasing/decreasing direction
  - corrects first-order bias in data

- $Dx^2$ captures **global curvature**
  - allows acceleration or deceleration over long time
  - prevents linear underfitting

- $E$ shifts the entire curve vertically
  - sets baseline level of the function

Why polynomial terms are essential:
- they provide global flexibility
- they are stable over the entire domain
- they approximate smooth large-scale behaviour efficiently

However:
- polynomials alone tend to overshoot or behave unrealistically at boundaries
- they cannot model saturation or diminishing returns well

---

### 2. Logarithmic term (early-stage growth / saturation effect)

$$A\ln(x+0.1)$$

The logarithmic function is included to model:
- rapid initial growth
- slowing rate of increase over time
- early saturation behaviour

Why logarithm is useful:
- steep slope near small $x$
- gradually decreasing derivative:
  $$\frac{d}{dx}\ln(x)=\frac{1}{x}$$
- naturally produces diminishing returns

The shift $(x+0.1)$ ensures:
- domain stability at $x=0$
- avoids singularity at $\ln(0)$

---

### 3. Square root term (mid-range curvature shaping)

$$U\sqrt{x}$$

This term is used to:
- add smooth nonlinear growth in mid-range
- correct mismatch between logarithmic and polynomial behaviour
- introduce gentle concavity without instability

Why $\sqrt{x}$ is chosen:
- grows faster than $\ln x$ but slower than $x$
- provides intermediate scaling behaviour
- has stable derivative:
  $$\frac{d}{dx}\sqrt{x}=\frac{1}{2\sqrt{x}}$$

This makes it ideal for:
- smoothing transitions between early and mid stages of the curve

---

### 4. Exponential term (attempted long-range correction)

$$B e^{(x+3)}$$

This term is included in the regression basis but effectively rejected by the fit (coefficient ≈ $10^{-146}$).

Its theoretical role would be:
- modelling explosive growth or decay
- capturing extreme curvature at boundaries

However in this dataset:
- exponential growth is not supported
- regression naturally suppresses it to zero

This is why it can be safely ignored.

---

## Final simplified model

After removing numerically insignificant terms and stabilising the left endpoint, we define:

$$f: [0,280]\to\mathbb{R},\quad f(x)= 2.82976095867\ln(x+0.1)- 0.459895034255x+ 0.000320857875479x^{2}+ 6.5157654001701+ 6.2354020108\sqrt{x}$$

## Overall modelling interpretation

This function is a **hybrid empirical regression model**, constructed using:

- polynomial terms → global structure
- logarithmic term → early rapid change + saturation
- square root term → smooth mid-range curvature
- exponential term → automatically eliminated by regression

This reflects a general modelling strategy:

> start with a flexible polynomial backbone, then enrich it with nonlinear basis functions that represent different growth regimes, and finally let regression eliminate unnecessary components.

In this sense, the final model is not “one function”, but a **superposition of interpretable functional behaviours**, each responsible for a different scale of the curve.


# Wood's Model
$$Y_t=at^be^{-ct}$$

where:  
- $Y_t$ is milk yield  
- $t$ is days after calving  
- $a,b,c$ control the shape of the curve  

This model is widely used in dairy science to represent lactation curves. It combines a power growth term ($t^b$) with an exponential decay term ($e^{-ct}$), producing a characteristic unimodal curve: an initial increase, a peak, and a gradual decline. This shape closely matches biological lactation behaviour, where milk production rises after calving, reaches a maximum, and then decreases due to physiological limitations.

# Parameter $a$

Parameter $a$ is a vertical dilation factor.  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522092622264.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522092552157.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522092711394.png)

Increasing $a$:
- increases the value of $Y_t$ at every time point $t$
- increases the peak milk yield proportionally
- does not change the shape of the curve
- does not change the timing of the peak
- does not change the curvature or growth/decay rates

Mathematically:
- $Y_t = a \cdot t^b e^{-ct}$
- $a$ is a linear scaling constant
- therefore:
  - $Y_t \propto a$
  - $\frac{\partial Y_t}{\partial a} = t^b e^{-ct}$

This means $a$ affects only magnitude, not dynamics.

Possible values:
- $a > 0$ (required because negative milk production is biologically meaningless)
- $a = 0$ implies no milk production at all
- typical range depends on breed and environment:
  - low-yield cows → small $a$
  - high-yield dairy breeds → large $a$

Biological interpretation:
- overall productivity level of the cow
- influenced by:
  - genetics (breed potential)
  - nutrition quality
  - farm management
  - health conditions
  - metabolic capacity

From the graphs:
- all curves are identical in shape
- peak occurs at exactly the same time
- only vertical stretching is observed
- entire curve is uniformly scaled

Conclusion:
$a$ represents the **baseline production capacity (scale factor)** of milk yield.

---

# Parameter $b$

Parameter $b$ controls the initial growth behaviour of the curve.  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522093858686.png)  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522093933171.png)  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522093952462.png)

The term $t^b$ dominates early-stage behaviour, especially when $t$ is small. Therefore, $b$ strongly influences how quickly the curve rises after calving.

Increasing $b$:
- significantly increases early growth rate
- makes the curve more convex in the rising phase
- delays the peak time
- increases peak milk yield
- increases the area under the curve in early lactation
- strengthens asymmetry of the curve (longer rise, sharper peak)

Decreasing $b$:
- flattens early growth
- reduces acceleration of milk production
- causes earlier peak
- reduces peak height
- may make curve closer to exponential decay shape when very small

Mathematically:
- $b$ controls the exponent in $t^b$
- for small $t$, changes in $b$ have strong impact because:
  - $t^b = e^{b\ln t}$
  - sensitivity depends on $\ln t$

Possible values:
- $b > 0$ (required for biologically realistic growth phase)
- $b = 0$ gives:
  $$Y_t = a e^{-ct}$$
  which removes lactation rise entirely (unrealistic in biology)
- very large $b$:
  - extremely slow initial increase followed by rapid surge
  - delayed but sharp peak
- very small $b$ (close to 0):
  - almost no growth phase

Biological interpretation:
- speed of lactation build-up after calving
- influenced by:
  - hormonal activation of milk production
  - recovery after parturition
  - feed intake adaptation
  - energy balance transition

From graphs:
- increasing $b$ shifts peak significantly to the right
- early portion becomes steeper
- curve becomes more skewed
- growth phase dominates longer before decline begins

Importantly:
- declining phase is almost unchanged
- because $b$ does not appear in exponential decay term $e^{-ct}$

Conclusion:
$b$ represents the **rate of biological activation and early lactation growth strength**.

---

# Parameter $c$

Parameter $c$ controls the exponential decay after peak production.  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094114424.png)  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094132021.png)  
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094222802.png)

The term $e^{-ct}$ dominates long-term behaviour. Therefore, $c$ governs how quickly milk production declines after reaching peak.

Increasing $c$:
- increases decay rate exponentially
- shortens duration of lactation
- reduces persistence of milk production
- causes earlier peak occurrence
- steepens post-peak decline
- reduces right tail of the curve significantly

Decreasing $c$:
- slows exponential decay
- extends lactation duration
- produces a longer “tail”
- increases persistence of milk yield
- delays steep decline phase

Mathematically:
- $\frac{\partial Y_t}{\partial c} = -a t^{b+1} e^{-ct}$
- sensitivity increases with time $t$
- therefore $c$ has stronger influence in later stages

Possible values:
- $c > 0$ (required to ensure eventual decline)
- $c \to 0^+$:
  $$Y_t \approx a t^b$$
  meaning no decay → unrealistic sustained growth
- large $c$:
  - rapid collapse after peak
  - very short productive period

Biological interpretation:
- persistence of milk production after peak
- influenced by:
  - metabolic stress
  - energy deficit after peak lactation
  - health deterioration rate
  - efficiency of nutrient conversion
  - udder tissue regulation

From graphs:
- increasing $c$ compresses curve strongly on right side
- peak shifts left
- decline becomes much sharper
- early growth remains largely unchanged

Reason:
- $c$ only affects exponential decay term, not $t^b$

Conclusion:
$c$ represents the **rate of biological exhaustion and post-peak decline strength**.

---

# Key Features of the Curve

The turning point occurs at:

$$t=\frac{b}{c}$$


This result shows a fundamental balance between growth and decay mechanisms:

- $b$ increases growth dominance → delays peak
- $c$ increases decay dominance → advances peak
- $a$ does not affect timing → only magnitude

Derivation insight:
- peak occurs when $\frac{dY}{dt} = 0$
- differentiating:
  $$
  Y_t = a t^b e^{-ct}
  $$
  leads to:
  $$
  \frac{dY}{dt} = a t^{b-1} e^{-ct}(b - ct)
  $$
- setting derivative to zero:
  $$
  b - ct = 0
  $$
  so:
  $$
  t = \frac{b}{c}
  $$
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094454866.png)
This clearly shows:
- peak location depends only on ratio $b/c$
- not on $a$

Structural interpretation:
- $b$ controls “build-up speed”
- $c$ controls “decay pressure”
- equilibrium determines peak timing

---

# Three Stages of the Curve

## 1. Early lactation (growth-dominated phase)
- mainly controlled by $b$
- $t^b$ dominates behaviour
- milk yield increases rapidly after calving
- biological explanation:
  - activation of lactation system
  - increasing feed intake
  - metabolic adjustment period

## 2. Peak production (transition equilibrium)
- controlled by interaction between $b$ and $c$
- occurs when growth rate equals decay rate
- mathematically:
  - derivative equals zero
- biologically:
  - maximum efficiency state
  - balance between energy input and physiological limits

## 3. Late lactation (decay-dominated phase)
- mainly controlled by $c$
- exponential decay dominates
- production declines continuously
- biological explanation:
  - metabolic fatigue
  - reduced endocrine stimulation
  - energy reallocation away from milk production

---

# Role of Parameter $a$

Across all stages:
- $a$ only scales output magnitude
- does not affect:
  - timing of peak
  - shape of curve
  - growth rate
  - decay rate
  - biological dynamics

It acts as a pure amplitude parameter:
- $Y_t \propto a$
- all derivatives scale linearly with $a$

Therefore:
- $a$ = scale
- $b$ = growth dynamics
- $c$ = decay dynamics



# Another model: Wilmink model

$$Y_t = a + b e^{-kt} + ct$$

## Key features
- Linear term $ct$ gives long‑term decline.
- Exponential $be^{-kt}$ captures the early rise and peak.
- $k>0$ controls how fast the exponential decays; larger $k$ gives sharper peak.
- $a$ is the asymptotic baseline yield as $t\to\infty$.

## Comparison with Wood’s model

| Feature | Wood’s model | Wilmink model |
|---------|--------------|----------------|
| Biological interpretation | $a,b,c$ directly relate to level, ramp‑up, persistency | $a,b,k,c$ – less direct biological meaning |
| Mathematical form | Power × exponential | Exponential + linear |
| Peak time | $t_{\text{peak}}=\frac{b}{c}$ | $t_{\text{peak}}=\frac{1}{k}\ln\left(\frac{bk}{c}\right)$ (if $c>0$) |
| Asymptote | $Y_t\to 0$ as $t\to\infty$ | $Y_t\to a+ct$ (linear decay, never zero) |
| Flexibility | Good for typical unimodal shapes | More flexible for data with non‑zero final yield |
| Computational fit | Nonlinear, sometimes convergence issues | Often easier to fit (linear in $a,b$ if $k$ fixed) |

**Conclusion**: Wood’s model has clearer biological meaning; Wilmink is more flexible for empirical fitting, especially when the lactation tail is not near zero.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyNzc1MDY4N119
-->