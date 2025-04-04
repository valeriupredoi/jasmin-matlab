## MATLAB installation on JASMIN

### Install

- source: `/apps/jasmin/community/matlab/matlab_R2024b`
- install targget: `/apps/jasmin/community/matlab/MATLAB`
- install procedure:
  - install with flags, using `installer_input_local.txt` configuration file
  - install command:
```
source install.sh -inputFile installer_input_local.txt
```
  - needs:
    - a valid license file via option `fileInstallationKey=xxxxx-xxxxx-...
  - logs in `/apps/jasmin/community/matlab/MATLAB/log`