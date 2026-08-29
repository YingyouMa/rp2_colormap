# Optimal color immersion for three-dimensional nematic directors

## Motivation

A director field in a three-dimensional nematic liquid crystal is represented by a **unit director**

$$
\hat{\mathbf n}=(n_x,n_y,n_z),\qquad \hat{\mathbf n}\cdot\hat{\mathbf n}=1,
$$

with the nematic identification

$$
\hat{\mathbf n}\sim-\hat{\mathbf n}.
$$

For visualization, we would like to assign a color to every possible director orientation. Because a displayed color is described by three coordinates (ultimately RGB), it is tempting to imagine that one can construct a one-to-one correspondence between three-dimensional nematic orientations and three-dimensional color coordinates.

This is not a trivial coordinate-matching problem. The obstruction is topological.

The space of all unit vectors is the sphere $\mathbb S^2$. Nematic symmetry identifies every antipodal pair $\hat{\mathbf n}$ and $-\hat{\mathbf n}$, so the physical orientation space is

$$
\mathbb S^2/\{\hat{\mathbf n}\sim-\hat{\mathbf n}\}
\cong \mathbb{RP}^2,
$$

the real projective plane.

A key starting point of this project is that $\mathbb{RP}^2$ cannot be continuously embedded in $\mathbb R^3$. Equivalently, there is no continuous one-to-one assignment

$$
\mathbb{RP}^2\hookrightarrow\mathbb R^3
$$

that realizes the nematic orientation space as a surface without self-identification. Since the RGB cube is a subset of $\mathbb R^3$, restricting the target to physical RGB colors cannot remove this obstruction.

Therefore, any continuous three-coordinate color representation of a three-dimensional nematic director field must fail to be globally one-to-one somewhere.

The goal of this project is not to avoid that impossibility, but to construct the **best possible immersion** for scientific visualization.

---

## Embedding and immersion: what do they mean here?

The terms *embedding* and *immersion* come from differential geometry. They are easy to interpret in the present problem.

Suppose we construct a smooth color map

$$
\mathbf c:\mathbb{RP}^2\longrightarrow\mathbb R^3.
$$

An **embedding** would mean, roughly, that the entire nematic orientation space is placed inside color space without identifying any two distinct physical orientations. Distinct points of $\mathbb{RP}^2$ would always receive distinct colors, and the resulting surface in color space would have no self-intersections.

That is precisely what is impossible for $\mathbb{RP}^2$ in three dimensions.

An **immersion** is weaker. It only requires the map to remain locally two-dimensional. At every director orientation, two independent infinitesimal changes of orientation must produce two independent infinitesimal changes in color.

More formally, at each point $[\hat{\mathbf n}]\in\mathbb{RP}^2$, the differential

$$
D\mathbf c_{[\hat{\mathbf n}]}:
T_{[\hat{\mathbf n}]}\mathbb{RP}^2
\longrightarrow
T_{\mathbf c([\hat{\mathbf n}])}\mathbb R^3
$$

must be injective. Since $T_{[\hat{\mathbf n}]}\mathbb{RP}^2$ is two-dimensional, this is equivalent to

$$
\operatorname{rank}\!\left(D\mathbf c_{[\hat{\mathbf n}]}\right)=2
\qquad
\forall\,[\hat{\mathbf n}]\in\mathbb{RP}^2.
$$

Thus an immersion is allowed to intersect itself globally: two distant director orientations may occasionally receive the same color. What is forbidden is a local collapse in which a two-dimensional neighborhood of director orientations is flattened into a curve or a point.

This distinction is exactly what makes the visualization problem possible. A globally one-to-one map does not exist, but smooth immersions of $\mathbb{RP}^2$ into $\mathbb R^3$ do exist. Classical examples include Boy's surface.

---

## What should an optimal color immersion preserve?

Once global one-to-one correspondence is known to be impossible, the problem becomes an optimization problem rather than a search for an exact representation.

For two unit nematic directors $\hat{\mathbf n}$ and $\hat{\mathbf m}$, a natural sign-invariant physical distance is

$$
d_Q\!\left(\hat{\mathbf n},\hat{\mathbf m}\right)^2
=
1-\left(\hat{\mathbf n}\cdot\hat{\mathbf m}\right)^2.
$$

