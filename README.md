# StrucGP-Mod

## Overview

**Name:** StrucGP-Mod  
**Platforms:** Windows and Linux  
**Main modules:** modification screening and modification identification  
**Implementation:** Python

StrucGP-Mod is designed for screening potential glycan modifications and subsequently identifying user-defined glycan modifications. The software is provided in both Windows and Linux versions. The Linux version is recommended for large datasets because it generally offers faster processing and more flexible CPU parallelization.

## System requirements

### Operating system and software

- **Windows:** Run the graphical interface through `StrucGP-Mod-Windows.exe`.
- **Linux:** Run the command-line version through `main_linux` in the Python environment supplied or specified with the software package.

### Hardware

Hardware requirements depend on the size and number of MS files. For large datasets, a multi-core CPU and sufficient RAM are recommended. In the Linux version, the number of CPU cores used by the identification module can be controlled with the `Threads` parameter.

## Installation guide

### Windows

No conventional installation is required. Extract the software package and run `StrucGP-Mod-Windows.exe`.

### Linux

Activate the Python environment, and run `main_linux` from the program directory.

Keep the license file in the program directory, or provide its path explicitly when starting the program.

## How to acquire and activate a StrucGP-Mod license

1\. You can input 'ipconfig/all' in your command prompt window(CMD) on Windows platform. Under "Ethernet adapter" you may find a string consisting of 12 hexadecimal digits after the "Physical Address", which is your device's media access control address(MAC Address).

2\. After getting your MAC address, you need to copy it next to "MAC" between the "####" chars.

3\. Besides, in order to get closer contact with users. You need to fill all below items between the "####" chars. You can just send this README txt file to sun\_glycolab@126.com after finish it and we will provide StrucOGP license to you as soon as possible.

4\. You will find a LIC file named StrucGP-Mod\_license in our reply. Please put it in the same folder as the main.exe. The wrong path or license file may cause failure when you start program.

5\. StrucGP-Mod is available freely for academic research, non-commercial or educational purposes under academic license.

6\. If there are any other errors when you activate or use StrucGP-Mod, Please contact with us via sun\_glycolab@126.com.

\#####################################################################

MAC:

User Name:

Your e-mail address:

University or Organization name:

Country or Region:

Your Leader or Supervisor:

Your leader's e-mail:

\#####################################################################

## Instructions for use

StrucGP-Mod contains two workflows:

- **Screening mode:** screens MS data for potential special or unknown glycan modifications.
- **Identification mode:** identifies selected glycan modifications using user-defined modification settings.

### Windows version

1. Run `StrucGP-Mod-Windows.exe`. The graphical interface and a command console will open.

2. In **Database**, select the following input files. For detailed information, refer to Figure Step 1:

   - MS files directory, for example `test.raw`
   - Protein database directory, for example `fetuin.fasta`
   - Glycan branch structure database directory, for example `19.xlsx`

3. In **Parameter**, configure the general search parameters according to the experimental data. For detailed information, refer to Figure Step 2.

4. In **Glycan Modifications**, select the glycan modifications to be identified. If a required modification is not listed, add it by specifying its name, attachment site of monosaccharide, and modification mass. After configuration, click **Save `param.json`** in the lower-right corner. For detailed information, refer to Figure Step 3.

5. In **Running**, configure the remaining identification parameters and click "Run Analysis" to start the identification workflow For detailed information, refer to Figure Step 4.

6. To run the screening workflow instead, set all glycan modifications in **Glycan Modifications** to `false`, return to **Database**, and click "Checking" at the bottom of the page.

![Step 1.png](docs/images/Step%201.png)
![Step 2.png](docs/images/Step%202.png)
![Step 3.png](docs/images/Step%203.png)
![Step 4.png](docs/images/Step%204.png)

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

2. Keep the remaining search parameters consistent with the experimental method. A excerpt is shown below; the complete distributed parameter file contains additional required settings.

   ```json
   {
     "MsFile": [
       "D:\\test\\test.npz"
     ],
     "ProteinDatabase": "D:\\test\\Mouse_reviewed_2026_07_03.fasta",
     "B_ionsDatabase": "D:\\test\\branch_structures_19_20260602.xlsx",
     "Threads": 1,
     "HighEnergy": [
       33
     ],
     "LowEnergy": 20,
     "Label": null,
     "MS1_mass_tolerance": 10,
     "MS2_mass_tolerance": 20,
     "Oxo_top_n": 20,
     "Oxo_ess_n": 2,
     "Oxo_ess_mz": "138.0550,204.0866",
     "Oxo_noness_mz": "145.0495,163.0601,168.0655,186.0761,274.0927,292.1032,325.1135,366.14",
     "Top_n": 300,
     "VariableModification": "M,Oxidation,any",
     "FixedModification": "C,Carbamidomethyl,any",
     "min_pepfrag_number": 5,
     "iTRAQ_reporters": "114.1112,115.1083,116.1116,117.1150",
     "TMT_reporters": "126.127725,127.124760,127.131079,128.128114,128.134433,129.131468,129.137787,130.134822,130.141141,131.138176",
     "missed_cleavage": 2,
     "Peptide_FDR_cutoff": 0.01,
     "Glycan_FDR_cutoff": 0.01,
     "Protease": "Trypsin",
     "Protease_terminal": "C-",
     "Cleavage_sites": "KR",
     "Search for special branches": true,
     "Search for special branches with high energy": true,
     "Search for special branches with low energy": true,
     "Search for special branches with high and low energy": true,
     "glycan_modification": []
   }
   ```

