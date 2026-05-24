
# 1. Lactation Cycle Phase Description & Hybrid Empirical Regression Model

### Biological Context of the Lactation Cycle
The lactation cycle of a dairy cow represents a dynamic physiological process that is traditionally partitionable into four distinct sequential biological phases:
1. **Early Lactation (Days 1–50):** Commences immediately postpartum. It is characterized by a rapid, exponential acceleration in daily milk production, culminating in the peak milk yield. During this stage, the cow experience a negative energy balance as milk output outpaces dry matter feed intake.
2. **Mid Lactation (Days 51–140):** A period of metabolic stabilization. Milk yield begins a highly predictable, gradual decline from its peak, while the cow's feed intake reaches its maximum capacity, bringing the animal back into a positive energy balance.
3. **Late Lactation (Days 141–305):** Characterized by a continuous, steady decline in milk volume. This phase is driven by the progressive regression and natural apoptosis of mammary secretory epithelial cells.
4. **Dry Period (Days 306–365):** A critical non-lactating phase lasting approximately 60 days prior to the next calving event. This period is biologically essential to allow for mammary tissue rejuvenation, health recovery, and fetal development.

### Original Empirical Model Construction
Using $(0,0)$ as the definitive physical left endpoint constraint at calving, the operational domain for this empirical investigation is defined as $[0, 280]$. The initial highly flexible, multi-component basis function rule selected is:
$$y = A\cdot \ln(x+0.1) + B\cdot e^{(x+3)} + Cx + Dx^2 + E + U\cdot \sqrt{x}$$

