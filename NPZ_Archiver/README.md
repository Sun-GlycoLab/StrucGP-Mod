This software is used to generate the npz files required for StrucGP-Mod to run on Linux systems
# MS Data to NPZ Converter 

## Overview

**Name:** MS Data to NPZ Converter  
**Platforms:** Windows   
**Implementation:** Python


## System requirements

### Operating system and software

- **Windows:** Run the graphical interface through `main_NPZ.exe`.

## Installation guide

### Windows

No conventional installation is required. Extract the software package and run `main_NPZ.exe`.

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


### Windows version

1. Run `main_window.exe`. The graphical interface and a command console will open.

2. In **Database**, select the following input files. For detailed information, refer to Figure Step 1:

   - MS files directory, for example `test.raw`
   - Protein database directory, for example `fetuin.fasta`
   - Glycan branch structure database directory, for example `19.xlsx`

3. In **Parameter**, configure the general search parameters according to the experimental data. For detailed information, refer to Figure Step 2.

4. In **Glycan Modifications**, select the glycan modifications to be identified. If a required modification is not listed, add it by specifying its name, attachment site of monosaccharide, and modification mass. After configuration, click **Save `param.json`** in the lower-right corner. For detailed information, refer to Figure Step 3.

5. In **Running**, configure the remaining identification parameters and click "Run Analysis" to start the identification workflow For detailed information, refer to Figure Step 4.

6. To run the screening workflow instead, set all glycan modifications in **Glycan Modifications** to `false`, return to **Database**, and click "Checking" at the bottom of the page.

## Demo

### Windows demo

1. Run `main_window.exe`.
2. In Step 1, select `test.raw`, the supplied fetuin FASTA file, and the supplied `19.xlsx` glycan branch database.
3. Complete the parameter settings described above.
4. Choose screening or identification mode and start the analysis.


### Expected output


## Notes

1. This tool is freely available under an academic license for academic research, non-commercial use, and educational purposes.

## Contact

If you have any questions or suggestions, contact `sun_glycolab@126.com` or open an issue through the Sun-GlycoLab GitHub organization.
