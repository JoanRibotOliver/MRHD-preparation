# Theoretical Framework & Evolution Equations for Compact Binary Mergers

This document summarizes the core partial differential equations (PDEs) and mathematical formulations used by the IAC3/UIB numerical relativity group to model Binary Neutron Star (BNS) and Black Hole–Neutron Star (BHNS) mergers, turbulent magnetic field amplification, non-ideal MHD effects, and neutrino radiation.

*(Conventions: Spacelike signature $(-,+,+,+)$, geometrized units $G = c = M_\odot = 1$ unless stated otherwise; Greek/early Latin indices $a, b, \mu, \nu \in \{0,1,2,3\}$ denote spacetime coordinates, and $i, j, k \in \{1,2,3\}$ denote spatial coordinates).*

---

## 1. Spacetime Sector: 3+1 Decomposition & CCZ4 Formulation

To evolve Einstein's field equations $G_{ab} = 8\pi (T_{ab} + T_{ab}^{\mathrm{rad}})$ as a well-posed hyperbolic initial value problem, spacetime is foliated into spacelike hypersurfaces $\Sigma_t$ with unit normal $n_a = (-\alpha, 0)$[cite: 276, 314].

### 1.1. The 3+1 Line Element
$$ds^2 = -\alpha^2 dt^2 + \gamma_{ij}(dx^i + \beta^i dt)(dx^j + \beta^j dt)$$
where $\alpha$ is the **lapse function**, $\beta^i$ is the **shift vector**, and $\gamma_{ij}$ is the induced spatial 3-metric with determinant $\gamma$[cite: 276, 314].

### 1.2. Conformal and Covariant Z4 (CCZ4) Variables
The Z4 formalism extends Einstein's equations by introducing a four-vector $Z_a$ measuring constraint violations, damped dynamically via $\kappa_z > 0$[cite: 314]:
$$R_{ab} + \nabla_a Z_b + \nabla_b Z_a = 8\pi \left(T_{ab} - \frac{1}{2}g_{ab}\mathrm{tr}\,T\right) + \kappa_z (n_a Z_b + n_b Z_a - g_{ab} n^c Z_c)$$

Applying a conformal decomposition yields the evolved geometric fields $\{\chi, \tilde{\gamma}_{ij}, \hat{K}, \tilde{A}_{ij}, \hat{\Gamma}^i, \Theta\}$[cite: 276, 314]:
$$\tilde{\gamma}_{ij} = \chi \gamma_{ij}, \qquad \tilde{A}_{ij} = \chi \left(K_{ij} - \frac{1}{3}\gamma_{ij}\mathrm{tr}\,K\right)$$
$$\hat{K} \equiv K - 2\Theta, \qquad \hat{\Gamma}^i \equiv \tilde{\Gamma}^i + \frac{2}{\chi}Z^i, \qquad \Theta \equiv -n_a Z^a$$
where $K_{ij} \equiv -\frac{1}{2}\mathcal{L}_n \gamma_{ij}$ is the extrinsic curvature and $\tilde{\Gamma}^i \equiv \tilde{\gamma}^{jk}\tilde{\Gamma}^i_{jk}$[cite: 276, 314].

### 1.3. CCZ4 Evolution Equations
* **Conformal metric & factor:**
  $$\partial_t \tilde{\gamma}_{ij} = \beta^k \partial_k \tilde{\gamma}_{ij} + \tilde{\gamma}_{ik}\partial_j \beta^k + \tilde{\gamma}_{kj}\partial_i \beta^k - \frac{2}{3}\tilde{\gamma}_{ij}\partial_k \beta^k - 2\alpha \left(\tilde{A}_{ij} - \frac{1}{3}\tilde{\gamma}_{ij}\tilde{A}\right) - \frac{\alpha}{3}\kappa_c \tilde{\gamma}_{ij}\ln\tilde{\gamma}$$
  $$\partial_t \chi = \beta^k \partial_k \chi + \frac{2}{3}\chi\left[\alpha(\hat{K} + 2\Theta) - \partial_k \beta^k\right]$$