![Image](https://raw.githubusercontent.com/icaijy/md-image/main/Untitled.png)

Executing a non-linear least-squares regression optimization via [Desmos](https://www.desmos.com/calculator/ioxoio3elf) maps the empirical dataset to the following heavily parameterized function:
$$g(x)= -3.1401734205\cdot10^{-146}\cdot e^{(x+3)}+ 2.82976095867\ln(x+0.1)- 0.459895034255x+ 0.000320857875479x^{2}+ 6.54535870733+ 6.2354020108\sqrt{x}$$

### Model Simplification & Boundary Condition Optimization
The coefficient of the exponential term is mathematically negligible ($-3.14 \times 10^{-146}$). Even at the extreme upper boundary of the domain ($x = 280$), the value of this term evaluates to approximately $-7.7 \times 10^{-24}$, which yields absolutely zero geometric or numeric impact on the curve profile. To maintain structural parsimony and prevent numerical instability, this term is discarded.

Furthermore, a critical physical constraint dictates that at the exact moment of calving ($x = 0$), the milk yield must be precisely zero ($f(0) = 0$). Evaluating the remaining basis functions at $x = 0$ yields:
$$2.82976095867\ln(0.1) - 0.459895034255(0) + 0.000320857875479(0)^2 + 6.2354020108\sqrt{0} = 2.82976095867 \times (-2.30258509299) \approx -6.51576540017$$

The raw statistical regression baseline constant ($E = 6.54535870733$) introduces a slight positive offset at calving due to global data pooling. To strictly enforce the $(0,0)$ endpoint constraint without distorting the curve's dynamics, the global constant $E$ is analytically adjusted to counteract the logarithmic initial offset:
$$E = -A\ln(0.1) = -(2.82976095867 \times \ln(0.1)) \approx 6.5157654001701$$

The finalized, structurally optimized, and boundary-stabilized hybrid mathematical model is defined as:
$$f: [0,280]\to\mathbb{R},\quad f(x)= 2.82976095867\ln(x+0.1)- 0.459895034255x+ 0.000320857875479x^{2}+ 6.5157654001701+ 6.2354020108\sqrt{x}$$

Where the independent variable $x$ explicitly denotes the time elapsed post-calving (Days in Milk, DIM) measured in **days**, and the dependent functional output $f(x)$ quantifies the absolute daily milk volume produced, measured in **Litres per day (L/day)**.

### Analytical Differentiation and Peak Coordinates Extraction
To rigorously verify the stationary turning point governing maximum production capacity within this original formulation, the optimized hybrid function is differentiated using standard calculus rules (the chain rule and power rule):
$$f'(x) = \frac{2.82976095867}{x+0.1} - 0.459895034255 + 0.000641715750958x + \frac{3.1177010054}{\sqrt{x}}$$

Setting the global first derivative to zero ($f'(x) = 0$) to locate the mathematical global maximum within the domain, numerical approximation routines performed via the technological solver isolate the precise empirical peak turning point at the following coordinates:
$$\text{Peak Location} \approx (69.25, 40.09)$$
This indicates that the modeled biological subject reaches its absolute peak performance of $40.09\text{ L/day}$ at exactly $69.25\text{ days}$ postpartum, establishing highly realistic alignment with field observations.

### Layered Function Selection Strategy
Rather than relying on an arbitrary polynomial fit, this model utilizes a **layered basis function design approach**, superimposing a global polynomial backbone with specialized non-linear functions to handle distinct biological growth regimes:

1. **Polynomial Backbone ($Cx + Dx^2 + E$):** Establishes long-range, macro-structural control over the entire domain. The linear coefficient $Cx$ captures the overall downward trajectory post-peak, the quadratic term $Dx^2$ provides stable curvature to prevent overshooting at the boundaries, and $E$ anchors the theoretical baseline.
2. **Logarithmic Term ($A\ln(x+0.1)$):** Characterized by an extremely steep initial derivative ($\frac{d}{dx}\ln(x)=\frac{1}{x}$), this term effectively models the rapid, explosive acceleration of milk yield during early lactation. Shifting the argument by $+0.1$ satisfies continuity requirements and prevents a mathematical singularity at $x=0$.
3. **Square Root Term ($U\sqrt{x}$):** Functions as an intermediate scaling operator ($\frac{d}{dx}\sqrt{x}=\frac{1}{2\sqrt{x}}$). It bridges the transition between early logarithmic growth and late-stage linear-quadratic decay, smoothing out mid-range curvature without causing long-term instability.

---

# 2. Comprehensive Analysis of Wood's Incomplete Gamma Model

Wood’s incomplete gamma model represents the standard mathematical benchmark for representing lactation profiles through a multiplicative power-exponential structure:
$$Y_t=at^be^{-ct}$$
where $Y_t$ defines the daily milk yield in Litres at $t$ days post-calving.

### Parameter Sensitivity and Biological Interpretation
* **Parameter $a$ (Scale Factor):** Functions as a pure vertical dilation parameter ($Y_t \propto a$). Altering $a$ uniformly stretches or compresses the entire curve along the vertical axis without shifting the peak timing or modifying the underlying geometric dynamics. Biologically, it represents the **baseline production capacity** of the cow, dictated by genetic merit, parity, and baseline herd management.

![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522092622264.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522092552157.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522092711394.png)

* **Parameter $b$ (Inclination Constant):** Governs the power function growth ($t^b = e^{b\ln t}$), dominating the early-stage postpartum rise ($t \to 0$). Increasing $b$ enhances the initial acceleration rate, delays peak timing, and increases the overall skewness of the curve. Biologically, it signifies the **velocity and strength of mammary secretory cell activation** as the cow scales up production.

![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522093858686.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522093933171.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522093952462.png)

* **Parameter $c$ (Decline Constant):** Governs the exponential decay ($e^{-ct}$), establishing total dominance across the late-lactation tail ($t \to \infty$). Higher values of $c$ severely accelerate the post-peak drop-off rate, truncating lactation duration. Biologically, it represents the **rate of mammary cell apoptosis** and metabolic exhaustion, serving as an inverse proxy for lactation persistency.

![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094114424.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094132021.png) ![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094222802.png)

### Mathematical Derivation of Crucial Curve Features
The turning point corresponding to peak production is determined analytically by isolating the stationary point where the first derivative equals zero ($\frac{dY}{dt} = 0$). Differentiating Wood's model via the product rule yields:
$$\frac{dY}{dt} = \frac{d}{dt}(at^b) * e^{-ct} + at^b * \frac{d}{dt}(e^{-ct})$$
$$\frac{dY}{dt} = abt^{b-1}e^{-ct} - act^be^{-ct}$$
$$\frac{dY}{dt} = at^{b-1}e^{-ct}(b - ct)$$

Setting $\frac{dY}{dt} = 0$ to locate the critical value $t_{\text{peak}}$ (under the real-world conditions that $a > 0$, $t > 0$, and $e^{-ct} \neq 0$):
$$b - ct_{\text{peak}} = 0 \implies t_{\text{peak}} = \frac{b}{c}$$
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522094454866.png)

Substituting this critical time $t_{\text{peak}}$ back into the primary function yields the analytical solution for the **Peak Yield ($Y_{\text{max}}$)**:
$$Y_{\text{max}} = Y\left(\frac{b}{c}\right) = a\left(\frac{b}{c}\right)^b e^{-c\left(\frac{b}{c}\right)} = a\left(\frac{b}{c}\right)^b e^{-b}$$

This uncovers a vital structural insight: the timing of peak production ($t_{\text{peak}}$) is exclusively dictated by the ratio of growth to decay parameters ($\frac{b}{c}$), entirely independent of the scale parameter $a$. Conversely, the maximum yield volume ($Y_{\text{max}}$) scales perfectly linearly with parameter $a$.

### Advanced Curvature Analysis: Inflection Points and Persistency
To map the behavioral mechanics of persistency mathematically, the second derivative of Wood's function must be evaluated to identify the point of inflection ($\frac{d^2Y}{dt^2} = 0$), where the rate of production decline transitions from accelerating acceleration to a stabilizing, linear-like decay:
$$\frac{d^2Y}{dt^2} = \frac{d}{dt} \left[ abt^{b-1}e^{-ct} - act^be^{-ct} \right]$$
Applying the product rule to each independent component yields:
$$\frac{d^2Y}{dt^2} = ab(b-1)t^{b-2}e^{-ct} - abct^{b-1}e^{-ct} - acbt^{b-1}e^{-ct} + ac^2t^be^{-ct}$$
Factoring out the shared non-zero base component ($at^{b-2}e^{-ct}$) simplifies the statement into a standard quadratic structure:
$$\frac{d^2Y}{dt^2} = at^{b-2}e^{-ct} \left[ c^2t^2 - 2bct + b(b-1) \right]$$
Setting $\frac{d^2Y}{dt^2} = 0$, the root establishing the post-peak inflection marker $t_{\text{inflection}}$ is solved using the quadratic formula:
$$c^2t^2 - 2bct + b(b-1) = 0 \implies t_{\text{inflection}} = \frac{2bc + \sqrt{(-2bc)^2 - 4c^2b(b-1)}}{2c^2} = \frac{b + \sqrt{b}}{c}$$
Biologically, $t_{\text{inflection}}$ isolates the exact temporal boundary where the acute post-peak metabolic drop-off reaches maximum velocity, immediately before smoothing out into the prolonged mid-to-late lactation persistency phase.

---

# 3. Alternative Mathematical Modeling: The Wilmink Function

The Wilmink model addresses the lactation curve using an additive linear-exponential structure:
$$Y_t = a + b e^{-kt} + ct$$

### Key Structural Features
* **Post-Peak Linear Decline Baseline ($a + ct$):** As time progresses ($t \to \infty$), the early-stage exponential adjustment term asymptotically collapses to zero ($b e^{-kt} \to 0$). The function simplifies to a linear profile: $Y_t \approx a + ct$. In a biologically realistic model, $c < 0$ dictates the continuous daily rate of production decline, while parameter $a$ represents the critical vertical intercept of this late-lactation linear baseline.
* **Early-Stage Exponential Adjustment ($b e^{-kt}$):** To accurately model the initial postpartum production surge, parameters are constrained to $b < 0$ and $k > 0$. At calving ($t=0$), the yield starts at an analytically suppressed initial value bounding the $y$-intercept at:
$$Y_0 = a + b e^{-k(0)} + c(0) = a + b$$
As $t$ increases, the term $b e^{-kt}$ climbs rapidly from its negative baseline value toward zero, constructing the characteristic upward slope. The constant $k$ acts as an adjustment velocity parameter, governing how sharply the curve transitions into its peak.

### Mathematical Derivation of Wilmink Curve Features
The operational turning point defining peak production within Wilmink's additive formulation is mapped by executing an exact first-order differentiation with respect to time $t$:
$$\frac{dY_t}{dt} = \frac{d}{dt}\left(a + b e^{-kt} + ct\right) = -bke^{-kt} + c$$
Imposing the critical baseline condition for local extrema ($\frac{dY_t}{dt} = 0$) isolates the peak time placeholder $t_{\text{peak}}$:
$$-bke^{-kt_{\text{peak}}} + c = 0 \implies e^{-kt_{\text{peak}}} = \frac{c}{bk}$$
Applying the natural logarithm to both sides isolates the explicit algebraic expression for peak timing:
$$-kt_{\text{peak}} = \ln\left(\frac{c}{bk}\right) \implies t_{\text{peak}} = \frac{1}{k}\ln\left(\frac{bk}{c}\right)$$
*(Note: Because standard empirical constraints enforce $b < 0$ and $c < 0$, the internal ratio $\frac{bk}{c}$ remains strictly positive, ensuring structural continuity within the real logarithmic domain).*

### Comparative Analytical Framework

| Mathematical & Biological Features | Wood’s Incomplete Gamma Model | Wilmink Additive Model |
| :--- | :--- | :--- |
| **Mathematical Structure** | Multiplicative (Power $\times$ Exponential) | Additive (Linear $+$ Exponential) |
| **Standard Parameter Constraints** | $a > 0, \ b > 0, \ c > 0$ | $a > 0, \ b < 0, \ c < 0, \ k > 0$ |
| **Time to Peak ($t_{\text{peak}}$)** | $$t_{\text{peak}} = \frac{b}{c}$$|$$t_{\text{peak}} = \frac{1}{k}\ln\left(\frac{bk}{c}\right)$$ |
| **Peak Yield Formula ($Y_{\text{max}}$)** | $$Y_{\text{max}} = a\left(\frac{b}{c}\right)^b e^{-b}$$|$$Y_{\text{max}} = a + \frac{c}{k}\left(1 - \ln\left(\frac{bk}{c}\right)\right)$$ |
| **Long-Term Tail Behavior ($t \to \infty$)** | Asymptotically decays to zero ($Y_t \to 0$) | Diverges linearly to negative infinity ($Y_t \to -\infty$) |
| **Empirical Fit Flexibility** | Excellent for classic unimodal shapes; struggles when initial calving yields are highly elevated. | Highly flexible for empirical regressions; isolates the initial calving baseline from the decay rate. |

---

# 4. Comparative Analysis of Lactation Profiles Across Species

### Graph Structural Features (Axes, Scales, and Units)
To evaluate the cross-species adaptability of Wood’s incomplete gamma model, a comparative analysis was conducted between **Dairy Cows (Bovine: Holstein, Parity 1)** and **Dairy Sheep (Ovine: Awassi, Parity 2)** utilizing the empirical parameters established in Table 3 (Steri et al., 2010). 

The mathematical models for both species are defined as follows:
* **Bovine (Holstein, Parity 1):** $$Y_{\text{cow}}(t) = 13.46 \cdot t^{0.22} \cdot e^{-0.0041t}$$
* **Ovine (Awassi, Parity 2):** $$Y_{\text{sheep}}(t) = 0.42 \cdot t^{0.33} \cdot e^{-0.012t}$$

When plotting these functions, the structural configurations of the graphs reveal distinct scaling properties:
* **Horizontal Axis ($t$):** Both graphs share an identical independent variable representing 'Days in Milk' (DIM) measured in units of **days**. The operational domain is set across a standard lactation cycle of $[0, 305]$ days.
* **Vertical Axis ($Y_t$):** Both axes quantify daily milk volume (measured in **Litres per day, L/day** or Kilograms per day). However, their geometric scales require drastically different vertical windows to maintain visual clarity. The Bovine curve requires a vertical scaling window of $[0, 40]$ to capture its massive amplitude, whereas the Ovine curve requires a highly compressed vertical window of $[0, 2]$ to prevent the profile from appearing as a flat line.


![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522120223890.png)
>red line: cow.
>black line: sheep.



### Core Mathematical and Geometric Similarities
1. **Universal Unimodal Geometric Morphology:** Despite the immense variance in absolute parameters, both functions generate a classic **unimodal (single-peaked)** curve. Because both species exhibit parameter constraints where $b > 0$ and $c > 0$, the first derivative $\frac{dY}{dt} = at^{b-1}e^{-ct}(b-ct)$ mathematically guarantees a singular local maximum post-calving. Both curves possess an initial steep concave-down acceleration phase, a stationary turning point, and a long, smooth linear-exponential decay tail.
2. **Early Proportional Peak Attainment:** Analytically evaluating the turning points using $t_{\text{peak}} = \frac{b}{c}$ reveals that both species achieve their maximum daily production capacity remarkably early relative to their full 305-day lactation cycle:
   * **Bovine ($t_{\text{peak}}$):** $$\frac{0.22}{0.0041} \approx 53.66 \text{ days}$$
   * **Ovine ($t_{\text{peak}}$):** $$\frac{0.33}{0.012} = 27.50 \text{ days}$$
   
   Geometrically, both species hit their peak within the first 10% to 18% of their active milking window, demonstrating a mathematically synchronized early-stage acceleration dynamic.

---

### Core Mathematical and Geometric Differences
1. **Absolute Yield Scaling and Curve Amplitude:** The scale parameter $a$ introduces an extreme vertical dilation discrepancy. The baseline scaling factor for the cow ($a = 13.46$) is over **32 times greater** than that of the sheep ($a = 0.42$). Substituting the respective $t_{\text{peak}}$ values into the peak yield formula $Y_{\text{max}} = a\left(\frac{b}{c}\right)^b e^{-b}$ yields:
   * **Bovine Peak Yield ($Y_{\text{max}}$):** $$13.46 \times (53.66)^{0.22} \times e^{-0.22} \approx 25.97 \text{ L/day}$$
   * **Ovine Peak Yield ($Y_{\text{max}}$):** $$0.42 \times (27.50)^{0.33} \times e^{-0.33} \approx 0.90 \text{ L/day}$$
   
   This creates a massive discrepancy in the vertical amplitude of the curves, where the bovine profile demonstrates immense vertical stretching.

2. **Post-Peak Decay Gradients and Persistency Profiles:** The exponential decay constant $c$ dictates a highly divergent tail behavior between the two models. The decay parameter for the sheep ($c = 0.012$) is nearly **3 times larger** than that of the cow ($c = 0.0041$). In geometric terms, this means that after reaching its peak at Day 27.5, the Ovine curve exhibits an aggressive, steep downward slope. Conversely, the Bovine curve exhibits a highly prolonged, gentle downward slope post-Day 53.66, signaling a much flatter and more stable late-lactation tail.
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522115452517.png)
![](https://raw.githubusercontent.com/icaijy/md-image/main/20260522120008735.png)



### Deep Biological Analysis and Interpretations (The "Why")
* **Biological Rationale for Structural Similarities:** The shared unimodal profile and early peak timing are governed by universal mammalian endocrine mechanisms triggered postpartum. The act of giving birth (calving/lambing) induces a massive systemic surge in lactogenic hormones (such as prolactin and growth hormone), which rapidly stimulates and recruits dormant mammary secretory epithelial cells, causing the initial explosive surge in yield ($b > 0$). Eventually, the biological system reaches its cellular limit at $t_{\text{peak}}$. Beyond this point, the natural, gradual aging and programmed cell death (apoptosis) of these secretory cells sets in, causing the inevitable, continuous decline in daily milk volume ($c > 0$).
* **Biological Rationale for Amplitude and Scale Differences:** The extreme difference in the scale parameter $a$ and absolute peak volume $Y_{\text{max}}$ is rooted in body mass scaling and intensive genetic selection. Dairy cattle possess a significantly larger physical mammary gland volume and a vastly superior network of blood vessels capable of pumping hundreds of litres of nutrient-rich blood through the udder per hour. Furthermore, the Holstein breed has undergone centuries of intensive, systematic artificial selection aimed exclusively at maximizing commercial fluid milk volume, prioritizing metabolic energy toward milk synthesis far beyond the evolutionary baseline of small ruminants like the Awassi sheep.
* **Biological Rationale for Decay Tail and Persistency Variance:** The discrepancy in the post-peak decline slope ($c$) reflects fundamentally different biological energy allocation strategies. High-producing dairy cows are genetically driven to maintain a stable, prolonged population of active mammary epithelial cells via continuous endocrine support, optimizing long-term "persistency" for commercial farming. Conversely, sheep exhibit lower lactation persistency; their biological systems are evolutionary optimized to down-regulate milk production rapidly once the offspring passes the initial critical growth phase. Their higher mammary cell apoptosis rate ($c = 0.012$) allows them to aggressively conserve metabolic energy, reallocating bodily resources toward maintaining their own body condition and preparing for the next seasonal reproductive cycle.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1NjgyNzIzMiwtMTUyNTEwOTk3N119
-->