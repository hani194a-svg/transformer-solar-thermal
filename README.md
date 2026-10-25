# Sun-Aware Transformer Thermal Calculator

A browser-based research tool that estimates the hot-spot temperature and insulation ageing of oil-immersed transformers in hot, sunny climates. It compares the standard IEEE C57.91 Clause 7 thermal model, which uses only ambient temperature and load, with an extended model that adds absorbed solar heat and wind cooling. A field-comparison module calibrates the extended model against measured top-oil temperatures.

**Status:** research prototype, version 0.2. Results are estimates and have not yet been validated against field measurements.

## Why

Loading guides relate insulation ageing to hot-spot temperature, which depends on ambient temperature and load. In the Arabian Peninsula, solar radiation adds heat that air-temperature-based calculations do not capture, so a standard calculation may underestimate hot-spot temperature and ageing. This tool quantifies that gap and provides a way to test it with field data.

The work addresses a gap in the hot-climate thermal modelling of transformers: standard calculations ignore solar heating, and measured data from the Arabian Peninsula are scarce.

## Use

Open `index.html` in any modern browser. No installation, server or internet connection is needed apart from web fonts. The interface is in Arabic.

1. Choose a site (Unaizah or NEOM) and a day (typical July day or hottest July day, 2026).
2. Set the load profile, transformer losses, cooling type, rated top-oil rise, paint colour, sunlit area and wind exposure.
3. Read the hot-spot temperature, daily insulation ageing and maximum normal-life peak load for both models.
4. To compare with measurements, paste one day of readings, one per line:
   `time, ambient °C, load %, top-oil °C, irradiance W/m² (optional), wind km/h (optional)`.
   The tool reports the error of each model and fits the sunlit area and wind exposure that best match the readings.

## Model

```
Heat ratio  = (K²·R + 1)/(R + 1) + α·G·A / P_tot
Wind factor = 0.5 + 0.5 · h₀ / (h₀ + 3.8·k·v),   h₀ = 10.7 W/m²K
Δθ_TO,U     = Δθ_TO,R · (Heat ratio)^n · Wind factor
Δθ_H,U      = Δθ_H,R · K^(2m)
θ_H         = θ_A + Δθ_TO + Δθ_H
F_AA        = exp(15000/383 − 15000/(θ_H + 273))
```

- `K` per-unit load, `R` ratio of load loss to no-load loss, `P_tot` rated total losses
- `α` solar absorptance of the paint, `G` global horizontal irradiance, `A` effective sunlit area
- `k` wind exposure factor (0 sheltered, 1 fully exposed), `v` 10 m wind speed
- Exponential top-oil and winding responses with time constants per IEEE C57.91 Clause 7, solved at one-minute steps over four repeated days to reach a periodic steady state
- Ageing for thermally upgraded paper (reference 110 °C)

The solar term is treated as an additional heat input. The wind term is a simplified assumption: half of the oil-to-air thermal resistance is external and responds to forced convection. Both are hypotheses to be tested with field data.

## Limitations

- Wind cooling is a simplified estimate and must be calibrated with measurements.
- Irradiance is on a horizontal surface; vertical tank walls receive less at midday.
- Sunlit area and absorptance are approximate inputs.
- Not a substitute for a certified loading study or manufacturer data.

## Data

Hourly weather for July 2026 from the Open-Meteo Historical Weather API (ERA5-based reanalysis), licensed under CC BY 4.0: https://open-meteo.com/

## Cite

See `CITATION.cff`. After a release is archived on Zenodo, cite the DOI shown on the Zenodo record.

## License

MIT License. See `LICENSE`.

---

## بالعربي

حاسبة بحثية تقدّر حرارة أسخن نقطة في المحول الزيتي وتآكل عزله في المناخ الحار المشمس. تقارن الطريقة المعتادة، التي تعتمد على حرارة الجو والحمل فقط، بطريقة تضيف حرارة الشمس وتبريد الرياح. وفيها قسم يقارن الطريقتين بقراءات حقيقية من محول في الموقع.

للتشغيل: افتح ملف الصفحة الرئيسية في أي متصفح.

المؤلف: هاني المطوع، باحث مستقل، عنيزة، القصيم.