* **Extrinsic curvature & Z4 scalars:**
  $$\partial_t \tilde{A}_{ij} = \beta^k \partial_k \tilde{A}_{ij} + \tilde{A}_{ik}\partial_j \beta^k + \tilde{A}_{kj}\partial_i \beta^k - \frac{2}{3}\tilde{A}_{ij}\partial_k \beta^k - \frac{\alpha}{3}\kappa_c \tilde{\gamma}_{ij}\tilde{A} + \chi\left[\alpha\left({}^{(3)}R_{ij} + D_i Z_j + D_j Z_i - 8\pi S_{ij}\right) - D_i D_j \alpha\right]^{\mathrm{TF}} + \alpha\left(\hat{K}\tilde{A}_{ij} - 2\tilde{A}_{ik}\tilde{A}^k_j\right)$$
  $$\partial_t \hat{K} = \beta^k \partial_k \hat{K} - D_i D^i \alpha + \alpha\left[\frac{1}{3}(\hat{K}+2\Theta)^2 + \tilde{A}_{ij}\tilde{A}^{ij} + 4\pi(\tau + S) + \kappa_z \Theta\right] + 2Z^i\partial_i \alpha$$
  $$\partial_t \Theta = \beta^k \partial_k \Theta + \frac{\alpha}{2}\left[{}^{(3)}R + 2D_i Z^i + \frac{2}{3}\hat{K}^2 + \frac{2}{3}\Theta(\hat{K}-2\Theta) - \tilde{A}_{ij}\tilde{A}^{ij}\right] - Z^i\partial_i \alpha - \alpha(8\pi \tau + 2\kappa_z \Theta)$$
* **Conformal connection functions:**
  $$\partial_t \hat{\Gamma}^i = \beta^j \partial_j \hat{\Gamma}^i - \hat{\Gamma}^j \partial_j \beta^i + \frac{2}{3}\hat{\Gamma}^i \partial_j \beta^j + \tilde{\gamma}^{jk}\partial_j \partial_k \beta^i + \frac{1}{3}\tilde{\gamma}^{ij}\partial_j \partial_k \beta^k - 2\tilde{A}^{ij}\partial_j \alpha + 2\alpha\left[\tilde{\Gamma}^i_{jk}\tilde{A}^{jk} - \frac{3}{2\chi}\tilde{A}^{ij}\partial_j \chi - \frac{2}{3}\tilde{\gamma}^{ij}\partial_j \hat{K} - 8\pi\tilde{\gamma}^{ij}S_i\right] + 2\alpha\left[-\tilde{\gamma}^{ij}\left(\frac{1}{3}\partial_j \Theta + \frac{\Theta}{\alpha}\partial_j \alpha\right) - \frac{1}{\chi}Z^i\left(\kappa_z + \frac{2}{3}(\hat{K}+2\Theta)\right)\right]$$

### 1.4. Gauge Conditions (Moving Punctures)
* **1+log slicing:** $\partial_t \alpha = \beta^i \partial_i \alpha - 2\alpha f_\alpha(\alpha) \hat{K}$ (with $f_\alpha = 1$)[cite: 277]
* **Gamma-driver shift:** $\partial_t \beta^i = \beta^j \partial_j \beta^i + \frac{3}{4}f_\beta(\alpha)\hat{\Gamma}^i - \eta(\beta^i - \beta_0^i)$ (with $f_\beta = 1$ or $4/3$, $\eta \approx 2/M$)[cite: 277, 314]

---

## 2. Matter & Magnetic Field Sector: Ideal & Non-Ideal GRMHD

### 2.1. Covariant Conservation Laws
$$\nabla_a (\rho u^a) = 0, \qquad \nabla_a (Y_e \rho u^a) = \rho \mathcal{R}, \qquad \nabla_a T^a_b = \mathcal{S}_b, \qquad \nabla_a {}^*\!F^{ab} = 0$$
where $u^a = W(n^a + v^a)$ is the fluid 4-velocity, $W = (1 - v_i v^i)^{-1/2}$ is the Lorentz factor, and $Y_e$ is the electron fraction[cite: 276, 314].

### 2.2. Flux-Conservative Formulation (Valencia Formulation)
To capture shocks via High-Resolution Shock-Capturing (HRSC) schemes, the system evolves the **conserved variables** $\sqrt{\gamma}\{D, D_Y, \tau, S_i, B^i\}$ derived from the **primitive variables** $\{\rho, \epsilon, Y_e, p, v^i, B^i\}$[cite: 277, 314]:
$$D = \rho W, \qquad D_Y = \rho W Y_e$$
$$S_i = (hW^2 + B^2)v_i - (B^k v_k)B_i$$
$$\tau \equiv U - D = hW^2 - p + B^2 - \frac{1}{2}\left[(B^k v_k)^2 + \frac{B^2}{W^2}\right] - \rho W$$
where $h = \rho(1 + \epsilon) + p$ is the total enthalpy[cite: 277, 314].

