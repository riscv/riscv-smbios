# RISC-V Processor SMBIOS Spec
**(This spec is not in the ratification process yet)**

This repository contains SMBIOS tables of RISC-V processor. SMBIOS tables provide hardware information and capability of RISC-V processor to the management software, OS boot loader, OS, shell application, etc for recognizing the presence of hardware and settings. For RISC-V processor, below SMBIOS tables are necessary to built up according to the architecture of RISC-V processor variant.
- SMBIOS Type 4, Processor Information
- SMBIOS Type 44, Processor Additional Information

The intention of providing RISC-V SMBIOS tables is to boot RISC-V system to the unified software on RISC-V processor variants, without rebuilding software to conform to the specific architecture of RISC-V variant. The unified RISC-V software could execute different processes according to the information of RISC-V capability and settings provided in SMBIOS tables. 
