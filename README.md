# NexusMaxEnt™

A maximum-entropy processing tool for 2D-DOSY NMR reconstruction.

---

## Features (Promotional CPU Version)
- **Advanced NMR Data Processing**: Powered by algorithms inspired by PALMA, GIFA, and Classic MaxEnt techniques.
- **ADMM Mirror CPU Version**: Includes a freely redistributable CPU-based optimization module for evaluation and promotional purposes. *(Note: Other proprietary CPU and GPU algorithms are excluded).*
- **Standalone Execution**: Runs as a portable desktop application without requiring external Python setup.

---

## System Requirements
- **OS**: Windows 11 (64-bit recommended)
- **Runtime**: Self-contained executable

---

## How to Use & Data Preparation

### 1. Data Processing in Bruker TopSpin
Designed for **1H DOSY NMR data** measured using the **bpp-ste-led** sequence (e.g., `ledbpgp2s`).
- Ensure proper phase correction (e.g., `apk`) is completed in 1D/2D before export.
- Set F2 size (`SI`) to approx 8192 points.
- Set F1 size (`SI`) to match F1 `TD`.
- Execute **`xf2`** and **`abs2`** to generate the **`2rr`** file.

### 2. Required Experimental Parameters
Automatically extracts: **`d20`**, **`p30`**, **`d16`**, and **`difflist`**. *(Manual fallback input available if retrieval fails).*

### 3. Running the Application
1. Extract the downloaded ZIP archive.
2. Double-click the `.exe` file to launch.
3. Load your prepared TopSpin dataset directory (`2rr` file and parameters).

---

### 4. Important Note on b-value Calculation & Pulse Sequences
This tool calculates the diffusion gradient b-value based on the **bpp-ste-led** sequence (e.g., `ledbpgp2s`):

`b_value = gamma^2 * difflist^2 * 4 * p30^2 * (d20 - 2*p30/3 - d16/2)`

- **`p30`**: Defined as the duration of a **single PFG pulse** in bipolar gradient pairs (Little Delta / 2).
- **`d20`**: Big Delta (Diffusion time in seconds).
- **`d16`**: Gradient recovery delay in seconds.

*Note for Non-bipolar Sequences (e.g., standard STE):*
If you are processing datasets acquired with non-bipolar gradient sequences, please take extra care when specifying `p30` and `d16` (gradient recovery delay), as the definition of Little Delta differs.

---

## Input Data Format (`.npz`)

Supports **NumPy compressed archive (`.npz`)** files for direct data ingestion.

### Input `.npz` Structure (Keys)
The input `.npz` file must contain the following specific keys:
- **`data`**: 2D Array, shape `(N_f1, N_f2)` (The F1 x F2 intensity matrix, e.g., 2rr data)
- **`axis_f1`**: 1D Array, shape `(N_f1,)` (F1 axis values)
- **`axis_f2`**: 1D Array, shape `(N_f2,)` (F2 chemical shifts in ppm)
- **`difflist`**: 1D Array, shape `(N_f1,)` (Gradient strength list)
- **`d20`**: Float (Big delta in seconds)
- **`p30`**: Float (Little delta in microseconds)
- **`d16`**: Float (Gradient recovery delay in seconds)

*Note: Custom `.npz` files can easily be generated using basic Python scripts or generative AI.*

---

## Output Data Format (Analyzed Results)

Exported results (`nexus_maxent_result.npz`) strictly follow this structure and can be loaded using the corresponding keys:

### Output `.npz` Structure (Keys)
- **`dosy_matrix`**: 2D Array, shape `(N_diff, N_f2)` (Reconstructed intensity matrix)
- **`lap_axis`**: 1D Array, shape `(N_diff,)` (Diffusion coefficients in m^2/s)
- **`axis_f2`**: 1D Array, shape `(N_f2,)` (F2 chemical shifts in ppm)
- **`d20`**, **`p30`**, **`d16`**: Floats (Inherited experimental parameters)

### Custom Visualization & Viewer Development
Because the output is saved in a standardized `.npz` format, you are completely free to build custom visualization scripts or interactive viewer applications tailored to your specific research needs using standard Python libraries (such as NumPy, Matplotlib, Plotly, or PyQt) or with the assistance of generative AI tools.

---

## License & Redistribution Terms
Provided under a custom proprietary license.
- **Free Component**: The "ADMM Mirror CPU version" component is available for free use and free original binary redistribution.
- **Restrictions**: Resale, reverse engineering, and independent redistribution of other proprietary modules are strictly prohibited.
- For full details, refer to **`LICENSE.txt`**.

---

## Scientific Background & Acknowledgments

### 1. Pioneering Works & Literature
Acknowledges Dr. Marc-André Delsuc and the developers of **GIFA** and **PALMA**, as well as Dr. John Skilling for foundational Maximum Entropy methods. Full citations are available in **`ACKNOWLEDGMENTS.txt`**.

### 2. Open-Source Ecosystem
Built upon **Python**, **NumPy**, **SciPy**, **CuPy**, **Matplotlib**, **Cython**, **PyInstaller**, and **Miniconda**. Refer to **`ACKNOWLEDGMENTS.txt`** for details.

### 3. Technical Support
Developed with technical assistance from generative AI tools, including Google Gemini.