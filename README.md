# StrucGP-Mod
StrucGP-Mod is an automated two-stage tool for screening and identifying glycan modifications. It traces monosaccharide-loss paths from diagnostic B ions and uses KDE calibration to discover rare glycan features without databases, then identifies dozens of predefined modifications using characteristic fragments and stratified FDR control.

## Overview

**Name:** StrucGP-Mod  
**Platforms:** Windows and Linux  
**Main modules:** modification screening and modification identification  
**Implementation:** Python

StrucGP-Mod is designed for screening potential glycan modifications and subsequently identifying user-defined glycan modifications. The software is provided in both Windows and Linux versions. The Linux version is recommended for large datasets because it generally offers faster processing and more flexible CPU parallelization.

## System requirements

### Operating system and software

- **Windows:** Run the graphical interface through `main_window.exe`.
- **Linux:** Run the command-line version through `main_linux.py` in the Python environment supplied or specified with the software package.

### Hardware

Hardware requirements depend on the size and number of MS files. For large datasets, a multi-core CPU and sufficient RAM are recommended. In the Linux version, the number of CPU cores used by the identification module can be controlled with the `Threads` parameter.

## Installation guide

### Windows

No conventional installation is required. Extract the software package and run `main_window.exe`.

### Linux

Extract the software package, activate the required Python environment, and run `main_linux.py` from the program directory.

Keep the license file in the program directory, or provide its path explicitly when starting the program.

## How to acquire and activate a StrucGP-Mod license

1. Obtain the physical address (MAC address) of the computer on which StrucGP-Mod will be used.

   On Windows, open Command Prompt and run:

   ```text
   ipconfig /all
   ```

   Under the active network adapter, locate the 12-character hexadecimal value shown after **Physical Address**.

   On Linux, run:

   ```bash
   ip link
   ```

2. Copy the MAC address and complete the user-information form below.

3. Send the completed information to `sun_glycolab@126.com`. A StrucGP-Mod license file will be provided after the request is processed.

4. Save the returned license file as `StrucGP-Mod_license.lic`. For the Windows version, place it in the same directory as `main_window.exe`. For the Linux version, place it in the program directory or pass its path with `--license`.

5. StrucGP-Mod is freely available under an academic license for academic research, non-commercial use, and educational purposes.

6. If license activation fails, verify both the license filename and its path. For further assistance, contact `sun_glycolab@126.com`.

```text
#####################################################################

MAC:

User name:

Email address:

University or organization:

Country or region:

Principal investigator or supervisor:

Principal investigator's or supervisor's email:

#####################################################################
```

## Instructions for use

StrucGP-Mod contains two workflows:

- **Screening mode:** screens MS data for potential special or unknown glycan modifications.
- **Identification mode:** identifies selected glycan modifications using user-defined modification settings.

### Windows version

1. Run `main_window.exe`. The graphical interface and a command console will open.

2. In **Step 1**, select the following input files:

   - MS data file, for example `test.raw`
   - protein sequence database, for example `fetuin.fasta`
   - glycan branch database, for example `19.xlsx`

3. In **Step 2**, configure the general search parameters according to the experimental data.

4. In **Step 3**, select the glycan modifications to be identified. If a required modification is not listed, add it by specifying its name, target monosaccharide, and monoisotopic mass. After configuration, click **Save `param.json`** in the lower-right corner.

5. In **Step 4**, configure the remaining identification parameters and start the identification workflow.

6. To run the screening workflow instead, set all glycan modifications in Step 3 to `False`, return to Step 1, and click the screening button at the bottom of the page.

### Linux version

The Linux version is operated from the command line. The same command-line workflow may also be run from Windows Command Prompt if the required Python environment is available.

#### Screening mode

1. Open `parameter.json` and update at least the following fields:

   | Parameter | Description |
   | --- | --- |
   | `MsFile` | One or more MS data paths |
   | `ProteinDatabase` | Protein FASTA database path |
   | `B_ionsDatabase` | Glycan branch database path |
   | `HighEnergy` | High-energy collision setting(s) |
   | `LowEnergy` | Low-energy collision setting |
   | `Label` | Quantification label; use `null` for no label or, for example, `"TMT10plex"` for TMT10 |
   | `glycan_modification` | Must be an empty list (`[]`) in screening mode |

2. Keep the remaining search parameters consistent with the experimental method. A minimal excerpt is shown below; the complete distributed parameter file contains additional required settings.

   ```json
   {
     "MsFile": [
       "/path/to/test.raw"
     ],
     "ProteinDatabase": "/path/to/fetuin.fasta",
     "B_ionsDatabase": "/path/to/19.xlsx",
     "Threads": 1,
     "HighEnergy": [
       33
     ],
     "LowEnergy": 20,
     "Label": null,
     "glycan_modification": []
   }
   ```

