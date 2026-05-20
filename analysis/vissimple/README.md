### Quick check: Text output (ASCII)

For quick inspection and debugging, ASCII output can be enabled in
`config.f90`:
```fortran
logical,parameter:: asciiout = .true. !! Ascii-files are additionaly damped.
```

The data is dumped as `ascdata/snap###-?????.csv`. Here `###` is the MPI
process and `?????` is the snapshot number. The format is
`x y d vx vy p phi X1`. `gnuplot` is useful for a quick check:
```bash
gnuplot
set view map
splot "ascdata/snap###-?????.csv" u 1:2:8 w pm3d
```

### 2. Sample data

This directory contains two sample sets:

- `2Dsample/`
  A single-rank 2D-style sample. It contains
  `snap000-00001.csv` and `snap000-00101.csv`, together with
  `initial.png` and `final.png`.
- `3Dsample/`
  A multi-rank sample prepared from four MPI processes. It contains
  `snap000` to `snap003` for the initial and final snapshots, plus
  `initial.png` and `final.png`.

Since the production runs can include random perturbations, your turbulence
pattern does not have to match the samples exactly. The samples are mainly
for checking that the ASCII output format and the large-scale structure look
reasonable.

### 3. `plotasc.plt`

Both `2Dsample/` and `3Dsample/` include a `plotasc.plt` script for
`gnuplot`. The script reads the CSV files and generates `initial.png` and
`final.png` from column 8 (`X1`):
```gnuplot
set view map
set cbrange [0:1]
splot ... u 1:2:8 w pm3d
```

Run it like this:
```bash
cd analysis/vissimple/2Dsample
gnuplot plotasc.plt

cd ../3Dsample
gnuplot plotasc.plt
```

The main parameters in the script are:

- `dir`
  Directory containing the CSV files. The default is `./`.
- `snap_start`, `snap_end`
  MPI rank range to plot. `2Dsample` uses `0:0`, while `3Dsample` uses `0:3`.
- `step_init`, `step_final`
  Snapshot numbers for the initial and final states. The sample scripts use
  `1` and `101`.
- `pngflag`
  When set to `1`, the script writes PNG files.

If you want to use the script for your own run, copy `plotasc.plt` into the
directory containing `snap###-?????.csv` and adjust the rank range and
snapshot numbers as needed.

<img width="320" height="240" alt="initial" src="https://github.com/user-attachments/assets/90d21b9e-e3f7-4606-b425-8d4d8715aab3" />
<img width="320" height="240" alt="final" src="https://github.com/user-attachments/assets/04091023-8ab7-4ce6-a261-237f9bdf4f61" />
<img width="320" height="240" alt="final" src="https://github.com/user-attachments/assets/01ecbb59-c21b-4e38-8e6a-78d9e7433791" />