**Evolution system with Hyperbolic Divergence Cleaning ($\phi$):**
$$\partial_t(\sqrt{\gamma}D) + \partial_k\left[\sqrt{\gamma}(-\beta^k + \alpha v^k)D\right] = 0$$
$$\partial_t(\sqrt{\gamma}D_Y) + \partial_k\left[\sqrt{\gamma}(-\beta^k + \alpha v^k)D_Y\right] = \alpha\sqrt{\gamma}\rho\mathcal{R}$$
$$\partial_t(\sqrt{\gamma}\tau) + \partial_k\left[\sqrt{\gamma}\left(-\beta^k \tau + \alpha(S^k - D v^k)\right)\right] = \sqrt{\gamma}\left(\alpha S^{ij}K_{ij} - S^j\partial_j\alpha + \alpha\mathcal{Q}W\right)$$
$$\partial_t(\sqrt{\gamma}S_i) + \partial_k\left[\sqrt{\gamma}(-\beta^k S_i + \alpha S^k_i)\right] = \sqrt{\gamma}\left[\alpha\Gamma^j_{ik}S^k_j + S_j\partial_i\beta^j - (\tau+D)\partial_i\alpha + \alpha\mathcal{Q}W v_i\right]$$
$$\partial_t(\sqrt{\gamma}B^i) + \partial_k\left[\sqrt{\gamma}\left\{B^i(\alpha v^k - \beta^k) - B^k(\alpha v^i - \beta^i) + \alpha\gamma^{ki}\phi\right\}\right] = \sqrt{\gamma}\phi\left(\gamma^{ik}\partial_k\alpha - \alpha\gamma^{jk}\Gamma^i_{jk}\right)$$
$$\partial_t(\sqrt{\gamma}\phi) + \partial_k\left[\sqrt{\gamma}(-\beta^k\phi + \alpha c_h^2 B^k)\right] = \sqrt{\gamma}\left(c_h^2 B^k \partial_k\alpha - \alpha\phi\,\mathrm{tr}\,K - \alpha\kappa\phi\right)$$

### 2.3. Non-Ideal MHD Extensions
* **General-Relativistic Resistive MHD (Dionysopoulou et al. 2013):**
  When electrical conductivity $\sigma$ is finite (e.g., magnetosphere vs. stellar interior), the electric field $E^i$ is no longer given by $E^i = -\epsilon^{ijk}v_j B_k$ and must be evolved alongside Maxwell's extended equations[cite: 30, 31, 240]:
  $$\partial_t(\sqrt{\gamma}E^i) + \partial_k\left(-\beta^k\sqrt{\gamma}E^i - \alpha\epsilon^{ikj}\sqrt{\gamma}B_j\right) = -\sqrt{\gamma}E^k\partial_k\beta^i - \alpha\sqrt{\gamma}\gamma^{ij}\partial_j\psi - \alpha\sqrt{\gamma}J^i$$
  closed by an isotropic **relativistic Ohm's law**[cite: 31, 32]:
  $$J^i = q v^i + W\sigma\left[E^i + \epsilon^{ijk}v_j B_k - (v_k E^k)v^i\right]$$
* **Generalized Induction Equation in NS Interiors (Viganò et al. 2019):**
  Accounts for Ohmic resistivity ($f_d$), Hall drift ($f_h$), and ambipolar diffusion ($f_a$)[cite: 193]:
  $$\frac{\partial \vec{B}}{\partial t} + \vec{\nabla} \times \left[f_d \vec{j} + f_h (\vec{j} \times \vec{B}) - f_a (\vec{j} \times \vec{B}) \times \vec{B}\right] = 0, \qquad \vec{j} = \vec{\nabla}\times\vec{B}$$

### 2.4. Large Eddy Simulations (LES) — Gradient Subgrid-Scale (SGS) Model
To capture small-scale turbulent dynamo amplification (Kelvin-Helmholtz instability) below the grid resolution $\Delta$, filtered GRMHD equations include SGS flux tensors proportional to $\xi = \gamma^{1/3}\Delta^2 / 24$[cite: 240, 279]:
$$\tau_N^k = -\mathcal{C}_{\mathcal{N}}\xi H_N^k, \qquad \tau_{N_Y}^k = -\mathcal{C}_{\mathcal{N}}\xi H_{N_Y}^k, \qquad \tau_T^{ki} = -\mathcal{C}_{\mathcal{T}}\xi H_T^{ki}, \qquad \tau_M^{ki} = -\mathcal{C}_{\mathcal{M}}\xi H_M^{ki}$$
In BNS/BHNS production runs, $\mathcal{C}_{\mathcal{M}} = 8$ is applied primarily to the induction equation ($\partial_t(\sqrt{\gamma}B^i) + \partial_k[\dots - \alpha\sqrt{\gamma}\tau_M^{ki}] = \dots$) to recover small-scale magnetic amplification without introducing excessive numerical viscosity[cite: 245, 246, 254, 315].

