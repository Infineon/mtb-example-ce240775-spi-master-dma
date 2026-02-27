# PDL: SCB SPI master with DMA

This example demonstrates the use of the SPI serial communication block (SCB) resource for Infineon MCU in master and slave mode using DMA. The SPI master is configured to send command packets to control a user LED on the slave. Both the master and slave can be on the same device or on different devices.

[View this README on GitHub.](https://github.com/Infineon/mtb-example-ce240775-spi-master-dma)

[Provide feedback on this code example.](https://yourvoice.infineon.com/jfe/form/SV_1NTns53sK2yiljn?Q_EED=eyJVbmlxdWUgRG9jIElkIjoiQ0UyNDA3NzUiLCJTcGVjIE51bWJlciI6IjAwMi00MDc3NSIsIkRvYyBUaXRsZSI6IlBETDogU0NCIFNQSSBtYXN0ZXIgd2l0aCBETUEiLCJyaWQiOiJrb2ppLm1penVtb3RvQGluZmluZW9uLmNvbSIsIkRvYyB2ZXJzaW9uIjoiMS4xLjAiLCJEb2MgTGFuZ3VhZ2UiOiJFbmdsaXNoIiwiRG9jIERpdmlzaW9uIjoiTUNEIiwiRG9jIEJVIjoiQVVUTyIsIkRvYyBGYW1pbHkiOiJBVVRPIE1DVSJ9)


## Requirements

- [ModusToolbox&trade;](https://www.infineon.com/modustoolbox) v3.7 or later (tested with v3.7)
- Board support package (BSP) minimum required version 3.0.0
- Programming language: C
- Associated parts: [TRAVEO&trade; T2G family Cluster series](https://www.infineon.com/cms/en/product/microcontroller/32-bit-traveo-t2g-arm-cortex-microcontroller/32-bit-traveo-t2g-arm-cortex-for-cluster/), [XMC5000 MCUs](https://www.infineon.com/products/microcontroller/32bit-industrial-arm-cortex-m/xmc5000) and [TRAVEO&trade; T2G family body high CYT4BF series](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-body/t2g-cyt4bf)


## Supported toolchains (make variable 'TOOLCHAIN')

- GNU Arm&reg; Embedded Compiler v14.2.1 (`GCC_ARM`) – Default value of `TOOLCHAIN`
- Arm&reg; Compiler v6.22 (`ARM`)
- IAR C/C++ Compiler v9.50.2 (`IAR`)


## Supported kits (make variable 'TARGET')

- [TRAVEO&trade; T2G Cluster 6M Lite Kit](https://www.infineon.com/cms/en/product/evaluation-boards/kit_t2g_c-2d-6m_lite/) (`KIT_T2G_C-2D-6M_LITE`) – Default value of `TARGET`
- [XMC5200 Evaluation Kit](https://www.infineon.com/evaluation-board/KIT-XMC52-EVK) (`KIT_XMC52_EVK`) 
- [TRAVEO&trade; T2G Cluster 4M Lite Kit](https://www.infineon.com/cms/en/product/evaluation-boards/kit_t2g_c-2d-4m_lite/) (`KIT_T2G_C-2D-4M_LITE`)
- [TRAVEO&trade; T2G Body high Lite Kit](https://www.infineon.com/evaluation-board/KIT-T2G-B-H-LITE) (`KIT_T2G-B-H_LITE`)
- [TRAVEO&trade; T2G Body high Evaluation Kit](https://www.infineon.com/evaluation-board/KIT-T2G-B-H-EVK) (`KIT_T2G-B-H_EVK`)



## Hardware setup

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

Connect the pins of the SPI master (mSPI) and the SPI slave (sSPI) using jumper wires.

   **Table 1. SPI mster and slave pins on KIT_T2G_C-2D-6M_LITE**

   SPI Output/Input   |  mSPI                     | sSPI        
   -------------------| --------------------------| ---------- 
   SCLK               | SCB0_CLK    | SCB8_CLK    
   MOSI               | SCB0_MOSI   | SCB8_MOSI   
   MISO               | SCB0_MISO   | SCB8_MISO   
   SS                 | SCB0_SEL0   | SCB8_SEL0   

   **Table 2. SPI mster and slave pins on KIT_T2G_C-2D-4M_LITE**

   SPI Output/Input   |  mSPI                     | sSPI        
   -------------------| --------------------------| ---------- 
   SCLK               | SCB7_CLK    | SCB8_CLK    
   MOSI               | SCB7_MOSI   | SCB8_MOSI   
   MISO               | SCB7_MISO   | SCB8_MISO   
   SS                 | SCB7_SEL0   | SCB8_SEL0   

   **Table 3. SPI mster and slave pins on KIT_T2G-B-H_LITE**

   SPI Output/Input   |  mSPI                     | sSPI        
   -------------------| --------------------------| ---------- 
   SCLK               | ADC1_22    | SPI_CLK    
   MOSI               | UART_TX    | SPI_MOSI   
   MISO               | UART_RX    | SPI_MISO   
   SS                 | P14_3      | SPI_SEL0   

   **Table 4. SPI mster and slave pins on KIT_T2G-B-H_EVK**

   SPI Output/Input   |  mSPI                     | sSPI        
   -------------------| --------------------------| ---------- 
   SCLK               | P14_2    | ARD_P13_2    
   MOSI               | P14_1    | ARD_P13_1   
   MISO               | P14_0    | ARD_P13_0   
   SS                 | P14_3    | ARD_P13_3   


To operate this code example for KIT_T2G_C-2D-4M_LITE, connect below. 

   **Table 5. The connectors on KIT_T2G_C-2D-4M_LITE**

   Connector   |  Connection   | 
   ------------| --------------|
   X300        |  MB_CLK (2-3) |
   X301        |  MB_MSI (2-3) |

   
To operate this code example for KIT_T2G-B-H_EVK, implement 0 ohm registances below.  

   **Table 6. The 0 ohm registances on KIT_T2G-B-H_EVK**

   Remove   |  Implement   | 
   ---------| -------------|
   R129     | R134         |
   R132     | R136         |
   R144     | R148         |
   R146     | R150         |
 


## Software setup

See the [ModusToolbox&trade; tools package installation guide](https://www.infineon.com/ModusToolboxInstallguide) for information about installing and configuring the tools package.




This example requires no additional software or tools.



## Using the code example


### Create the project

The ModusToolbox&trade; tools package provides the Project Creator as both a GUI tool and a command line tool.

<details><summary><b>Use Project Creator GUI</b></summary>

1. Open the Project Creator GUI tool

   There are several ways to do this, including launching it from the dashboard or from inside the Eclipse IDE. For more details, see the [Project Creator user guide](https://www.infineon.com/ModusToolboxProjectCreator) (locally available at *{ModusToolbox&trade; install directory}/tools_{version}/project-creator/docs/project-creator.pdf*)

2. On the **Choose Board Support Package (BSP)** page, select a kit supported by this code example. See [Supported kits](#supported-kits-make-variable-target)

   > **Note:** To use this code example for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work

3. On the **Select Application** page:

   a. Select the **Applications(s) Root Path** and the **Target IDE**

      > **Note:** Depending on how you open the Project Creator tool, these fields may be pre-selected for you

   b. Select this code example from the list by enabling its check box

      > **Note:** You can narrow the list of displayed examples by typing in the filter box

   c. (Optional) Change the suggested **New Application Name** and **New BSP Name**

   d. Click **Create** to complete the application creation process

</details>


<details><summary><b>Use Project Creator CLI</b></summary>

The 'project-creator-cli' tool can be used to create applications from a CLI terminal or from within batch files or shell scripts. This tool is available in the *{ModusToolbox&trade; install directory}/tools_{version}/project-creator/* directory.

Use a CLI terminal to invoke the 'project-creator-cli' tool. On Windows, use the command-line 'modus-shell' program provided in the ModusToolbox&trade; installation instead of a standard Windows command-line application. This shell provides access to all ModusToolbox&trade; tools. You can access it by typing "modus-shell" in the search box in the Windows menu. In Linux and macOS, you can use any terminal application.


The following example clones the "[mtb-example-ce240775-spi-master-dma](https://github.com/Infineon/mtb-example-ce240775-spi-master-dma)" application with the desired name "SCB_SPI_master_with_DMA" configured for the *KIT_T2G_C-2D-6M_LITE* BSP into the specified working directory, *C:/mtb_projects*:

   ```
   project-creator-cli --board-id KIT_T2G_C-2D-6M_LITE --app-id mtb-example-ce240775-spi-master-dma --user-app-name SCB_SPI_master_with_DMA --target-dir "C:/mtb_projects"
   ```



The 'project-creator-cli' tool has the following arguments:

Argument | Description | Required/optional
---------|-------------|-----------
`--board-id` | Defined in the <id> field of the [BSP](https://github.com/Infineon?q=bsp-manifest&type=&language=&sort=) manifest | Required
`--app-id`   | Defined in the <id> field of the [CE](https://github.com/Infineon?q=ce-manifest&type=&language=&sort=) manifest | Required
`--target-dir`| Specify the directory in which the application is to be created if you prefer not to use the default current working directory | Optional
`--user-app-name`| Specify the name of the application if you prefer to have a name other than the example's default name | Optional

<br>

> **Note:** The project-creator-cli tool uses the `git clone` and `make getlibs` commands to fetch the repository and import the required libraries. For details, see the "Project creator tools" section of the [ModusToolbox&trade; tools package user guide](https://www.infineon.com/ModusToolboxUserGuide) (locally available at {ModusToolbox&trade; install directory}/docs_{version}/mtb_user_guide.pdf).

</details>



### Open the project

After the project has been created, you can open it in your preferred development environment.


<details><summary><b>Eclipse IDE</b></summary>

If you opened the Project Creator tool from the included Eclipse IDE, the project will open in Eclipse automatically.

For more details, see the [Eclipse IDE for ModusToolbox&trade; user guide](https://www.infineon.com/MTBEclipseIDEUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_ide_user_guide.pdf*).

</details>


<details><summary><b>Visual Studio (VS) Code</b></summary>

Launch VS Code manually, and then open the generated *{project-name}.code-workspace* file located in the project directory.

For more details, see the [Visual Studio Code for ModusToolbox&trade; user guide](https://www.infineon.com/MTBVSCodeUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_vscode_user_guide.pdf*).

</details>


<details><summary><b>Arm&reg; Keil&reg; µVision&reg;</b></summary>

Double-click the generated *{project-name}.cprj* file to launch the Keil&reg; µVision&reg; IDE.

For more details, see the [Arm&reg; Keil&reg; µVision&reg; for ModusToolbox&trade; user guide](https://www.infineon.com/MTBuVisionUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_uvision_user_guide.pdf*).

</details>


<details><summary><b>IAR Embedded Workbench</b></summary>

Open IAR Embedded Workbench manually, and create a new project. Then select the generated *{project-name}.ipcf* file located in the project directory.

For more details, see the [IAR Embedded Workbench for ModusToolbox&trade; user guide](https://www.infineon.com/MTBIARUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_iar_user_guide.pdf*).

</details>


<details><summary><b>Command line</b></summary>

If you prefer to use the CLI, open the appropriate terminal, and navigate to the project directory. On Windows, use the command-line 'modus-shell' program; on Linux and macOS, you can use any terminal application. From there, you can run various `make` commands.

For more details, see the [ModusToolbox&trade; tools package user guide](https://www.infineon.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mtb_user_guide.pdf*).

</details>


## Operation



1. Connect the board to your PC using the provided USB cable through the KitProg3 USB connector.

2. Configure the value of the `SPI_MODE` macro in *interface.h* to `SPI_MODE_BOTH` (only for kits with two SPI ports).

3. Program the board using one of the following:

   <details><summary><b>Using Eclipse IDE</b></summary>

      1. Select the application project in the Project Explorer.

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3_MiniProg4)**.
   </details>


   <details><summary><b>In other IDEs</b></summary>

   Follow the instructions in your preferred IDE.
   </details>


   <details><summary><b>Using CLI</b></summary>

     From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. The default toolchain is specified in the application's Makefile but you can override this value manually:
      ```
      make program TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TOOLCHAIN=GCC_ARM
      ```

    </details>

4. After programming, the application starts automatically. Observe that the user LED blinks at 1 Hz.


## Debugging

You can debug the example to step through the code.


<details><summary><b>In Eclipse IDE</b></summary>

Use the **\<Application Name> Debug (KitProg3_MiniProg4)** configuration in the **Quick Panel**. For details, see the "Program and debug" section in the [Eclipse IDE for ModusToolbox&trade; user guide](https://www.infineon.com/MTBEclipseIDEUserGuide).


</details>


<details><summary><b>In other IDEs</b></summary>

Follow the instructions in your preferred IDE.
</details>



## Design and implementation

By default, the kit is configured to work in both master and slave modes. Ensure that the `SPI_MODE` macro in *interface.h* is configured as `SPI_MODE_BOTH`.

Do the following to configure the kit to work in both master and slave modes:
1. Run the [Device configurator](https://www.infineon.com/ModusToolboxDeviceConfig) tool from the **Quick Panel** of the IDE.

   Because the kit has only one available SPI port, the associated SCB is by default aliased as 'mSPI' in the **Peripherals** tab.

2. Rename the associated SCBs to 'sSPI' and 'mSPI' and configure the SCBs as follows.

   **Figure 1. Configure peripherals master SPI**

   <img src = images/spi_master_set.jpg width="1000"> 

   <br />
   
   **Figure 2. Configure peripherals slave SPI**

   <img src = images/spi_slave_set.jpg width="1000">

   <br />


3. In the **Pins** tab, assign the correct drive mode to SPI pins in the **Drive Mode** drop-down menu:

   **Table 7. Drive mode for slave SPI pins**

    SPI pins | Drive mode
    :--------| ------------
    MOSI     | Digital High-Z; Input buffer ON
    MISO     | Strong Drive; Input buffer OFF
    SCLK     | Digital High-Z; Input buffer ON
    SS0      | Digital High-Z; Input buffer ON
   
   **Table 8. Drive mode for master SPI pins**
    SPI pins | Drive mode
    :--------| ------------
    MOSI     | Strong Drive; Input buffer OFF
    MISO     | Digital High-Z; Input buffer ON
    SCLK     | Strong Drive; Input buffer OFF
    SS0      | Strong Drive; Input buffer OFF

4. In the **DMA** tab, rename the DMA to 'txDma' and 'rxDma' and configure it as follows.

   **Figure 3. Configure txDMA**

   <img src = images/tx_dma_set.jpg width="1000">

   <br />
   
   **Figure 4. Configure rxDMA**

   <img src = images/rx_dma_set.jpg width="1000">

   <br />

5. Select **File** > **Save** to save the changes and generate the configuration files.


### Resources and settings

The Arm&reg; Cortex&reg;-M0 (CM0P) CPU controls both the master and slave SCBs. This example can be configured to operate in both master and slave SPI modes, allowing it to run on a single kit, as long as the kit supports two SPI ports on its I/O header.

The master sends a packet to the slave with the commands to turn the user LED ON or OFF. The packets are sent at one-second intervals. DMA is used to transfer the command data from the SRAM to the SPI FIFO on the master side, and similarly from the SPI FIFO to the SRAM on the slave side. The slave receives the packet and controls the LED according to the command received.

**Table 9. Application resources**

 Resource            | Alias/object        | Purpose
 --------------------| --------------------| ----------------------------------
 SCB (SPI)           |      mSPI           | Master SPI SCB
 SCB (SPI)           |      sSPI           | Slave SPI SCB
 GPIO                |     CYBSP_USER_LED  | LED indication
 DMA                 |     txDma           | Data transmitter
 DMA                 |     rxDma           | Data receiver

<br />


## Related resources

Resources  | Links
-----------|----------------------------------
Application notes  | [AN235305](https://www.infineon.com/assets/row/public/documents/10/42/infineon-an235305-getting-started-with-traveo-t2g-family-mcus-in-modustoolbox-applicationnotes-en.pdf) – Getting started with TRAVEO™ T2G family MCUs in ModusToolbox&trade; <br> [AN220193](https://www.infineon.com/assets/row/public/documents/10/42/infineon-an220118---gpio-usage-setup-in-traveo-t2g-family-applicationnotes-en.pdf) – GPIO usage setup in TRAVEO&trade; T2G family <br> [AN225401](https://www.infineon.com/assets/row/public/documents/10/42/infineon-an225401---how-to-use-serial-communication-block-scb-in-traveo-t2g-family-applicationnotes-en.pdf?fileId=8ac78c8c7cdc391c017d0d3e657867d2) – How to use serial communications block (SCB) in TRAVEO&trade; T2G family　<br /> [AN220191](https://www.infineon.com/assets/row/public/documents/10/42/infineon-an220191---how-to-use-direct-memory-access-dma-controller-in-traveot2g-family-applicationnotes-en.pdf) – How to use direct memory access (DMA) in TRAVEO&trade; T2G family <br /> [AN241720](https://www.infineon.com/document-promo/infineon-an241720-getting-started-with-xmc5000-mcu-on-modustoolbox-software_1071f992-eb73-4dce-94ad-e84c41407bfc) – Getting started with XMC5000 MCU on ModusToolbox&trade; software
Code examples  | [Using ModusToolbox&trade;](https://github.com/Infineon/Code-Examples-for-ModusToolbox-Software) on GitHub
Device documentation | [TRAVEO&trade; T2G body high family MCUs datasheets](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-body/t2g-cyt4bf/#documents) <br> [TRAVEO&trade; T2G body high family MCUs architecture/registers reference manuals](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-body/t2g-cyt4bf/#documents) <br> [TRAVEO&trade; T2G cluster family MCUs datasheets for CYT4DN](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-cluster/#documents) <br> [TRAVEO&trade; T2G cluster family MCUs architecture/registers reference manuals for CYT4DN](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-cluster/#documents) <br> [TRAVEO&trade; T2G cluster family MCUs datasheets for CYT3DL](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-cluster/#documents) <br> [TRAVEO&trade; T2G cluster family MCUs architecture/registers reference manuals for CYT3DL](https://www.infineon.com/products/microcontroller/32-bit-traveo-t2g-arm-cortex/for-cluster/#documents)　<br> [XMC5000 MCUs documents](https://www.infineon.com/products/microcontroller/32bit-industrial-arm-cortex-m/xmc5000#Documents)
Development kits | Select your kits from the [Evaluation board finder](https://www.infineon.com/cms/en/design-support/finder-selection-tools/product-finder/evaluation-board).
Libraries on GitHub  | [mtb-pdl-cat1](https://github.com/Infineon/mtb-pdl-cat1) – Peripheral Driver Library (PDL) 　<br /> [retarget-io](https://github.com/Infineon/retarget-io/tree/master) – Utility library to retarget STDIO messages to a UART port <br> [mtb-hal-xmc5x](https://github.com/Infineon/mtb-hal-xmc5x) – Hardware Abstraction Layer (HAL) library 
 <br /> Tools  | [ModusToolbox&trade;](https://www.infineon.com/modustoolbox) – ModusToolbox&trade; software is a collection of easy-to-use libraries and tools enabling rapid development with Infineon MCUs for applications ranging from wireless and cloud-connected systems, edge AI/ML, embedded sense and control, to wired USB connectivity using PSOC&trade; Industrial/IoT MCUs, AIROC&trade; Wi-Fi and Bluetooth&reg; connectivity devices, XMC&trade; Industrial MCUs, and EZ-USB&trade;/EZ-PD&trade; wired connectivity controllers. ModusToolbox&trade; incorporates a comprehensive set of BSPs, HAL, libraries, configuration tools, and provides support for industry-standard IDEs to fast-track your embedded application development.
<br />




## Other resources



Infineon provides a wealth of data at [www.infineon.com](https://www.infineon.com) to help you select the right device, and quickly and effectively integrate it into your design.




## Document history

Document title: *CE240775* – *PDL: SCB SPI master with DMA* 

| Version | Description of change |
| ------- | --------------------- |
| 1.0.0   | New code example      |
| 1.1.0   | Added support for KIT_XMC52_EVK, KIT_T2G_C-2D-4M_LITE, KIT_T2G-B-H_EVK and KIT_T2G-B-H_LITE, and updated to support ModusToolbox&trade; v3.7   |
<br />


All referenced product or service names and trademarks are the property of their respective owners.

The Bluetooth&reg; word mark and logos are registered trademarks owned by Bluetooth SIG, Inc., and any use of such marks by Infineon is under license.

PSOC&trade;, formerly known as PSoC&trade;, is a trademark of Infineon Technologies. Any references to PSoC&trade; in this document or others shall be deemed to refer to PSOC&trade;.

---------------------------------------------------------

(c) 2024-2026, Infineon Technologies AG, or an affiliate of Infineon Technologies AG. All rights reserved.
This software, associated documentation and materials ("Software") is owned by Infineon Technologies AG or one of its affiliates ("Infineon") and is protected by and subject to worldwide patent protection, worldwide copyright laws, and international treaty provisions. Therefore, you may use this Software only as provided in the license agreement accompanying the software package from which you obtained this Software. If no license agreement applies, then any use, reproduction, modification, translation, or compilation of this Software is prohibited without the express written permission of Infineon.
<br>
Disclaimer: UNLESS OTHERWISE EXPRESSLY AGREED WITH INFINEON, THIS SOFTWARE IS PROVIDED AS-IS, WITH NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, ALL WARRANTIES OF NON-INFRINGEMENT OF THIRD-PARTY RIGHTS AND IMPLIED WARRANTIES SUCH AS WARRANTIES OF FITNESS FOR A SPECIFIC USE/PURPOSE OR MERCHANTABILITY. Infineon reserves the right to make changes to the Software without notice. You are responsible for properly designing, programming, and testing the functionality and safety of your intended application of the Software, as well as complying with any legal requirements related to its use. Infineon does not guarantee that the Software will be free from intrusion, data theft or loss, or other breaches (“Security Breaches”), and Infineon shall have no liability arising out of any Security Breaches. Unless otherwise explicitly approved by Infineon, the Software may not be used in any application where a failure of the Product or any consequences of the use thereof can reasonably be expected to result in personal injury.
