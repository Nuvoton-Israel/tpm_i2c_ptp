# TPM I2C PTP
TPM I2C Linux Driver, implements the I2C interface defined by TCG in the PC Client PTP for TPM2.0 Revision 1.03 version 22
https://trustedcomputinggroup.org/wp-content/uploads/TCG_PC_Client_Platform_TPM_Profile_PTP_2.0_r1.03_v22.pdf

This driver was developed and tested with Nuvoton's NPCT7XX, but has been designed to be compatible with any TPM that implements this interface.

## PACKAGE CONTENTS
- **tpm_i2c_ptp.c** - I2C-based TPM driver source code
- **001-add-tpm_i2c_ptp.patch** - Patch to apply before TPM module building
- **README.md** - This descriptive Letter
- **_rpi_example_**
  - **HowToBuild** - Instructions for building and installing the driver on Raspberry Pie 3
  - **002-add-tpm-to-rpi-dts.patch** - Example patch of adding a tpm-i2c-ptp device to the RPI devicetree

## How to use
- Clone Linux kernel source tree v5.0-rc1 to _'myLinux'_
- To insert support in TPM I2C PTP, apply 001-add-tpm_i2c_ptp.patch
- Copy tpm_i2c_ptp.c to _'myLinux'_/linux/drivers/char/tpm
- Add the TPM to the devicetree, see example in rpi_example/002-add-tpm-to-rpi-dts.patch. Set tpm-irq gpio in order to work with host intterrupts (better performance), otherwise, the driver will use polling instead.
- Use menuconfig to set the TPM HW (Device Drivers->Character devices>):
  Set 'M'/'Y' for "TPM Hardware Support" and enter its menu 
  set 'M'/'Y' for TCG_TIS_I2C_PTP. If any other TPM is set, clear it
  After setting TCG_TIS_I2C_PTP, TCG_TIS_I2C_PTP_MAX_SIZE will be prompted.
    Default value is 32 (bytes). If the target machine is Rasperry Pie, set it to be 15, in order to bypass RPI issue #254 (I2C clock stretch, https://github.com/raspberrypi/linux/issues/254)
    You can also skip this step and set this parameter later during the module intallation (sudo insmod drivers/char/tpm/tpm_i2c_ptp.ko i2c_max_size=15), in case you choose 'M'
- Build and install the kernel, modules, and Device Tree blobs for the target machine and boot it
- In case you built the tpm module as an external module, install it:
```
  sudo modprobe crc-ccitt
  sudo insmod drivers/char/tpm/tpm.ko
  sudo insmod drivers/char/tpm/tpm_i2c_ptp.ko
```

Full instruction of how to bring-up RPI, and how to insert and build the TPM I2C PTP module in the RPI kernel, are detailed in rpi_example/HowToBuild
