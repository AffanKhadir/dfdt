# dfdt
Measure a linear drift rate for a fast radio burst using a 2D auto-correlation analysis and Monte Carlo resampling.

Created by Ziggy Pleunis, with contributions from Alex Josephy and Deborah Good.

Feel free to open an issue on GitHub or email ziggy.pleunis@physics.mcgill.ca with questions or comments.

This method has been developed to analyze CHIME/FRB bursts, and it has been used in, e.g.:

https://ui.adsabs.harvard.edu/abs/2019ApJ...885L..24C/abstract

https://ui.adsabs.harvard.edu/abs/2020ApJ...891L...6F/abstract

## Method

See the `docs` directory for a brief explanation of the method.

## Usage

The `data` directory contains a dedispersed waterfall from repeating sources of FRBs 180916.J0158+65 as detected by CHIME/FRB. Running the `ac_mc_drift.py` script on the pipeline (i.e., `python ac_md_drift.py`) measures the drift rate in this burst, using these lines:
```python

fname = "./data/23891929_DM348.8_waterfall.npy"
dedispersed_intensity = np.load(fname)

# burst parameters
dm_uncertainty = 0.2  # pc cm-3
source = "R3"
eventid = "23891929"

# instrument parameters
dt_s = 0.00098304
df_mhz = 0.0244140625
nchan = 16384
freq_bottom_mhz = 400.1953125
freq_top_mhz = 800.1953125

ds = DynamicSpectrum(dt_s, df_mhz, nchan, freq_bottom_mhz, freq_top_mhz)

constrained, dfdt_data, dfdt_mc, dfdt_mc_low, dfdt_mc_hihg = ac_mc_drift(
    dedispersed_intensity, dm_uncertainty, source, eventid, ds,
    dm_trials=100, mc_trials=100
)
```
The output of a run, two diagnostic figures and a `.npz` file with the results, with 100x100=10,000 Monte Carlo trials is saved into the `results` directory.

The `dm_uncertainty` is the standard deviation of the normal distribution where dispersion measure resamples are drawn from. The number of dispersion measure resamples are set by `dm_trials` and the number of noise resamples are set by `mc_trials`. You can also set the downsampling factor in frequency with `sub_factor` (64 by default), the required `detection_confidence` (99.73 by default) for a measurement, the confidence interval for the uncertainty region `uncertainty_confidence` (68.0 by default). 
