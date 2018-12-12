# RISC-V Processor SMBIOS Tables
## RISC-V SMBIOS Type 4 Processor Information
The information in SMBIOS Type 4 defines the attributes of a single RISC-V processor. Below fields in SMBIOS Type 4 contain RISC-V processor-specific information which is introduced in SMBIOS v3.?.?(TBD). All other fields SMBIOS Type 4 are built depends on RISC-V capability accordingly.                                                     

### Offset 06h Processor Family
----------------------------------------------------------
RISC-V processor is cataloged into three families. The value for RISC-V family is consistent with Machine Types defined in <a href= "https://docs.microsoft.com/en-us/windows/desktop/Debug/pe-format">Microsoft Protable Executable and Common Object File Format Specification (PE COFF)</a>. Due to the width of this filed is in 8-bit length which can't fit RISC-V processor family value, RISC-V processor family is declared in *offset 28h* ***Processor Family 2*** with *offset 06h* ***Processor Family*** set to FEh according to SMBIOS spec.

### Offset 08h Processor ID
----------------------------------------------------------
For RISC-V class CPUs, the processor ID contains a QWORD Machine **Vendor ID CSR (mvendroid)** of RISC-V processor hart 0. More information of RISC-V class CPU feature is described in RISC-V processor additional information (SMBIOS Type 44)
### Offset 26h Processor Characteristics
----------------------------------------------------------
***Processor Characteristics*** is set according to RISC-V processor capability accordingly.
- Both Bit 2 and Bit 8 must be 0 if RISC-V processor only support RV32.
- Bit 2 set to 1 if RISC-V processor supports RV64.
- Bit 8 set to 1 if RISC-V processor supports RV128.
  
| **WORD Bit Position** | **Meaning if Set**|
|---------------|-------------------|
|Bit 0|Reserved|
|Bit 1|Unknown|
|Bit 2|64-bit Capable |
|Bit 3|Multi-Core  |
|Bit 4|Hardware Thread  |
|Bit 5|Execute Protection  |
|Bit 6|Enhanced Virtualization  |
|Bit 7|Power/Performance Control  |
|Bit 8|128-bit Capable |
|Bit 9:15|Reserved |

### Offset 28h Processor Family 2
----------------------------------------------------------
RISC-V processor family is declared in this filed with *offset 06h* ***Processor Family*** set to FEh.

| **Hex Value** | **Decimal Value** | **Meaning**|
|---------------|-------------------|------------|
|200h|512|RISC-V RV32|
|201h|513|RISC-V RV64|
|202h|514|RISC-V RV128|

### Offset 30h The SMBIOS handle of Processor Additional Information
----------------------------------------------------------
Because SMBIOS Type 4 ***Processor Information*** is not sufficient to describe the characteristics of RISC-V processor and harts, SMBIOS handle in this filed links to additional information of processor  for describing RISC-V processor capabilities.

## RISC-V SMBIOS Type 44 Processor Additional Information
The information in this structure defines the processor additional information in case SMBIOS type 4 is not sufficient to describe processor characteristics. The SMBIOS type 44 has a field as reference handle to link back to SMBIOS type 4 which it belongs to. There may has one or multiples SMBIOS type 44 records linked to the same SMBIOS type 4. Each SMBIOS type 44 record describes each core of specific information in processor. SMBIOS type 44 defines the standard header of processor-specific block in SMBIOS type 44 (see section 7.45.1) while the content of processor-specific data is maintained by processor architecture in the separate document (see 7.45.2).

### Standard Processor Additional Information (Type 44) structure
-------------------------------------------------------------

Below is the standard header of SMBIOS type 44 defined in SMBIOS spec v3.?.?(TBD), the total length of entire SMBIOS type 44 is indicated in *offset 01h* and the value must be less than or equal to 255 in bytes according to section 6.1.2 in <a href="https://www.dmtf.org/standards/smbios">SMBIOS spec 3.2.0 spec</a>.

| **Offset** | **Name**                                            | **Length** | **Value** | **Description**  |
|------------|-----------------------------------------------------|------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 00h        | Type                                                | BYTE       | 44        | Processor Additional Information.|
| 01h        | Length                                              | BYTE       | 4+Y       | Length of the structure. Y is the length of Processor-specific Block specified at offset 06h.|
| 02h        | Handle                                              | WORD       | Varies    | Handle, or instance number, associated with the structure.|
| 04h        | Referenced Handle                                   | WORD       | Varies    | Handle, or instance number, associated with the processor (SMBIOS type 4) which the processor additional information describes.|
| 06h        | Processor-specific Additional Information Structure | Varies (Y) | Varies    | Processor-specific block. (See below section)|

### RISC-V Processor-specific Additional Information Structure
----------------------------------------------------------

RISC-V processor-specific additional information is constructed by RISC-V boot loader such as uboot, coreboot, UEFI edk2 firmware, etc. SMBIOS Type 44 is referred by RISC-V software for recognizing RISC-V hart capability. RISC-V SMBIOS Type 44 is version controlled which use major and minor versioning in 16-bit value at *offset 0*. The new fields must be added to the end of this table for the backward compatible.