3. Start the screening workflow:

   ```bash
   python main_linux --license ./StrucGP-Mod_license.lic --check --check-param ./parameter.json
   ```

The filename passed to `--check-param` must exactly match the parameter filename, including letter case on Linux.

#### Identification mode

1. Open `param.json` and update the MS-data path, protein database, glycan branch database, collision energies, labeling method, and other search parameters. To avoid potential compatibility and deployment issues with vendor-specific RAW-file reading dependencies in CentOS-based HPC environment, first convert the RAW files to NPZ format on a Windows computer using our in-house mainnpz.exe utility. Then transfer the resulting NPZ files to the HPC system and specify their paths as the MS-data input for StrucGP-Mod analysis. Detailed step-by-step instructions are provided in the READMENPZ.md file.

2. Set `Threads` to the number of CPU cores that the program may use.

3. Configure `glycan_modification`. The number of modification types is not limited by the workflow. Each modification entry uses the following fields:

   | Field | Description                                                                                                                                                            |
   | --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `Name` | Modification name; English letters and numbers are supported                                                                                                           |
   | `Glycan_it_modify` | Monosaccharide site or sites on which the modification can occur; follow the multi-site syntax used by the O-acetylation (OAc) example in the distributed `Param.json` |
   | `Modification_masses` | Masses of the special modifications or rare monosaccharides                                                                                                            |
   | `Max_mod_per_glycan` | Maximum number of this modification allowed on one glycan                                                                                                              |
   | `enabled` | `true` includes the modification in identification; `false` excludes it                                                                                                |

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
         "Name": "OAc",
         "Glycan_it_modify": "S",
         "Modification_masses": 42.0106,
         "Max_mod_per_glycan": 1,
         "enabled": false
       },
       {
         "Name": "OAc",
         "Glycan_it_modify": "G",
         "Modification_masses": 42.0106,
         "Max_mod_per_glycan": 1,
         "enabled": false
       }
     ]
   }
   ```

5. The parameter file contains 12 common modification presets, which are disabled by default. Change only the desired entries to `true`. If every entry is `false`, StrucGP-Mod performs a conventional identification without considering glycan modifications.

6. Save the completed file as `param.json` in the location expected by the program, and start the identification workflow:

   ```bash
   python main_linux --license ./StrucGP-Mod_license.lic --param ./param.json
   ```

   The supplied guide treats `param.json` as the default identification configuration and does not specify a separate identification-mode parameter flag. If your software build uses an explicit parameter-file option, confirm the supported syntax with `python main_linux.py --help`.

## Important parameter notes

- `Label` must be valid JSON: use `null` when no quantitative label is applied, or a quoted label name such as `"TMT10plex"` when appropriate.
- `Threads` specifies the number of CPU cores used in the Linux identification workflow.
- `HighEnergy` and `LowEnergy` must match the collision-energy settings used to acquire the MS data.
- `MS1_mass_tolerance` and `MS2_mass_tolerance` control precursor- and fragment-mass tolerances, respectively.
- `VariableModification` and `FixedModification` define peptide modifications.
- `Peptide_FDR_cutoff` and `Glycan_FDR_cutoff` control peptide- and glycan-level false-discovery-rate thresholds.
- `Protease`, `Protease_terminal`, `Cleavage_sites`, and `missed_cleavage` define protein digestion rules.
- In screening mode, do not add any entries to `glycan_modification`.
- In identification mode, custom modifications must follow the exact field names, value types, and punctuation used in the distributed JSON file.

## Demo

### Windows demo

1. Run `StrucGP-Mod-Windows.exe`.
2. In Step 1, select `test.raw`, the supplied fetuin FASTA file, and the supplied `19.xlsx` glycan branch database.
3. Complete the parameter settings described above.
4. Choose screening or identification mode and start the analysis.

### Linux Screening demo

1. Copy or Click the `parameter.json` file and modify `MsFile`, `ProteinDatabase`, and `B_ionsDatabase` to specify the paths of the demo files.
2. Set `glycan_modification` to `[]`.
3. Run the screening command shown above.

### Linux Identifying demo

1. Copy or Click the `param.json` file and modify `MsFile`, `ProteinDatabase`, and `B_ionsDatabase` to specify the paths of the demo files.
2. Configure the desired modifications in `glycan_modification` by following the steps described above.
3. Run the identifying command shown above.

### Expected output
Windows:

Screening Mode:

test_nuclear_density_analysis_result.xlsx (result file) and test_possible_ions_and_all_paths (result file), are saved in the directory of test.raw.

test_raw.pkl is the file generated after spectrum reading and preprocessing of the raw file.

Identifying Mode:

test_result.xlsx (result file), test.csv (file used for visualization) and test_result_AllSpectra.xlsx (log file), are saved in the directory of test.raw.

test_raw.pkl is the file generated after spectrum reading and preprocessing of the raw file.

Linux:

Screening Mode:

test_nuclear_density_analysis_result.xlsx (result file) and test_possible_ions_and_all_paths (result file), are saved in the directory of test.npz.

test_raw.npz is the file generated after spectrum reading and preprocessing of the raw file.

Identifying Mode:

test_result.xlsx (result file), test.csv (file used for visualization) and test_result_AllSpectra.xlsx (log file), are saved in the directory of test.npz.

test_raw.npz is the file generated after spectrum reading and preprocessing of the raw file.

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