---

## 3. Neutrino Radiation Sector: Leakage & M1 Moment Formalism

Neutrinos ($\nu_e, \bar{\nu}_e, \nu_x$) cool the merger remnant and drive weak interactions ($e^+ + n \to p + \bar{\nu}_e$, $e^- + p \to n + \nu_e$) that alter the electron fraction $Y_e$ and govern r-process nucleosynthesis in kilonova ejecta[cite: 278].

### 3.1. Neutrino Leakage Scheme (Palenzuela et al. 2022)
* **Radiation four-force & lepton sources:**
  $$\mathcal{S}_a = \mathcal{Q} u_a, \qquad \mathcal{Q} = -(Q_{\nu_e} + Q_{\bar{\nu}_e} + Q_{\nu_x}), \qquad \mathcal{R} = R_{\bar{\nu}_e} - R_{\nu_e}$$
* **Effective emission rate interpolation** between optically thin (free-streaming, $X_{\mathrm{free}}$) and optically thick (diffusive, $X_{\mathrm{diff}}$) regimes[cite: 278]:
  $$X_{\mathrm{eff}} = \frac{X_{\mathrm{diff}} X_{\mathrm{free}}}{X_{\mathrm{diff}} + X_{\mathrm{free}}}, \qquad X \in \{Q_\nu, R_\nu\}$$
* **Optical depth via the curved-space Eikonal equation**[cite: 282, 283]:
  $$|\nabla \tau_\nu| = \sqrt{\gamma^{ij}(\nabla_i \tau_\nu)(\nabla_j \tau_\nu)} = \kappa_\nu \implies |\nabla \tau_\nu|_{\mathrm{flat}} = \chi^{-1/2}\kappa_\nu = (\sqrt{\gamma})^{1/3}\kappa_\nu$$

### 3.2. Truncated Moments Formalism (M1 Scheme — Next-Generation Target)
To include directional neutrino transport, scattering, and reabsorption in optically thin regions, the Boltzmann equation is integrated over momentum space into its first two angular moments (energy density $E$ and flux $F^i$ per species), closed by the **Minerbo closure** for the radiation pressure tensor $P^{ij}$:
$$P^{ij} = \frac{3\chi_{\mathrm{M1}} - 1}{2} P_{\mathrm{thin}}^{ij} + \frac{3(1 - \chi_{\mathrm{M1}})}{2} P_{\mathrm{thick}}^{ij}, \qquad \chi_{\mathrm{M1}}(\xi) = \frac{1}{3} + \frac{2\xi^2}{15}(3 - \xi + 3\xi^2)$$
where $\xi = \sqrt{F_i F^i}/E \in [0, 1]$ is the Eddington flux factor.

---

## 4. Key Physical & Diagnostic Quantities

* **Plasma beta parameter:** $\beta = \frac{2p}{B^2}$ (measures fluid vs. magnetic pressure dominance; jets require $\beta^{-1} \gg 1$ along the polar funnel)[cite: 255, 259, 297, 300].
* **Kelvin-Helmholtz Instability (KHI) growth rate:** $\sigma = \frac{\pi \Delta v}{\lambda}$ (amplifies $B$ exponentially from $\sim 10^{11}\,\mathrm{G}$ to $\sim 10^{16}\,\mathrm{G}$ within $\sim 5\,\mathrm{ms}$ post-merger)[cite: 257, 261, 307].
* **Magnetic winding:** $|B^T| \approx \frac{3}{2}\Omega t |B^R|$ (linearly amplifies the toroidal field via differential rotation $\partial_R \Omega$ for $t \gtrsim 20\,\mathrm{ms}$)[cite: 257, 305].
* **Magneto-Rotational Instability (MRI) fastest mode:** $t_{\mathrm{MRI}} \approx \frac{4}{3\Omega}$, $\lambda_{\mathrm{MRI}}^i \approx \frac{2\pi}{\Omega} v_A^i$[cite: 257].
* **Gravitational Wave Extraction:** Computed via the Newman–Penrose Weyl scalar $\Psi_4$ decomposed into spin-weighted $s=-2$ spherical harmonics[cite: 256, 286, 318]:
  $$r\Psi_4(t,r,\theta,\phi) = \sum_{l,m} C_{l,m}(t,r) {}_{-2}Y_{l,m}(\theta,\phi)$$
  