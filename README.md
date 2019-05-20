# TPM I2C PTP
TPM Linux I2C Driver for TPM devices, that implement the I2C interface defined by TCG PC Client Platform TPM Profile (PTP) Specification, revision 1.03 version 22
https://trustedcomputinggroup.org/wp-content/uploads/TCG_PC_Client_Platform_TPM_Profile_PTP_2.0_r1.03_v22.pdf

This driver was developed and tested with Nuvoton's NPCT7XX, but has been designed to be compatible with any TPM that implements this interface.

The driver is compatible with kernel v5.1. 

## Content
- **tpm_i2c_ptp.c** - I2C-based TPM driver source code
- **001-add-tpm_i2c_ptp.patch** - Patch to apply before TPM module building
- **README.md** - This descriptive Letter

## How to use
- Clone Linux kernel source tree v5.1 to _'myLinux'_
- Apply 001-add-tpm_i2c_ptp.patch
- Copy tpm_i2c_ptp.c to _'myLinux'_/linux/drivers/char/tpm
- Add the TPM to your device tree, and set tpm-irq gpio in order to work with host intterrupts (recommended), see instructions and an example in Documentation/devicetree/bindings/security/tpm/tpm-i2c-ptp.txt.
- Use menuconfig to set the TPM HW (Device Drivers->Character devices>):
  Set 'M'/'Y' for "TPM Hardware Support" and enter its menu 
  set 'M'/'Y' for TCG_TIS_I2C_PTP. If any other TPM is set, clear it
  After setting TCG_TIS_I2C_PTP, TCG_TIS_I2C_PTP_MAX_SIZE will be prompted.
    Choose the maximal length of a single I2C transaction between 1 and 32 bytes (default).
    You can also skip this step and set this parameter later during the module intallation (sudo insmod drivers/char/tpm/tpm_i2c_ptp.ko i2c_max_size=15), in case you choose 'M'
- Build and install the kernel, modules, and Device Tree blobs for the target machine and boot it
- In case you built the tpm module as an external module, install it:
```
  sudo modprobe crc-ccitt
  sudo insmod drivers/char/tpm/tpm.ko
  sudo insmod drivers/char/tpm/tpm_i2c_ptp.ko
```

