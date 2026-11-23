# MATLAB-ExperimentalStrainAnalysis-DataProcessingTechniques
# Experimental Strain Analysis with MATLAB

Educational MATLAB examples covering load cell calibration, signal processing, and comparison between measured strain and beam theory.

The examples form a progressive workflow: calibrate a sensor, process a simulated measurement, and assess whether the results agree with a mechanical model.

Developed for a two-hour university lecture on Experimental Strain Analysis.

**Author:** Nuno Fernandes  
**Affiliation:** University of Minho

> All datasets are synthetic and intended for teaching. Script comments and classroom instructions are primarily written in Portuguese.

## Repository structure

The teaching files are organised into three folders:

| Folder | Topic | Main objective |
|---|---|---|
| `Ex1/` | Load cell calibration | Select an appropriate linear calibration range |
| `Ex2/` | Time-domain signal processing | Identify and treat measurement problems |
| `Ex3/` | Comparison with beam theory | Estimate Young’s modulus and interpret discrepancies |

Follow the examples in order. The plateau summary exported in Example 2 can be used as input for Example 3.

## Learning objectives

After completing the examples, students should be able to:

- Convert sensor output into a physical quantity using calibration.
- Select a linear operating range using residuals and error metrics.
- Identify offset, drift, missing readings, and isolated artefacts.
- Distinguish acquisition artefacts from physical events.
- Assess how smoothing changes transient peaks.
- Calculate statistics over selected loading plateaus.
- Compare measured strain with an analytical prediction.
- Estimate an apparent Young’s modulus and assess sensitivity to geometry.

## Requirements

- MATLAB R2020b or later is recommended.
- The main examples use base MATLAB functions.
- No physical sensors or acquisition hardware are required.

The scripts are divided into sections using `%%`. Run them progressively in the MATLAB Editor or convert them into Live Scripts (`.mlx`).

## Getting started

1. Download and extract the repository.
2. Open the script for Example 1 in `Ex1/`.
3. Execute the script section by section, following its comments.
4. Select the appropriate CSV when prompted.
5. Continue with Examples 2 and 3.

Keep the supplied datasets available when running the scripts. Where a script uses a relative path, select the corresponding example folder as MATLAB’s Current Folder.

For file selection in Live Scripts, prefer the provided selection dialog. A path based on `mfilename('fullpath')` may point to a temporary execution folder.

## Example 1 — Load cell calibration

This example uses a fictional single-point bending load cell with a full strain-gauge bridge.

| Parameter | Value |
|---|---|
| Nominal capacity | 50 N |
| Bridge excitation | 5 V |
| Ideal rated sensitivity | 2 mV/V |
| Ideal force sensitivity | 0.20 mV/N |
| Reference loads | 0 to 50 N, in 5 N increments |
| Readings per load | 3 |

The simulated response includes an offset, measurement noise, and deliberately exaggerated nonlinearity above 30 N.

Students fit different calibration ranges and compare:

- Sensitivity and intercept.
- Residual patterns.
- Root mean square error (RMSE).
- Coefficient of determination (R²).

The final task converts an unknown voltage reading into force.

**Key lesson:** a high R² alone does not establish linearity.

## Example 2 — Processing time-domain measurements

This example simulates a cantilever beam subjected to a sequence of transverse loads:

**0, 5, 10, 15, 20, 10, and 0 N**

The dataset contains force and longitudinal strain sampled synchronously at 50 Hz over 140 seconds.

The measurements include:

- Initial offsets.
- Synthetic noise and small oscillations.
- Isolated electrical artefacts.
- Three missing strain readings.
- A genuine mechanical impact.
- A gradual strain drift.

Students inspect both channels, correct the initial zero, interpolate a short gap, treat confirmed artefacts, and compare moving-average windows.

They then select plateau intervals, calculate means and standard deviations, and export the results.

### Dataset used for the updated lesson

Select:

`ensaio_viga_mock_deriva_ampliada.csv`

This version uses a strain drift of **0.80 microstrain/s**, providing a visible discrepancy for discussion in Example 3.

The original low-drift version uses **0.025 microstrain/s**. Its results differ, so identify the dataset used in any report.

**Key lesson:** smoothing can attenuate a genuine peak as well as unwanted fluctuations.

## Example 3 — Comparison with beam theory

This example compares the plateau measurements with the elastic prediction for a rectangular cantilever beam.

| Parameter | Value |
|---|---|
| Beam length | 300 mm |
| Width | 30 mm |
| Thickness in the bending direction | 10 mm |
| Strain-gauge position from the clamp | 20 mm |
| Reference Young’s modulus | 70 GPa |

For a transverse tip load, the predicted surface strain at the gauge position is:

\[
\varepsilon =
\frac{F(L-x_g)c}{EI}
=
\frac{6F(L-x_g)}{Ebh^2}
\]

The corresponding theoretical sensitivity is **8 microstrain/N**.

Students estimate an apparent Young’s modulus from the strain–force relationship and examine residuals against both force and time.

An optional correction assumes a linear drift between confirmed unloaded plateaus. The example then compares the results before and after correction.

### Expected results with the amplified-drift dataset

| Quantity | Before drift correction | After assumed correction |
|---|---:|---:|
| Apparent Young’s modulus | Approximately 62.55 GPa | Approximately 70.00 GPa |
| RMSE against theory | Approximately 62.78 microstrain | Approximately 0.0055 microstrain |

These results reflect the construction of the synthetic dataset. They do not demonstrate equivalent accuracy in a real experiment.

**Key lesson:** agreement with theory depends on the measurement, the geometry, and the assumptions behind the correction.

## Interpretation and limitations

- Preserve the original data and document every transformation.
- An automatically detected outlier is a candidate for investigation, not a confirmed error.
- Interpolated values are estimates rather than new measurements.
- Temporal standard deviation is not the total measurement uncertainty.
- Loading plateaus from one test are not independent experimental replicates.
- Linear drift correction assumes that the unloaded baseline changes linearly and that no permanent strain remains.
- High-pass filtering can remove part of the static strain response and is not the default drift correction for these examples.
- Excellent agreement with a model used to generate synthetic data is not independent model validation.

## Language

This README is written in English for wider accessibility. The MATLAB teaching material is primarily written in Portuguese.

## License

Refer to the repository’s `LICENSE` file for reuse conditions, if one is provided.
