# HCO⁺ Opacity Explorer

An interactive calculator for exploring what the HCO⁺ and H¹³CO⁺ $J=1\!\to\!0$ line ratio can tell us about optical depth. Vary the line-centre optical depth, abundance ratio, and observing conditions to see how the simulated spectra and the precision of the inferred opacities change.

## Run locally

Download or clone this repository and open [`index.html`](index.html) in a modern browser. No installation or build is needed. The calculations run in the browser; formatted equations and web fonts load from external services. A [hosted version](https://xfengwei.github.io/hcop_tau/) is also available.

Start with the optical-depth slider, then vary the abundance ratio $X$ and the per-channel RMS. Compare the simulated spectra, the peak-ratio curve, and the fractional uncertainty for each line. The integrated-intensity ratio is shown for comparison; it is **not** the ratio used for inversion.

## Scientific motivation

The tool is inspired by **Appendix F, equations (F1)–(F2)** of Xu et al. (2023), *ATOMS: ALMA Three-millimeter Observations of Massive Star-forming regions – XV. Steady accretion from global collapse to core feeding in massive hub-filament system SDC335*, *MNRAS*, **520**, 3259–3285 ([publisher](https://academic.oup.com/mnras/article/520/3/3259/7040271) · [SciX](https://scixplorer.org/abs/2023MNRAS.520.3259X/abstract)).

For SDC335, the authors used the optically thick HCO⁺ brightness to estimate the excitation temperature with unit beam filling, applied the optically thin approximation to H¹³CO⁺, and used the peak brightness ratio to check the thick-line assumption. The explorer is an extension for synthetic spectra: it takes the excitation temperature as an input, retains the full opacity dependence of both lines, and explores when the ratio provides a useful constraint. It does not reproduce the paper's opacity map.

## Equations and assumptions

For isotopologue $i$ (12 for HCO⁺, 13 for H¹³CO⁺), the tool models the background-subtracted brightness temperature of a single emitting component:

$$
T_i(v)=A_i[1-e^{-\tau_i(v)}],\qquad
A_i=f[J_{\nu_i}(T_{\rm ex})-J_{\nu_i}(T_{\rm bg})],\qquad
J_\nu(T)=\frac{h\nu/k_{\rm B}}{e^{h\nu/(k_{\rm B}T)}-1}.
$$

The lines share one LTE excitation temperature, beam-filling factor $f$, central velocity $v_0$, and intrinsic velocity dispersion $\sigma_v$. The optical-depth profiles are Gaussian, $\tau_i(v)=\tau_{0,i}e^{-(v-v_0)^2/(2\sigma_v^2)}$; the resulting **brightness** profiles are not necessarily Gaussian because saturation broadens them. The background is fixed at $T_{\rm bg}=2.725$ K.

With equal dipole moments and $X=[\mathrm{HCO}^+]/[\mathrm{H}^{13}\mathrm{CO}^+]$, the implementation sets $\tau_{0,12}=\kappa X\tau_{0,13}$. The LTE correction $\kappa$ is the ratio (12 over 13) of $(1-e^{-h\nu_i/(k_{\rm B}T_{\rm ex})})/Q_i(T_{\rm ex})$, with $Q_i$ the rigid-rotor partition function. The peak brightness ratio is

$$
R=\frac{T_{12}(v_0)}{T_{13}(v_0)}
=\varepsilon\frac{1-e^{-\kappa X\tau_{0,13}}}{1-e^{-\tau_{0,13}}},
\qquad \varepsilon=\frac{A_{12}}{A_{13}}.
$$

The common filling factor cancels from $R$, but a smaller $f$ makes both lines fainter and worsens the noise constraint. At fixed $T_{\rm ex}$, the tool propagates independent ratio and abundance uncertainties to first order, so $\sigma_{\tau_{0,13}}^2\simeq[\sigma_R^2+(\partial_X R\,\sigma_X)^2]/(\partial_{\tau_{0,13}}R)^2$. The optional whole-line mode approximates the improvement by the square root of the number of channels across the intrinsic FWHM; it does not fit the displayed spectra.

## Interpretation and limitations

- **Both lines thin:** $R\to\varepsilon\kappa X$. The ratio is insensitive to optical depth; at best it gives an upper limit. **Main line saturated, rare line still responsive:** the ratio constrains the rare-line opacity most directly, while the main-line opacity still depends on $X$. **Both lines saturated:** $R\to\varepsilon$; at best the ratio gives a lower limit.
- $X$ is a molecular abundance ratio. Presets based on Galactic isotope gradients are starting points, not measurements for a particular cloud. Chemical fractionation can make the molecular ratio differ from the elemental isotope ratio.
- The model does not include unequal excitation or emitting areas, radiative trapping that changes excitation, foreground self-absorption, a bright continuum background, or blended velocity components. These can bias real line ratios.
- The displayed uncertainties describe the assumed noise and abundance error, not model or calibration systematics. Near saturation or at low signal-to-noise, the inversion is poorly conditioned and the first-order error estimate is not a confidence interval. Use this as an exploratory tool, not as a substitute for radiative-transfer modelling or a fit to observed data.

## Copyright

Copyright © 2026 Fengwei Xu.
