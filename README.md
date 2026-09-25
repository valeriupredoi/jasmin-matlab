## MATLAB installation on JASMIN

### Group Workspace on JASMIN

- GWS ``rdg_climate_matlab``
- Apply at <https://accounts.jasmin.ac.uk/services/additional_services/rdg_climate_matlab/>

### Working Version

**UPDATE 30 May 2025**

James has managed to install it, using a mounted ISO!
Very many thanks to James Hannah!

- location: `/apps/jasmin/community/matlab/app/bin`
- start: `$ ./matlab -nodisplay`
- packages installed:

```
[valeriu@sci-vm-01 bin]$ ./matlab -nodisplay

                                                                     < M A T L A B (R) >
                                                           Copyright 1984-2024 The MathWorks, Inc.
                                                      R2024b Update 5 (24.2.0.2863752) 64-bit (glnxa64)
                                                                      January 31, 2025

 
To get started, type doc.
For product information, visit www.mathworks.com.

>> 2+2

ans =

     4

>> ver
---------------------------------------------------------------------------------------------------------------
MATLAB Version: 24.2.0.2863752 (R2024b) Update 5
MATLAB License Number: 1089045
Operating System: Linux 5.14.0-503.38.1.el9_5.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Apr 16 16:38:39 UTC 2025 x86_64
Java Version: Java 1.8.0_202-b08 with Oracle Corporation Java HotSpot(TM) 64-Bit Server VM mixed mode
---------------------------------------------------------------------------------------------------------------
MATLAB                                                Version 24.2        (R2024b)
Curve Fitting Toolbox                                 Version 24.2        (R2024b)
Image Processing Toolbox                              Version 24.2        (R2024b)
Mapping Toolbox                                       Version 24.2        (R2024b)
Optimization Toolbox                                  Version 24.2        (R2024b)
Statistics and Machine Learning Toolbox               Version 24.2        (R2024b)
>>
```

### Renew license (with new lic file)

- location: ``/apps/jasmin/community/matlab/matlab_R2024b``
- ``chown`` new lic file to ``valeriu:rdg_climate_matlab``
- also move new file to existng file in ``licenses/license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic`` (needs be the same name)

### Renew license 25 September 2026

Procedure:

- open a DTS ticket (at the new service-now portal) and email King and tell him this is up for renewal
- Ian at DTS will cut a new license, make sure you provide him with the expired license file to have the needed data
- pop the new license onto JASMIN, chown it like this:
```
[valeriu@sci-vm-01 licenses]$ ls -la
total 111
drwxr-xr-x  2 valeriu rdg_climate_matlab     0 Sep 25 09:36 .
drwxr-xr-x 17 valeriu rdg_climate_matlab     0 May 30  2025 ..
-rw-r--r--  1 valeriu users              39801 Sep 25 09:36 license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic
-rw-r--r--  1 valeriu rdg_climate_matlab 36765 May 30  2025 license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic.OLD
-rw-r--r--  1 valeriu rdg_climate_matlab 36432 Sep  9  2025 license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic.OLD2
[valeriu@sci-vm-01 licenses]$ chown valeriu:rdg_climate_matlab license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic
[valeriu@sci-vm-01 licenses]$ ls -la
total 111
drwxr-xr-x  2 valeriu rdg_climate_matlab     0 Sep 25 09:36 .
drwxr-xr-x 17 valeriu rdg_climate_matlab     0 May 30  2025 ..
-rw-r--r--  1 valeriu rdg_climate_matlab 39801 Sep 25 09:36 license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic
-rw-r--r--  1 valeriu rdg_climate_matlab 36765 May 30  2025 license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic.OLD
-rw-r--r--  1 valeriu rdg_climate_matlab 36432 Sep  9  2025 license_sci-vm-01.jasmin.ac.uk_1089045_R2024b.lic.OLD2
[valeriu@sci-vm-01 licenses]$ cd /apps/jasmin/community/matlab/app/bin
[valeriu@sci-vm-01 bin]$ ./matlab -nodisplay
Enter passphrase for /home/users/valeriu/.ssh/id_rsa_jasmin: 
Identity added: /home/users/valeriu/.ssh/id_rsa_jasmin (/home/users/valeriu/.ssh/id_rsa_jasmin)

Enter passphrase for /home/users/valeriu/.ssh/id_rsa_jasmin: 
Identity added: /home/users/valeriu/.ssh/id_rsa_jasmin (/home/users/valeriu/.ssh/id_rsa_jasmin)

                                                        < M A T L A B (R) >
                                              Copyright 1984-2024 The MathWorks, Inc.
                                         R2024b Update 5 (24.2.0.2863752) 64-bit (glnxa64)
                                                         January 31, 2025

 
To get started, type doc.
For product information, visit www.mathworks.com.
 
>> 2+2
```

### Install Procedure

- source: `/apps/jasmin/community/matlab/matlab_R2024b`
- install target: `/apps/jasmin/community/matlab/MATLAB`
- install procedure:
  - install with flags, using `installer_input_local.txt` configuration file
  - install command:
```
/mnt/.matlab/install -inputFile matlab-v.txt
```
  - needs:
    - a valid File Installation Key (FIK) via option `fileInstallationKey=xxxxx-xxxxx-...`
    - a valid license file via option `licensePath=/apps/jasmin/community/matlab/matlab_R2024b/matlab_license.lic`
  - logs in `/apps/jasmin/community/matlab/MATLAB/log`


### Obsolete

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
open a wretched window

As per https://uk.mathworks.com/matlabcentral/answers/1636600-install-matlab-on-linux-without-gui it appears others are saying one
still needs a bloody GUI. `enableLNU=no` doesn't fix it.

This page https://www.mathworks.com/help/install/ug/install-noninteractively-silent-installation.html doesn't says exactly what I am trying to do,
bar the target install is an unpacked ISO (which should be eqivalent to what we have on JASMIN).
