# Matter (previously CHIP) on AmebaD

## CHIP Ameba-D All Clusters Example

    README

    https://github.com/hank820/connectedhomeip/tree/base0728_gn/examples/all-clusters-app/ambd


## Get amebaD SDK & Matter SDK

    Test on Ubuntu 20.04

To check out this repository:

    git clone --recurse-submodules https://github.com/hank820/ambd_sdk_with_chip.git

If you already have a checkout, run the following command to sync submodules recursively:

	git submodule update --init --recursive

## Set Matter Build Environment 

    cd third_party/connectedhomeip

    source scripts/bootstrap.sh

    source scripts/activate.sh

    > Find more details to setup linux build environment
    > https://github.com/hank820/connectedhomeip/blob/master/docs/BUILDING.md


## Make Little CPU
    cd ambd_sdk_with_chip/project/realtek_amebaD_va0_example/GCC-RELEASE/project_lp

    make all
    
    output : project/realtek_amebaD_va0_example/GCC-RELEASE/project_lp/asdk/image/km0_boot_all.bin

## Make CHIP library by gn and Make lib_main.a
### all-cluster-app

    cd ambd_sdk_with_chip/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp
    make -C asdk lib_all

### lighting-app

    cd ambd_sdk_with_chip/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp
    make -C asdk light

### CHIP core (generate by GN/ninja in connectedhomeip. Config by [chip/Makefile](https://github.com/hank820/ambd_sdk_with_chip/blob/main/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/make/chip/Makefile))

    output : ambd_sdk_with_chip/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/lib/application

    > libCHIP.a, ibCoreTests.a, ibChipCryptoTests.a, ibRawTransportTests.a...

### CHIP application (generate by [chip_main/Makefile](https://github.com/hank820/ambd_sdk_with_chip/blob/main/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/make/chip_main/Makefile))

    output : ambd_sdk_with_chip/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/lib/application

    > lib_main.a

## Make Big CPU
    cd ambd_sdk_with_chip/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp

    make all
    
    output : 

    project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/image/km4_boot_all.bin
    
    project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/image/km0_km4_image2.bin

## Flash Image on AmebaD EVB

Please refer [Application Note](https://github.com/hank820/ambd_sdk_with_chip/blob/master/doc/AN0400%20Ameba-D%20Application%20Note%20v14.pdf) Chapter 8 : Image Tool

    Image Tool Path : $(SDK_ROOT)/tools/AmebaD/Image_Tool/
    

## Run CHIP task on Ameba D (all-cluster-app example)
    enter command in console

    ATW0=testAP

    ATW1=password

    ATWC

    ATS$ => Run chip task


## Test with [chip-tool](https://github.com/hank820/connectedhomeip/tree/master/examples/chip-tool)
Use standalone chip-tool app(linux) to communicate with the device.

`Disable "config_pair_with_random_id = false" in examples/chip-tool/BUILD.gn if test bypass mode`

`./chip-tool pairing bypass xxx.xxx.xxx.xxx 5540  (Ameba IP)`

<b>onoff cluster</b>

Use PB_5 as output, connect a LED to this pin and GND.

`./chip-tool onoff on 1`

`./chip-tool onoff off 1`
    
<b>doorlock cluster</b>

`./chip-tool doorlock lock-door 1 1`
    
`./chip-tool doorlock unlock-door 1 1`