3. Start the screening workflow:

   ```bash
   python main_linux.py \
     --license ./StrucGP-Mod_license.lic \
     --check \
     --check-param parameter.json
   ```

The filename passed to `--check-param` must exactly match the parameter filename, including letter case on Linux.

#### Identification mode

1. Open `Param.json` and update the MS-data path, protein database, glycan branch database, collision energies, labeling method, and other search parameters.

2. Set `Threads` to the number of CPU cores that the program may use.

3. Configure `glycan_modification`. The number of modification types is not limited by the workflow. Each modification entry uses the following fields:

   | Field | Description |
   | --- | --- |
   | `Name` | Modification name; English letters and numbers are supported |
   | `Glycan_it_modify` | Monosaccharide site or sites on which the modification can occur; follow the multi-site syntax used by the O-acetylation example in the distributed `Param.json` |
   | `Modification_masses` | Monoisotopic mass shift of the modification |
   | `Max_mod_per_glycan` | Maximum number of this modification allowed on one glycan |
   | `enabled` | `true` includes the modification in identification; `false` excludes it |

4. The example below enables phosphorylation and leaves the sodium-adduct entry disabled:

   ```json
   {
     "glycan_modification": [
       {
         "Name": "Phospho",
         "Glycan_it_modify": "H",
         "Modification_masses": 79.9663,
         "Max_mod_per_glycan": 1,
         "enabled": true
       },
       {
         "Name": "Na",
         "Glycan_it_modify": "H",
         "Modification_masses": 21.9819,
         "Max_mod_per_glycan": 1,
         "enabled": false
       }
     ]
   }
   ```

5. The distributed parameter file contains 12 common modification presets, which are disabled by default. Change only the desired entries to `true`. If every entry is `false`, StrucGP-Mod performs a conventional identification without considering glycan modifications.

6. Save the completed file as `Param.json` in the location expected by the program, and start the identification workflow:

   ```bash
   python main_linux.py --license ./StrucGP-Mod_license.lic
   ```

   The supplied guide treats `Param.json` as the default identification configuration and does not specify a separate identification-mode parameter flag. If your software build uses an explicit parameter-file option, confirm the supported syntax with `python main_linux.py --help`.

## Important parameter notes

- `Label` must be valid JSON: use `null` when no quantitative label is applied, or a quoted label name such as `"TMT10plex"` when appropriate.
- `Threads` specifies the number of CPU cores used in the Linux identification workflow.
- `HighEnergy` and `LowEnergy` must match the collision-energy settings used to acquire the MS data.
- `MS1_mass_tolerance` and `MS2_mass_tolerance` control precursor- and fragment-mass tolerances, respectively.
- `Oxo_top_n`, `Oxo_ess_n`, `Oxo_ess_mz`, and `Oxo_noness_mz` control oxonium-ion-based spectrum filtering.
- `Top_n` specifies the number of high-energy spectral peaks retained for subsequent identification.
- `VariableModification` and `FixedModification` define peptide modifications.
- `Peptide_FDR_cutoff` and `Glycan_FDR_cutoff` control peptide- and glycan-level false-discovery-rate thresholds.
- `Protease`, `Protease_terminal`, `Cleavage_sites`, and `missed_cleavage` define protein digestion rules.
- In screening mode, do not add any entries to `glycan_modification`.
- In identification mode, custom modifications must follow the exact field names, capitalization, value types, and punctuation used in the distributed JSON file.

## Demo

### Windows demo

1. Run `main_window.exe`.
2. In Step 1, select `test.raw`, the supplied fetuin FASTA file, and the supplied `19.xlsx` glycan branch database.
3. Complete the parameter settings described above.
4. Choose screening or identification mode and start the analysis.

### Linux screening demo

1. Copy the distributed parameter template and set `MsFile`, `ProteinDatabase`, and `B_ionsDatabase` to the paths of the demo files.
2. Set `glycan_modification` to `[]`.
3. Run the screening command shown above.

### Expected output

Progress information is displayed in the console during processing. Result and intermediate files are written to the configured output or MS-data directory, depending on the software build. Consult the release package for the exact output filenames associated with screening and identification modes.

## Notes

1. Linux is recommended for faster processing, especially for large datasets.
2. Screening and identification are separate workflows. Screening requires an empty modification list; identification requires explicit activation of the desired modifications.
3. A custom modification is considered only when its `enabled` field is set to `true`.
4. JSON Boolean and null values must be written in lowercase: `true`, `false`, and `null`.
5. Use forward slashes or properly escaped backslashes in JSON file paths. Invalid JSON syntax will prevent the parameter file from loading.
6. Keep the parameter filename used in the command consistent with the actual file on disk.
7. StrucGP-Mod is freely available under an academic license for academic research, non-commercial use, and educational purposes.

## Contact

If you have any questions or suggestions, contact `sun_glycolab@126.com` or open an issue through the Sun-GlycoLab GitHub organization.
