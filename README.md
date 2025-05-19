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
    - a valid File Installation Key (FIK) via option `fileInstallationKey=xxxxx-xxxxx-...
    - a valid license file via option ``
  - logs in `/apps/jasmin/community/matlab/MATLAB/log`

Make sure to set `$TMPDIR` to point to a writable dir eg:

```
export TMPDIR=/apps/jasmin/community/matlab/MATLAB/tmp
```

where install logs are written.

The install script detects the OS architecture (`glnxa64` for us)),
so the `install.sh` invocation boils down to a call to a simple executable:

```
bin/glnxa64/MathWorksProductInstaller -inputFile installer_input_local.txt
```

which, unfortunately, throws a SegFault 100% times. This is due to a missing X library (have no clue which one though).
Even though one tells Matlab to install via an installation file (so not via a popup window), it still tries to
open a wretched window!

As per https://uk.mathworks.com/matlabcentral/answers/1636600-install-matlab-on-linux-without-gui it appears others are saying one
still needs a bloody GUI. `enableLNU=no` doesn't fix it.

This page https://www.mathworks.com/help/install/ug/install-noninteractively-silent-installation.html doesn't load anymore BTW.
