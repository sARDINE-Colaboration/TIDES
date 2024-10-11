# Methods on fish movement

The movements of individual fishes is modeled by a model with inertia introduced for 2D-movement in [Klamser & Gómez-Nava et al. (2021)](https://doi.org/10.3389/fphy.2021.715996). The velocity vector $\mathbf{v} _i$ of individual $i$ changes over time according to

$$
\frac{\text{d}\mathbf{v} _i}{\text{d}t} =  \frac{1}{m}
\left( \mathbf{F} _i - \alpha
    \left(
        \frac{\text{d} \varphi _i}{\text{d}t} \hat{\mathbf{e}} _{\varphi, i} +
        \frac{\text{d} \theta _i}{\text{d}t} \hat{\mathbf{e}} _{\theta, i}
    \right)
\right)
$$

where $\mathbf{F} _i$ is the force acting on fish $i$, $m$ is its mass, $\alpha$ is the turning friction parameter, $\varphi _i$ is its heading angle in xy-plane, $\theta _i$ is its depth angle,
$\hat{\mathbf{e}} _{\varphi, i}$ is the unit vector pointing perpendicular to the heading direction in the xy-plane and 
$\hat{\mathbf{e}} _{\theta, i}$ is the unit vector pointing perpendicular to the heading direction in the depth-plane.

To understand the second term in the equation, the turning friction, we split (see ) the change in velocity into a (i) speed $v _i$ and (ii) heading angle $\varphi _i$ change:

$$
\begin{split}

\frac{\text{d} v _i}{\text{d} t} &=  \frac{\mathbf{F} _{i}}{m}  \cdot \hat{\mathbf{e}} _{v, i} \\

\frac{\text{d} \varphi _i}{\text{d} t} &= \frac{\mathbf{F} _{i}}{m v _i + \alpha} \cdot \hat{\mathbf{e}} _{\varphi, i} \\

\frac{\text{d} \theta _i}{\text{d} t} &= \frac{\mathbf{F} _{i}}{m v _i + \alpha} \cdot \hat{\mathbf{e}} _{\theta, i} \ .
\end{split}
$$

We see that the turning friction has no effect on the change in speed, but alters the change in heading and depth angle such that it does not allow instantaneous turning at $v_i=0$.

The force $\mathbf{F} _i$ acting on individual $i$ has a self-propulsion, an environmental and a social term:

$$
\mathbf{F} _i =
      \mathbf{F} _{i,\text{sp}}
    + \mathbf{F} _{i, \text{env}}
    + \mathbf{F} _{i, \text{soc}}
$$

The self-propulsion force regulates the individual's current speed to the preferred speed $v _0$, and adds fluctuations to speed $v$ and angular speeds $\dot{\varphi}$, $\dot{\theta}$:

$$
\mathbf{F} _{i, \text{sp}} = 
\left(
    \beta (v _0 - v _i) + \sqrt{2 D _v} \xi _{v}
\right) \hat{\mathbf{e}} _{i, v}
+ \left(
    \sqrt{2 D _{\varphi}} \xi _{\varphi} 
\right) \hat{\mathbf{e}} _{i, \varphi}
+ \left(
    \sqrt{2 D _{\theta}} \xi _{\theta} 
\right) \hat{\mathbf{e}} _{i, \theta}
$$

with $\beta$ as the speed relaxation coefficient, $\xi _v$, $\xi _{\varphi}$, $\xi _{\theta}$ as independent Gaussian white noise processes and $D _v$, $D _{\varphi}$ and $D _{\theta}$ as diffusion coefficients which set the noise intensity.

The environmental forces modulate the individual's response to the water surface, to the waters' ground and its shore line: 

$$
\mathbf{F} _{i, \text{env}} = \sum _{k \in [\text{surface, ground, shore}]} \mathbf{F} _{i, k} 
$$

with a force depending on the collision time $t _{i, \text{col}}$ with the respective border
$$
\mathbf{F} _{i, k} =
    \begin{cases}
        \text{sgn}(\hat{\mathbf{e}} _{i, x} \cdot \hat{\mathbf{n}} _{i, \text{B}}) \mu _k \hat{\mathbf{e}} _{i, x} &\text{ if } t_{i, \text{col}} \leq t _c \\
        0 &\text{ if } t_{i, \text{col}} > t _c \\
    \end{cases}
$$
where the signum $\text{sgn}(\dots)$ function ensures that the force points away from the border, $\hat{\mathbf{n}} _{i, \text{B}}$ is the normal vector at the collision point from the border and the $x$ in $\hat{\mathbf{e}} _{i, x}$ is $\varphi$ for shore-repulsion and $\theta$ otherwise.
In order to prevent a forth and back switching of the signum function due to the individual approaching a corner, the signum function is locked at first collision detection with a border.
Of course, the lock is released as soon as no collision is detected anymore $t_{i, \text{col}} > t _c$.


The social forces !!!!!!!!!!!!!!!!!!!!MISSING!!!!!!!!!!!!!!!!!!!!

## SI: derivation of speed and angular change terms  

Here we derive from the change in velocity the speed and angular change. The change in velocity in 3 dimensions is

$$
\begin{split}
\frac{\text{d}\mathbf{v} _i}{\text{d}t} 
    &= 
    \frac{\text{d}}{\text{d}t} ( v_i\  \hat{\mathbf{e}} _{v, i})
    =
    \frac{\text{d}}{\text{d}t} v_i \left(
        \begin{array}{c}
            \cos(\varphi) \sin(\theta) \\
            \sin(\varphi) \sin(\theta) \\
            \cos(\theta)
            
        \end{array} \right)\\
    &= 
    \frac{\text{d} v _i}{\text{d}t} \hat{\mathbf{e}} _{v, i}
        + v _i \frac{\text{d} \varphi}{\text{d}t}  \frac{\text{d} \hat{\mathbf{e}} _{v, i}}{\text{d} \varphi}
        + v _i \frac{\text{d} \theta}{\text{d}t}  \frac{\text{d} \hat{\mathbf{e}} _{v, i}}{\text{d} \theta} \\
    &=
    \frac{\text{d} v _i}{\text{d}t} \hat{\mathbf{e}} _{v, i}
        + v _i \frac{\text{d} \varphi}{\text{d}t}  \hat{\mathbf{e}} _{\varphi, i}
        + v _i \frac{\text{d} \theta}{\text{d}t}  \hat{\mathbf{e}} _{\theta, i}

\end{split}
$$

Since the unit vectors [$\hat{\mathbf{e}} _{v, i}$, $\hat{\mathbf{e}} _{\varphi, i}$, $\hat{\mathbf{e}} _{\theta, i}$] are a orthonormal basis, we can compute the change in speed, heading angle and depth angle by:

$$
\begin{split}
\frac{\text{d} v _i}{\text{d}t} 
    &= 
    \frac{\text{d}\mathbf{v} _i}{\text{d}t} \cdot
    \hat{\mathbf{e}} _{v, i} = \\
\frac{\text{d} \varphi _i}{\text{d}t} 
    &= 
    \frac{1}{v_i}
    \frac{\text{d}\mathbf{v} _i}{\text{d}t} \cdot
    \hat{\mathbf{e}} _{\varphi, i}  \\
\frac{\text{d} \theta _i}{\text{d}t} 
    &= 
    \frac{1}{v_i}
    \frac{\text{d}\mathbf{v} _i}{\text{d}t} \cdot
    \hat{\mathbf{e}} _{\theta, i}  \\


\end{split}
$$

In the following we compute $\frac{\text{d} \varphi}{\text{d} t}$ which can be repeated analogously for $\theta$:

$$
\begin{split}
&\frac{\text{d} \varphi _i}{\text{d}t} 
    = 
    \frac{1}{v_i}
    \frac{\text{d}\mathbf{v} _i}{\text{d}t} \cdot
    \hat{\mathbf{e}} _{\varphi, i}
    =
    \frac{ 1 }{m v_i}
    \left( \mathbf{F} _i \cdot \hat{\mathbf{e}} _{\varphi, i} - \alpha
            \frac{\text{d} \varphi _i}{\text{d}t} 
    \right) \\
    
\rightarrow\ 
&\frac{\text{d} \varphi _i}{\text{d}t}
    \underbrace{
    \left( 
        1 + \frac{\alpha}{m v _i}
    \right)
    } _{\frac{m v _i - \alpha}{m v _i}}
    =
    \frac{ \mathbf{F} _i }{m v_i}
     \cdot \hat{\mathbf{e}} _{\varphi, i}  \\

\rightarrow\ 
&\frac{\text{d} \varphi _i}{\text{d}t}
    =
    \frac{ \mathbf{F} _i }{m v _i + \alpha}
     \cdot \hat{\mathbf{e}} _{\varphi, i} \ .
\end{split}
$$

## SI: Notes on the rotational friction coefficient

Note that we assume here the overdamped case, i.e. we neglect the moment of inertia due to dominating turning friction.

At first glance it seems odd, that the turn friction still exists, even if the mass is set to zero.
However, the introduced turn friction relaxes the point-like particle assumption and is meant to represent the friction of the surrounding fluid with an object of non-zero volume.
That means the friction parameter rather relates to the surface of the object, than to its mass.
Thus, an extremely light object with $m \rightarrow 0$ that still has an surface area $A \gg 0$ does still experience finite turning speed.

Of course, the surface area for objects of similar shape and density, is a function of the mass, and for spherical particle, the relation is

$$ 
\begin{split}
    m &= \rho V = \rho \frac{4}{3} \pi r ^3 \\
    A &= 4 \pi r ^2 \\
    \rightarrow A &\propto m ^{2/3} \ .
\end{split}
$$