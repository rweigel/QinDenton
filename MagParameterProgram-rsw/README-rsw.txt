# Use
The Makefile in this directory automates the process of updating the Qin/Denton dataset, which requires downloading OMNI data files that are read by the program.

Note that creation of the 5-minute dataset requires a machine with at least 48 GB of RAM because it reads the full time span of the needed OMNI parameters into memory. One could modify the code to generate one QinDenton data file per year, but one has to account for the fact that there would be an amount of time at the start of the year in which the parameters are inaccurate because the code generates parameters based on a past time window of data.

If `gfortran` is available, executing

```
make allhour
make all5min
make all1min
```

should result in zip files of the Qin/Denton parameters along with the OMNI data that was used as input in `/tmp/QinDenton`.

# Notes

MagmodelinputmodelONE.f in `MagmodelinputmodelONE-rsw` is a modified versions (by rsw) of the original code in `MagParameterProgram`. The code was modified to allow non-interactive execution. The differences between the original and this version are shown in `MagmodelinputmodelONE-rsw/diffs/MagmodelinputmodelONE.f.diff`.

On 4/20/2009, Richard Denton provided `MagmodelinputmodelONE-rsw/MagmodelinputONE_corrected.f/, a slightly modified version of `MagmodelinputmodelONE/MagmodelinputONE.f` that only affects the iG2 variable for hourly data. The difference bewteen `MagmodelinputONE.f` and `MagmodelinputONE_corrected.f` are shown in `MagmodelinputmodelONE-rsw/diffs/MagmodelinputONE_corrected.f.diff`.

On 10/24/2013, Richard Denton provided `MagmodelinputmodelONE-rsw/MagmodelinputONE_corrected2.f`, which is a modified version of `MagmodelinputmodelONE-rsw/MagmodelinputONE_corrected.f` that allows the code to output data past the year 2013. The difference bewteen the `MagmodelinputmodelONE-rsw/MagmodelinputONE_corrected.f` and `MagmodelinputmodelONE-rsw/MagmodelinputONE_corrected2.f` are shown in `MagmodelinputmodelONE-rsw/diffs/MagmodelinputONE_corrected2.f.diff`.



