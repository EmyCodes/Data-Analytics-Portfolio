# An Order Six Numerical Method for Direct Solution of General Second Order ODEs
**Industrial Mathematics Thesis | Federal University of Technology, Akure (FUTA)**

## 📂 Documentation
* **Full Thesis (PDF):** [Download MTS192584_OGUNDARE_THESIS.pdf](./Documentation/MTS192584_OGUNDARE_THESIS.pdf)
* **Maple Source Code:** [View Scripts](./Scripts)

---

## 📌 Project Overview
This repository contains the full mathematical derivation and computational implementation of a **Sixth-Order Numerical Method** designed for the direct solution of general second-order Initial Value Problems (IVPs):
$$y'' = f(x, y, y'), \quad y(x_0) = y_0, \quad y'(x_0) = y'_0$$

Developed as a final year project in the Department of Mathematical Sciences, FUTA, this method utilizes a **Power Series (Taylor Series)** basis function to maintain the original second-order structure, avoiding the computational doubling required by reduction to first-order systems.

---

## 🛠️ Technical Stack
* **Computational Engine:** Maple (Matrix inversion, Taylor expansions, Numerical Experiments)
* **Typesetting:** LaTeX (High-precision mathematical documentation)
* **Mathematical Theory:** Collocation & Interpolation, Linear Multistep Methods (LMM).

---

## Mathematical Methodology

### 1. The Basis Function
We assume an approximate solution of the form:
$$y(x) = \sum_{j=0}^{7} a_j x^j$$
By collocating at $p=5$ points and interpolating at $q=3$ points, we construct the following matrix system $AX = B$:


$$
\begin{bmatrix}
1 & x_n & x_n^2 & x_n^3 & x_n^4 & x_n^5 & x_n^6 & x_n^7 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \\
0 & 0 & 2 & 6x_{n+4} & 12x_{n+4}^2 & 20x_{n+4}^3 & 30x_{n+4}^4 & 42x_{n+4}^5
\end{bmatrix}
\begin{bmatrix} a_0 \\ \vdots \\ a_7 \end{bmatrix} = \begin{bmatrix} y_n \\ \vdots \\ f_{n+4} \end{bmatrix}
$$

### 2. The Derived Discrete Scheme
Evaluating the continuous form at $t=4$ yields the main discrete scheme:
$$y_{n+4} - 2y_{n+2} + y_n = \frac{h^2}{15} (f_{n} + 16f_{n+1} + 26f_{n+2} + 16f_{n+3} + f_{n+4})$$

Differentiating the continuous scheme and evaluating at $t=4$ yields:
$$y'_{n+4} = - \frac{149}{42}y_{n+2} + \frac{128}{21}y_{n+1} - \frac{107}{42}y_{n} + \frac{h^{2}}{1260} \left( 325f_{n} + 4048f_{n+1} + 1106f_{n+2} + 1744f_{n+3} + 397f_{n+4} \right)$$

---

## Analysis of Basic Properties
* **Order ($p$):** 6
* **Error Constant ($C_8$):** $-\frac{2}{945} \approx -0.002116$
* **Consistency:** The method is proven consistent as $p \geq 1$ and it satisfies $\rho(1)=\rho'(1)=0$ and $\rho''(1) = 2\sigma(1)$.
* **Zero-Stability:** The first characteristic polynomial $\rho(z) = z^4 - 2z^2 + 1$ has roots $z=1$ (double) and $z=-1$ (double), satisfying the root condition for second-order ODEs.
* **Stability Interval:** Determined via boundary locus method to be $(0, -0.2742)$.

---

## Numerical Results & Benchmarks

### Problem 1: Linear IVP ($y''=y$)
* **Exact Solution:** $y(x) = e^x$
* **Absolute Error:** 2.5827e-10
* **Benchmark:** Outperforms **Olayemi (2015)**

![Problem 1 Graph](./Images/problem1_graph.png)


### Problem 2: Non-Linear IVP ($y''=x(y')^2$)
* **Exact Solution:** $y(x) = 1 + \frac{1}{2}\log\left(\frac{2+x}{2-x}\right)$
* **Absolute Error:** 4.4697e-02
* **Benchmark:** Validated against **Omole (2023)**

![Problem 2 Graph](./Images/problem2_graph.png)

### Problem 3: Stiff IVP ($y''=y'$)
* **Exact Solution:** $y(x) = 1-e^x$
* **Absolute Error:** 4.0000e-10
* **Benchmark:** Validated against **Kuboye (2021)**

![Problem 3 Graph](./Images/problem3_graph.png)

### Problem 4: Oscillatory IVP ($y''+y=2\cos x$)
* **Exact Solution:** $y(x) = \cos x + x\sin x$
* **Absolute Error:** 3.6358e-04
* **Benchmark:** Validated against **Adesanya (2011)**

![Problem 4 Graph](./Images/problem4_graph.png)


---

## 📁 Repository Structure
```text
├── Documentation/
│   └── MTS192584_OGUNDARE_THESIS.pdf  
│   └── MTS192584_OGUNDARE_THESIS.tex   # Full Thesis Document (LaTeX)
├── Scripts/
│   └── Maple_Calculations_1.mw         # Maple Worksheet for Derivations
│   └── Maple_Calculations_2.mw  
│   └── Maple_Calculations_3.mw  
│   └── Maple_Calculations_4.mw  
├── Images/
│   └── problem1_graph.png              # Graphs
│   └── problem2_graph.png  
│   └── problem3_graph.png  
│   └── problem4_graph.png  
└── README.md
```

## 📫 Connect with me:
* **LinkedIn:** [linkedin.com/in/emycodes](https://linkedin.com/in/emycodes)