This distance vanishes both for $\hat{\mathbf m}=\hat{\mathbf n}$ and for $\hat{\mathbf m}=-\hat{\mathbf n}$, as required by nematic symmetry. It is also directly related to the Frobenius distance between uniaxial $Q$ tensors at fixed scalar order parameter.

Ideally, perceptually similar colors should correspond to physically similar directors, while physically distant directors should receive perceptually distinct colors. We therefore seek a map for which color-space distance approximately follows $d_Q$ over the entire orientation space.

A representative global metric objective is

$$
\mathcal J_{\mathrm{pair}}
=
\iint_{\mathbb{RP}^2\times\mathbb{RP}^2}
\left[
\left\|\mathbf c([\hat{\mathbf n}])-\mathbf c([\hat{\mathbf m}])\right\|^2
-\alpha\left(1-\left(\hat{\mathbf n}\cdot\hat{\mathbf m}\right)^2\right)
\right]^2
\,\mathrm d\mu([\hat{\mathbf n}])\,\mathrm d\mu([\hat{\mathbf m}]),
$$

where $\mathrm d\mu$ is the uniform orientational measure and $\alpha$ sets the overall scale relating physical and perceptual distances.

Metric fidelity is not the only requirement. For scientific visualization we also care about quantities such as

- approximately uniform perceptual lightness, so that apparent brightness does not create artificial structure;
- intuitive reference colors for important directions, for example assigning the Cartesian axes to colors near red, green, and blue;
- sufficient contrast on a white background;
- confinement to the displayable sRGB gamut;
- preservation of the immersion condition, except for the unavoidable global self-intersections.

These requirements compete with one another. For example, forcing an almost constant lightness tends to flatten the image of $\mathbb{RP}^2$ toward a two-dimensional plane in color space, whereas metric fidelity may prefer to use all three perceptual dimensions. The correct object is therefore not a single geometrically perfect map, but a controlled trade-off among physically and perceptually meaningful objectives.

---

## RGB coordinates versus perceptual color space

The final colors must be displayable as sRGB values, but Euclidean distance in raw RGB coordinates is not a good model of perceived color difference. In this project, RGB should therefore be understood as the final display representation rather than necessarily the space in which the geometry is optimized.

The working strategy is to construct the immersion in a three-dimensional perceptual color space, such as OKLab,

$$
\mathbf c=(L,a,b),
$$

and then require the resulting colors to lie inside the sRGB gamut after conversion.

This separates two issues:

1. the geometry of the map from nematic orientation space into perceptual color space;
2. the hardware/display constraint that the resulting colors must be realizable in sRGB.

---

## Strategy of this project

The project begins with a known analytic immersion of $\mathbb{RP}^2$ related to Boy's surface. Rather than immediately searching over all possible maps, the first stage keeps this immersion geometry fixed and optimizes how it is affinely placed in perceptual color space:

$$
\mathbf c_B([\hat{\mathbf n}])
=
\mathbf A\,\mathbf p_B(\hat{\mathbf n})+\boldsymbol\beta,
$$

where $\mathbf p_B$ is a fixed polynomial immersion, $\mathbf A\in\mathbb R^{3\times3}$ is an affine linear transformation, and $\boldsymbol\beta\in\mathbb R^3$ is a translation in color space.

This stage answers a specific question:

> How good a nematic colormap can be obtained by optimally tuning a prescribed Boy-type immersion?

A later stage can remove the Boy-surface prior and optimize over a broader family of smooth even maps,

$$
\mathbf c(-\hat{\mathbf n})=\mathbf c(\hat{\mathbf n}),
$$

for example using even spherical harmonics. Comparing the two stages will distinguish limitations caused by color tuning from limitations intrinsic to the chosen immersion geometry.

The emphasis of this repository is reproducibility. Symbolic reductions, numerical optimization, parameter scans, and validation tests should be generated from the stated mathematical definitions rather than copied from hand-derived intermediate coefficients.

The long-term objective is a colormap that makes three-dimensional nematic orientation as perceptually faithful, interpretable, and displayable as the topology of $\mathbb{RP}^2$ allows.
