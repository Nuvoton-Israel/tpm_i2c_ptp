# TPM I2C PTP
TPM Linux I2C Driver for TPM devices, that implement the I2C interface defined by TCG PC Client Platform TPM Profile (PTP) Specification, revision 1.05 version 14
https://trustedcomputinggroup.org/resource/pc-client-platform-tpm-profile-ptp-specification/

This driver was developed and tested with Nuvoton's NPCT7XX on rpi3, but has been designed to be compatible with any TPM that implements this interface.

The driver is compatible with kernel v5.6.

## Content
I2C-based TPM driver source code patches:
- **v1-0001-tpm2-add-longer-timeout-for-verify-signature-command.patch **
- **v1-0002-tpm-Make-read-16-32-and-write32-in-tpm_tis_phy_ops-o.patch **
- **v1-0003-tpm-tpm_tis-Fix-expected-bit-handling-and-send-all-b.patch **
- **v1-0004-tpm-tpm_tis-Add-retry-in-case-of-protocol-failure **
- **v1-0005-tpm-tpm_tis-Add-verify_data_integrity-handle-to-tpm_ **
- **v1-0006-tpm-tpm_tis-Rewrite-tpm_tis_req_canceled **
- **v1-0007-tpm-Handle-an-exception-for-TPM-Firmware-Update-mode **
- **v1-0008-tpm-tpm_tis-verify-TPM_STS-register-is-valid-after-l **
- **v1-0009-tpm-Add-YAML-schema-for-TPM-TIS-I2C-options **
- **v1-0010-tpm-tpm_tis-add-tpm_tis_i2c-driver **
- **v1-0011-Interrupts-implementation **
rpi3 device tree configuration patch:
- **v1-0012-Device-tree-definitions- ** 
This descriptive Letter:
- **README.md** 

## How to use
- Clone Linux kernel source tree v5.2 to _'myLinux'_
- Apply patches v1-0001 to v1-0011
- add TPM to your device tree(if using rpi3, apply v1-0012 patch), and set tpm-irq gpio in order to work with host intterrupts (recommended)
- Use menuconfig to set the TPM HW (Device Drivers->Character devices>):
  Set 'M'/'Y' for "TPM Hardware Support" and enter its menu 
  set 'M'/'Y' for "TPM I2C Interface Specification". If any other TPM is set, clear it
- Build and install the kernel, modules, and Device Tree blobs for the target machine and boot it

