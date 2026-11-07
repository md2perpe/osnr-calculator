# Link OSNR Calculator

A single-page HTML calculator for the end-of-link optical signal-to-noise ratio (OSNR) of an amplified DWDM link. Move a slider or type a value, and the result updates immediately.

The page is self-contained: one HTML file with no build step and no dependencies. Google Fonts is loaded for typography, and the page falls back to system fonts without it.

## Inputs

| Input | Unit | Range | Default |
|---|---|---|---|
| Launch power per channel, P<sub>ch</sub> | dBm | −10 … +10 | 1.0 |
| Span loss, L<sub>span</sub> | dB | 5 … 35 | 22.0 |
| Amplifier noise figure, NF | dB | 3 … 10 | 5.5 |
| Number of spans, N | – | 1 … 40 | 10 |

Each input has a slider and a number box that stay in sync. Values typed into a box are clamped to the slider's range.

## Output

- **End-of-link OSNR** in dB, shown in large type.
- **Status chip**: green at 18 dB and above, amber from 12 to 18 dB, red below 12 dB. These thresholds are rough rules of thumb, not tied to any particular modulation format.
- **Worked formula**: the equation with the current values substituted in.
- **Per-span trace**: OSNR after each span from 1 to N, which shows the 10·log₁₀(N) roll-off.

## Model

The standard ASE-limited formula for a chain of identical amplified spans:

```
OSNR [dB] = 58 + P_ch [dBm] − L_span [dB] − NF [dB] − 10·log10(N)
```

The constant 58 is −10·log₁₀(h·ν·Δν) expressed in dBm, with:

- ν ≈ 193.4 THz (λ ≈ 1550 nm)
- Δν = 12.5 GHz, the 0.1 nm reference bandwidth

For example, the defaults give 58 + 1 − 22 − 5.5 − 10 = **21.5 dB**.

### Assumptions and limitations

- All spans are identical, and each amplifier's gain exactly compensates its span loss.
- Amplifier gain is ≫ 1, so the 1/G term is neglected.
- Only ASE noise is counted. Fibre nonlinearity (SPM, XPM, FWM), PMD, CD and filtering penalties are not modelled.
- Booster and pre-amplifier stages that differ from the in-line amplifiers are not modelled separately.
- The result is the link OSNR only. Transponder back-to-back OSNR and required OSNR or margin are not included.

## Usage

Open `osnr.html` in any modern browser. Nothing needs to be installed or served.

## Origin

Built from the example prompt in the description of [this video](https://www.youtube.com/watch?v=u47uTiNgeYc).

## Possible extensions

- A required-OSNR input with a margin readout
- Per-span loss and NF values for non-uniform links
- Separate booster and pre-amp noise figures
- A GN-model nonlinear noise term to find the optimum launch power