| **Offset** | **Additional Info. Version** | **Name**                                                              | **Length** | **Value** | **Description**                                                                                                                                                                                                                                                                                                       |
|------------|------------------------------|-----------------------------------------------------------------------|------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 00h        | 0009h (v0.9)                 | Revision of RISC-V Processor Additional Information Structure         | WORD       | Varies    | Bit 15:8 Major revision Bit 7:0 Minor revision The newer revision of RISC-V Processor Additional Information Structure is backward compatible with older version of this structure.|
| 02h        | 0009h (v0.9)                 | Structure Length | Byte       | 110         | Length of Processor-specific Data |
| 03h        | 0009h (v0.9)                 | Hart ID          | DQWORD     | Varies    | The ID of this RISC-V Hart        |
| 13h        | 0009h (v0.9)                 | Boot Hart        | BYTE       | Boolean   | 1: This is boot hart to boot system 0: This is not the boot hart |
| 14h        | 0009h (v0.9)                 | Machine Vendor ID | DQWORD     | Varies    | The vendor ID of this RISC-V Hart  |
| 24h        | 0009h (v0.9)                 | Machine Architecture ID  | DQWORD     | Varies    | Base microarchitecture of the hart. Value of 0 is possible to indicate the ﬁeld is not implemented. The combination of Machine Architecture ID and Machine Vendor ID should uniquely identify the type of hart microarchitecture that is implemented.  |
| 34h        | 0009h (v0.9)                 | Machine Implementation ID                                             | DQWORD     | Varies    | Unique encoding of the version of the processor implementation. Value of 0 is possible to indicate the ﬁeld is not implemented. The Implementation value should reﬂect the design of the RISC-V Hart. |
| 44h        | 0009h (v0.9)                 | Instruction set supported                                             | DWORD      | Bit-field | Bits [25:0] encodes the presence of RISC-V standard extensions, which is equivalent to bits [25:0] in RISC-V Machine ISA Register (**misa** CSR). Bits set to one mean the certain extensions of instruction set are supported on this hart. |
| 48h        | 0009h (v0.9)                 | Privilege Level Supported                                             | BYTE       | Varies    | The privilege levels supported by this RISC-V Hart. Bit 0 Machine Mode BIT 1 Reserved BIT 2 Supervisor Mode Bit 3 User Mode BIT 6:4 Reserved BIT 7 Debug Mode |
| 49H        | 0009h (v0.9)                 | Machine Exception Trap Delegation Information                         | DQWORD     | Varies    | Bit set to one means the corresponding exception is delegated to supervisor execution environment. Otherwise, supervisor execution environment must register the event handler in Machine-Mode for the certain exceptions through environment call.|
| 59H        | 0009h (v0.9)                 | Machine Interrupt Trap Delegation Information                         | DQWORD     | Varies    | Bit set to one means the corresponding interrupt is delegated to supervisor execution environment. Otherwise, supervisor execution environment must register the event handler in Machine-Mode for the certain interrupts through environment. |
| 69h        | 0009h (v0.9)                 | The register width (XLEN)                                             | BYTE       | ENUM      | The width of register supported by this RISC-V Hart  |
| 6Ah        | 0009h (v0.9)                 | Machine Mode native base integer ISA width (M-XLEN)                   | BYTE       | ENUM      | The width (See below) of Machine Mode native base integer ISA supported by this RISC-V Hart   |
| 6Bh        | 0009h (v0.9)                 | Reserved                                                              | BYTE       | ENUM      | Placeholder for Hypervisor Mode  |
| 6Ch        | 0009h (v0.9)                 | Supervisor Mode native base integer ISA width (S-XLEN)                | BYTE       | ENUM      | The width (See below) of Supervisor Mode native base integer ISA supported by this RISC-V Hart  |
| 6Dh        | 0009h (v0.9)                 | User Mode native base integer ISA width (U-XLEN)                      | BYTE       | ENUM      | The width (See below) of the User Mode native base integer ISA supported by this RISC-V Hart   |

### Encoding of RISC-V Native Base Integer ISA Width
----------------------------------------------------------

| **Byte Value** | **Meaning** |
|----------------|-------------|
| 00h            | Unsupported |
| 01h            | 32-bit      |
| 02h            | 64-bit      |
| 03h            | 128-bit     |

### External Reference
----------------------------------------------------------
SMBIOS Spec -  https://www.dmtf.org/standards/smbios
Microsoft PE Format - https://docs.microsoft.com/en-us/windows/desktop/Debug/pe-format
### Contributing
----------------------------------------------------------
#### All contributors are listed as below,
Abner Chang <abner.chang@hpe.com>, **Hewlett Packard Enterprise (HPE)**
Gilbert Chen <gilbert.chen@hpe.com>, **Hewlett Packard Enterprise (HPE)**
