# Light Polution: Tool 1
This project is a small exposure-reasoning tool that estimates how outdoor lighting on CSU’s campus contributes to sky brightness and star visibility as you travel away from campus, particularly in the Cache la Poudre Canyon direction. The tool is intended for order-of-magnitude reasoning and scenario comparison (e.g., “what if lighting is reduced by 30%?” or “what if it’s cloudy?”), not precise photometric prediction.

**Direct Observations**
- On a clear, cloudless, night, the night sky on the CSU Intramural Fields looks hazy compared to the night sky along the Cache la Poudre River. 
- There are significantly few stars visible on the CSU Intramural Fields than along the Cache la Poudre River.

**Inferences**
1. Campus lighting is scattering in the atmosphere, especially off aerosols and/or humidity, creating skyglow that reduces night-sky visibility.
2. Cloud cover amplifies skyglow by reflecting and scattering artificial light back downward, making the sky brighter than on clear nights.
3. Some fixtures may emit more light upward or sideways than intended, increasing the amount of light that reaches the sky.
4. A high density of light sources in a small area likely increases local sky brightness compared to areas with fewer fixtures.
5. Some areas may be over lit with a higher wattage.

**Engineering question**
- If CSU reduced outdoor lighting by X%, how much would the sky brightness change on campus? Would any change effect visibility at my Poudre River spot?

**Explicit unknown**
- I don’t know the true upward light output from CSU (direct uplight + reflected light), and I don’t know exactly how fast skyglow decays with distance.

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
- Files:  [Live Interactive Tool](https://jdgorton.github.io/Light-Pollution-Tool-1/interactive/)
- Code Used: [Source Code](interactive/index.html)

## Model overview and flow chart
The model is defined in:
- https://miro.com/app/live-embed/uXjVG9_VLcw=/?embedMode=view_only_without_ui&moveToViewport=-372%2C-86%2C1203%2C544&embedId=144339276103 

Core structure:
- Baseline sky brightness ratio at CSU IM Fields: **R0 = 35× natural**
- Natural sky baseline: **μ_nat = 21.9 mag/arcsec²**
- Distance decay: **f(d) = (d0 / (d0 + d))^p**
- Sky condition multiplier: **g(S)** = {Clear, Hazy, ThinCloud, Overcast}
- Total brightness ratio: **R_total(d) = 1 + (R0 − 1)·ULI·g(S)·f(d)**  
- Convert to μ(d), then to NELM and star estimate.

## How to use
### Excel
1. Open `excel/Tool1_LightPollution_CSU.xlsx`
2. May have to open / make a copy on the desktop version
3. Adjust inputs on the `Tool` sheet (ULI, distance, sky condition)
4. View the distance curve and chart on the `Curve` sheet

### Gemini interactive tool
1. Open
2. Adjust imputs using the sliders
3. View the star count on the simulated sky
4. Read `How to interpert these results`

## Assumptions + limitations
- CSU is treated as a single source region (Intramural Fields baseline anchors the model).
- Atmospheric effects are represented with a coarse multiplier (clear → overcast).
- Terrain shielding, line-of-sight effects, and local lighting sources along the route are not modeled.
- The star-count estimate is intentionally rough and should be interpreted qualitatively.
- Uniform atmospheric scattering
- Power-law distance decay

## Coordinates used
- CSU Intramural Fields (source): (40.573661, -105.093134)
- Poudre River spot (observer): (40.697135, -105.254757)
