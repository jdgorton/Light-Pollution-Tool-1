# Light Polution: Tool 1
This project is a small exposure-reasoning tool that estimates how outdoor lighting on CSU’s campus contributes to sky brightness and star visibility as you travel away from campus, particularly in the Cache la Poudre Canyon direction. The tool is intended for order-of-magnitude reasoning and scenario comparison (e.g., “what if lighting is reduced by 30%?” or “what if it’s cloudy?”), not precise photometric prediction.

## Scenario + Engineering Question
On clear nights, the night sky above CSU’s Intramural Fields appears hazier and shows fewer stars than the night sky along the Cache la Poudre River.  
**Engineering question:** If CSU reduced outdoor lighting by X%, how much would the sky brightness change on campus? Would any change affect visibility at my Poudre River spot?

## What the tool outputs
Given user inputs, the model estimates:
- **Sky brightness** μ(d) in **mag/arcsec²**
- **Visibility proxy:** **NELM** (naked-eye limiting magnitude)
- **Estimated visible stars** (rough, used only for interpretation)

## Implementations
This tool contains two implementations of the same model:

1) **Excel tool (baseline)**
- Interactive inputs (ULI, distance, sky condition) and a distance curve + chart.
- File: [excel/Tool1_LightPollution_CSU.xlsx](https://colostate-my.sharepoint.com/:x:/g/personal/jdgorton_colostate_edu/IQAeIv9CEOUfQY4V9l-XCt2xAWuh6IyVs-rJa86wepqh9x8?e=BOpVbT)

2) **Interactive tool (Gemini-built)**
- A more interactive version generated with Gemini and then adjusted to match the same model.
- Files:  [Live Interactive Tool](https://yourusername.github.io/tool1-light-pollution/interactive/) [Source Code](interactive/index.html)

## Model overview (single source of truth)
The model is defined in:
- `model/model_spec.md` (variables + equations)
- `model/constants.md` (default parameters)

Core structure:
- Baseline sky brightness ratio at CSU IM Fields: **R0 = 35× natural**
- Natural sky baseline: **μ_nat = 21.9 mag/arcsec²**
- Distance decay: **f(d) = (d0 / (d0 + d))^p**
- Sky condition multiplier: **g(S)** = {Clear, Hazy, ThinCloud, Overcast}
- Total brightness ratio: **R_total(d) = 1 + (R0 − 1)·ULI·g(S)·f(d)**  
- Convert to μ(d), then to NELM and star estimate.

## Comparison approach (Excel vs Gemini)
To verify both implementations match, I use the same inputs in both tools and compare outputs for a small set of test cases:
- `model/test_cases.csv`
- `comparison/comparison_table.md`

## How to use
### Excel
1. Open `excel/Tool1_LightPollution_CSU.xlsx`
2. Adjust inputs on the `Tool` sheet (ULI, distance, sky condition)
3. View the distance curve and chart on the `Curve` sheet

### Gemini interactive tool
See: `interactive/usage_instructions.md`

## Assumptions + limitations
- CSU is treated as a single source region (Intramural Fields baseline anchors the model).
- Atmospheric effects are represented with a coarse multiplier (clear → overcast).
- Terrain shielding, line-of-sight effects, and local lighting sources along the route are not modeled.
- The star-count estimate is intentionally rough and should be interpreted qualitatively.

## Coordinates used
- CSU Intramural Fields (source): (40.573661, -105.093134)
- Poudre River spot (observer): (40.697135, -105.254757)
