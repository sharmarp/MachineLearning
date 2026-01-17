<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>PINN Solution of 2D Navier–Stokes Equations</title>

    <!-- MathJax for LaTeX equations -->
    <script>
      MathJax = {
        tex: {
          inlineMath: [['$', '$'], ['\\(', '\\)']],
          displayMath: [['$$', '$$']]
        }
      };
    </script>
    <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

    <style>
        body {
            max-width: 900px;
            margin: auto;
            padding: 40px;
            font-family: "Georgia", "Times New Roman", serif;
            line-height: 1.6;
            background-color: #ffffff;
            color: #000000;
        }

        h1, h2, h3 {
            font-family: "Helvetica", Arial, sans-serif;
        }

        h1 {
            text-align: center;
            margin-bottom: 0;
        }

        .author {
            text-align: center;
            font-size: 1.1em;
            margin-top: 5px;
            margin-bottom: 30px;
        }

        .abstract {
            border: 1px solid #ccc;
            padding: 15px;
            background: #f9f9f9;
            margin-bottom: 30px;
        }

        .keywords {
            font-style: italic;
            margin-top: 10px;
        }

        section {
            margin-bottom: 35px;
        }

        figure {
            text-align: center;
            margin: 20px 0;
        }

        figcaption {
            font-size: 0.9em;
            color: #444;
        }

        code {
            background: #f4f4f4;
            padding: 2px 4px;
        }

        footer {
            margin-top: 50px;
            font-size: 0.9em;
        }
    </style>
</head>

<body>

<h1>Physics-Informed Neural Network Solution of the Two-Dimensional Incompressible Navier–Stokes Equations</h1>
<div class="author">Ramjee Sharma</div>

<div class="abstract">
<strong>Abstract.</strong>
A physics-informed neural network (PINN) is developed for the numerical solution of the two-dimensional incompressible Navier–Stokes equations.
The governing equations are embedded directly into the loss function of a deep neural network using automatic differentiation.
A streamfunction–pressure formulation is employed to satisfy the incompressibility constraint exactly.
The proposed method reconstructs velocity and pressure fields from sparse spatio-temporal training data.
Numerical results demonstrate accurate recovery of flow dynamics and physically consistent pressure fields.
</div>

<div class="keywords">
Keywords: Physics-informed neural networks, Navier–Stokes equations, incompressible flow, deep learning, computational fluid dynamics
</div>

<section>
<h2>1. Introduction</h2>
<p>
The incompressible Navier–Stokes equations describe the motion of viscous fluids and are fundamental in fluid mechanics.
Traditional numerical solvers such as finite difference, finite volume, and finite element methods rely on mesh-based discretizations
and often incur high computational costs for time-dependent problems.
</p>
<p>
Physics-informed neural networks (PINNs) provide an alternative mesh-free approach by embedding governing equations directly into
the training process of neural networks. Instead of requiring large labeled datasets, PINNs enforce physical laws as soft constraints
through the loss function.
</p>
</section>

<section>
<h2>2. Governing Equations</h2>
<p>
The two-dimensional incompressible Navier–Stokes equations are given by
</p>

$$
\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v \frac{\partial u}{\partial y}
= -\frac{\partial p}{\partial x}
+ \nu \left( \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} \right),
$$

$$
\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v \frac{\partial v}{\partial y}
= -\frac{\partial p}{\partial y}
+ \nu \left( \frac{\partial^2 v}{\partial x^2} + \frac{\partial^2 v}{\partial y^2} \right),
$$

$$
\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} = 0.
$$
</section>

<section>
<h2>3. Streamfunction–Pressure Formulation</h2>
<p>
To enforce incompressibility exactly, a scalar streamfunction $\psi(x,y,t)$ is introduced:
</p>

$$
u = \frac{\partial \psi}{\partial y}, \qquad
v = -\frac{\partial \psi}{\partial x}.
$$

<p>
The neural network approximates the mapping
$$
(x,y,t) \mapsto (\psi(x,y,t), p(x,y,t)).
$$
</p>
</section>

<section>
<h2>4. Physics-Informed Neural Network Architecture</h2>
<p>
A fully connected feedforward neural network is used with three inputs $(x,y,t)$,
three hidden layers with 30 neurons each and hyperbolic tangent activation functions,
and two outputs corresponding to the streamfunction and pressure.
</p>
</section>

<section>
<h2>5. Physics Residuals</h2>

$$
f = u_t + u u_x + v u_y + p_x - \nu (u_{xx} + u_{yy}),
$$

$$
g = v_t + u v_x + v v_y + p_y - \nu (v_{xx} + v_{yy}).
$$

<p>
These residuals are enforced to vanish at collocation points throughout the domain.
</p>
</section>

<section>
<h2>6. Loss Function</h2>

$$
\mathcal{L} =
\| u - u^{\text{data}} \|_2^2 +
\| v - v^{\text{data}} \|_2^2 +
\| f \|_2^2 +
\| g \|_2^2.
$$

<p>
Pressure is inferred implicitly through the momentum equations.
</p>
</section>

<section>
<h2>7. Training Procedure</h2>
<p>
Training is performed using the L-BFGS optimizer with automatic differentiation.
Spatio-temporal collocation points are randomly sampled from the reference dataset.
</p>
</section>

<section>
<h2>8. Numerical Results</h2>
<p>
The trained PINN reconstructs velocity and pressure fields across the domain.
Contour plots and animated visualizations of the pressure field demonstrate smooth,
physically consistent behavior.
</p>
</section>

<section>
<h2>9. Conclusion</h2>
<p>
A physics-informed neural network framework has been presented for solving the
two-dimensional incompressible Navier–Stokes equations.
The approach avoids explicit spatial discretization and accurately recovers
velocity and pressure fields from sparse data.
</p>
</section>

<footer>
<h3>References</h3>
<ol>
    <li>M. Raissi, P. Perdikaris, and G. E. Karniadakis, <em>Journal of Computational Physics</em>, 2019.</li>
    <li>S. B. Pope, <em>Turbulent Flows</em>, Cambridge University Press, 2000.</li>
</ol>
</footer>

</body>
</html>
