option(DISK_CACHE "Enable SD card disk cache" ON)
option(UNEXPECTED_SHUTDOWN "Enable the Unexpected Shutdown screen" ON)
option(IMU_LSM6DS33 "Enable I2C2 and LSM6DS33 IMU" OFF)
option(PXX1 "PXX1 protocol support" ON)
option(PXX2 "PXX2 protocol support" OFF)
option(AFHDS3 "AFHDS3 TX Module" ON)
option(MULTIMODULE "DIY Multiprotocol TX Module (https://github.com/pascallanger/DIY-Multiprotocol-TX-Module)" ON)
option(GHOST "Ghost TX Module" ON)
option(SPACEMOUSE "Support for SpaceMouse" OFF)
option(MODULE_SIZE_STD "Standard size TX Module" ON)
option(LUA_MIXER "Enable LUA mixer/model scripts support" ON)

set(PWR_BUTTON "PRESS" CACHE STRING "Pwr button type (PRESS/SWITCH)")
set(CPU_TYPE STM32F4)
set(HSE_VALUE 12000000)
set(SDCARD YES)
set(STORAGE_MODELSLIST YES)
set(HAPTIC YES)
set(GUI_DIR colorlcd)
set(BITMAPS_DIR 480x272)
set(NAVIGATION_TYPE horus)
set(TARGET_DIR horus)
set(RTC_BACKUP_RAM YES)
set(PPM_LIMITS_SYMETRICAL YES)
set(USB_SERIAL ON CACHE BOOL "Enable USB serial (CDC)")
set(ROTARY_ENCODER YES)
set(HARDWARE_EXTERNAL_MODULE YES)

# for size report script
set(CPU_TYPE_FULL STM32F429xI)
set(TARGET_LINKER_DIR stm32f429_sdram)
set(SIZE_TARGET_MEM_DEFINE "MEM_SIZE_SDRAM2=8192")

add_definitions(-DPCBFRSKY)

# defines existing internal modules
set(INTERNAL_MODULES PXX1;PXX2;MULTI;CRSF CACHE STRING "Internal modules")

# VCP CLI
set(ENABLE_SERIAL_PASSTHROUGH ON CACHE BOOL "Enable serial passthrough")
set(CLI ON CACHE BOOL "Enable CLI")
set(AUX_SERIAL ON)

# IMU support
set(IMU ON)

# Touch Screen support
set(HARDWARE_TOUCH YES)

if (PCB STREQUAL X10)
  set(PCBREV "STD" CACHE STRING "PCB Revision")
  add_definitions(-DSOFTWARE_VOLUME)
  set(FIRMWARE_SRC
    ${FIRMWARE_SRC}
    targets/common/arm/stm32/audio_dac_driver.cpp
    )
  if (NOT (PCBREV STREQUAL TX16S OR PCBREV STREQUAL T15))
      set(FIRMWARE_SRC
        ${FIRMWARE_SRC}
        targets/common/arm/stm32/sticks_pwm_driver.cpp
        )
  endif()
  set(BITMAPS_TARGET x10_bitmaps)
  set(FONTS_TARGET x10_fonts)
  set(LCD_DRIVER lcd_driver.cpp)
  if (PCBREV STREQUAL EXPRESS)
    set(FLAVOUR x10express)
    set(DEFAULT_INTERNAL_MODULE ISRM_PXX2 CACHE STRING "Default internal module")
    set(BLUETOOTH ON)
    set(AUX_SERIAL OFF)
    add_definitions(-DHARDWARE_POWER_MANAGEMENT_UNIT)
    add_definitions(-DRADIO_X10E)
    add_definitions(-DMANUFACTURER_FRSKY)
  elseif (PCBREV STREQUAL T15)
    set(FLAVOUR t15)
    add_definitions(-DRADIO_T15)
    add_definitions(-DRADIO_FAMILY_T16)
    set(FUNCTION_SWITCHES ON)
    set(DEFAULT_INTERNAL_MODULE CROSSFIRE CACHE STRING "Default internal module")
    option(BLUETOOTH "Support for bluetooth module" OFF)
    option(INTERNAL_GPS "Support for internal NMEA GPS" OFF)
    set(AUX_SERIAL ON)
    set(AUX2_SERIAL OFF)
    add_definitions(-DMANUFACTURER_JUMPER)
    set(LCD_DRIVER lcd_st7796s_driver.cpp)
  elseif (PCBREV STREQUAL T16)
    set(FLAVOUR t16)
    add_definitions(-DRADIO_T16)
    add_definitions(-DRADIO_FAMILY_T16)
    set(DEFAULT_INTERNAL_MODULE MULTIMODULE CACHE STRING "Default internal module")
    option(BLUETOOTH "Support for bluetooth module" OFF)
    option(INTERNAL_GPS "Support for internal GPS" ON)
    add_definitions(-DMANUFACTURER_JUMPER)
    if (NOT BLUETOOTH)
      set(AUX2_SERIAL ON)
    endif()
    set(INTERNAL_GPS_BAUDRATE "9600" CACHE STRING "Baud rate for internal GPS")    
    set(FLYSKY_GIMBAL ON)
  elseif (PCBREV STREQUAL TX16S)
    set(FLAVOUR tx16s)
    add_definitions(-DRADIO_TX16S)
    add_definitions(-DRADIO_FAMILY_T16)

#   Uncomment the line below to enable bootloader support for SPI flash
#    add_definitions(-DSPI_FLASH)

    set(DEFAULT_INTERNAL_MODULE MULTIMODULE CACHE STRING "Default internal module")
    option(BLUETOOTH "Support for bluetooth module" OFF)
    option(INTERNAL_GPS "Support for internal GPS" ON)
    set(SWSERIALPOWER YES)
    set(USB_CHARGER YES)
    add_definitions(-DMANUFACTURER_RADIOMASTER)
    if (NOT BLUETOOTH)
      set(AUX2_SERIAL ON)
    endif()
    set(INTERNAL_GPS_BAUDRATE "9600" CACHE STRING "Baud rate for internal GPS")
    set(FLYSKY_GIMBAL ON)
    set(FLEXSW "2" CACHE STRING "Max flex inputs usable as switches")
  elseif (PCBREV STREQUAL F16)
    set(FLAVOUR f16)
    add_definitions(-DRADIO_F16)
    add_definitions(-DRADIO_FAMILY_T16)
    set(DEFAULT_INTERNAL_MODULE MULTIMODULE CACHE STRING "Default internal module")
    option(BLUETOOTH "Support for bluetooth module" OFF)
    option(INTERNAL_GPS "Support for internal NMEA GPS" OFF)
    set(DEFAULT_THEME "DARKBLUE")
    add_definitions(-DMANUFACTURER_FATFISH)
    set(INTERNAL_GPS_BAUDRATE "9600" CACHE STRING "Baud rate for internal GPS")
    set(FLEXSW "2" CACHE STRING "Max flex inputs usable as switches")
    set(VIDEO_SWITCH ON)
  elseif (PCBREV STREQUAL T18)
    set(FLAVOUR t18)
    add_definitions(-DRADIO_T18)
    add_definitions(-DRADIO_FAMILY_T16)
    set(DEFAULT_INTERNAL_MODULE MULTIMODULE CACHE STRING "Default internal module")
    option(BLUETOOTH "Support for bluetooth module" OFF)
    option(INTERNAL_GPS "Support for internal GPS" ON)    
    add_definitions(-DMANUFACTURER_JUMPER)
    if (NOT BLUETOOTH)
      set(AUX2_SERIAL ON)
    endif()
    set(INTERNAL_GPS_BAUDRATE "9600" CACHE STRING "Baud rate for internal GPS")    
    set(FLYSKY_GIMBAL ON)
  else()
    set(FLAVOUR x10)
    set(DEFAULT_INTERNAL_MODULE XJT_PXX1 CACHE STRING "Default internal module")
    set(BLUETOOTH ON)
    add_definitions(-DMANUFACTURER_FRSKY)
  endif()
elseif (PCB STREQUAL X12S)
  set(DEFAULT_INTERNAL_MODULE XJT_PXX1 CACHE STRING "Default internal module")
  set(FLAVOUR x12s)
  set(BLUETOOTH ON)
  set(PCBREV "13" CACHE STRING "PCB Revision")
  if(${PCBREV} GREATER 10)
    option(INTERNAL_GPS "Internal GPS installed" YES)
  else()
    option(INTERNAL_GPS "Internal GPS installed" NO)
    if(NOT INTERNAL_GPS)
      message("Horus: Internal GPS is optional, use INTERNAL_GPS=YES option to enable it")
    endif()
  endif()
  set(INTERNAL_GPS_BAUDRATE "9600" CACHE STRING "Baud rate for internal GPS")
  set(AUX2_SERIAL ON) # wire to GPS
  set(FIRMWARE_SRC ${FIRMWARE_SRC} targets/common/arm/stm32/vs1053b.cpp)
  set(BITMAPS_TARGET x12s_bitmaps)
  set(FONTS_TARGET x12s_fonts)
  set(LCD_DRIVER lcd_driver.cpp)
  add_definitions(-DMANUFACTURER_FRSKY)
endif()

add_definitions(-DPCB${PCB})
add_definitions(-DPCBREV=${PCBREV})
add_definitions(-DPCBREV_${PCBREV})

set(RADIO_DEPENDENCIES
  ${RADIO_DEPENDENCIES}
  ${BITMAPS_TARGET}
  )

add_definitions(-DPCBHORUS -DSTM32F429_439xx -DSTM32F429xx -DSDRAM -DCCMRAM -DCOLORLCD -DHARDWARE_KEYS)
add_definitions(-DEEPROM_VARIANT=0 -DAUDIO -DVOICE -DRTCLOCK)
add_definitions(-DGPS_USART_BAUDRATE=${INTERNAL_GPS_BAUDRATE})
add_definitions(-DPWR_BUTTON_${PWR_BUTTON})
add_definitions(-DSTM32_SUPPORT_32BIT_TIMERS)

set(SDRAM ON)

if(FUNCTION_SWITCHES)
add_definitions(-DFUNCTION_SWITCHES)
endif()

if(NOT UNEXPECTED_SHUTDOWN)
  add_definitions(-DNO_UNEXPECTED_SHUTDOWN)
endif()

if(IMU_LSM6DS33)
  add_definitions(-DIMU_LSM6DS33)
  set(AUX_SERIAL OFF)
endif()

if(AUX_SERIAL)
  add_definitions(-DCONFIGURABLE_MODULE_PORT)
endif()

if(NOT INTERNAL_MODULE_PXX1)
  option(ALLOW_TRAINER_MULTI "Allow multimodule to be use as trainer input" OFF)
endif()

if("PXX2" IN_LIST INTERNAL_MODULES)
  if (NOT PCBREV STREQUAL EXPRESS)
    set(ACCESS_DENIED ON)
    add_definitions(-DACCESS_DENIED)
  endif()
endif()

if(ALLOW_TRAINER_MULTI)
  add_definitions(-DALLOW_TRAINER_MULTI)
endif()

if(DISK_CACHE)
  set(SRC ${SRC} disk_cache.cpp)
  add_definitions(-DDISK_CACHE)
endif()

if(INTERNAL_GPS)
  set(SRC ${SRC} gps.cpp gps_nmea.cpp gps_ubx.cpp)
  add_definitions(-DINTERNAL_GPS)
  message("-- Internal GPS enabled")
endif()

if(SPACEMOUSE)
  add_definitions(-DSPACEMOUSE)
  set(SRC ${SRC} spacemouse.cpp)
  message("-- SpaceMouse enabled")
endif()

set(GVAR_SCREEN model_gvars.cpp)

if(HARDWARE_TOUCH)
  if(FLAVOUR STREQUAL t15)
    set(TOUCH_DRIVER cst8xx_driver.cpp)
  else()
    set(TOUCH_DRIVER tp_gt911.cpp)
    add_definitions(-DTP_GT911)
  endif()
  add_definitions(-DHARDWARE_TOUCH)
endif()

if(SWSERIALPOWER)
  add_definitions(-DSWSERIALPOWER)
endif()

if(USB_CHARGER)
  set(FIRMWARE_TARGET_SRC ${FIRMWARE_TARGET_SRC} usb_charger_driver.cpp)
  add_definitions(-DUSB_CHARGER)
endif()

if(BLUETOOTH)
  add_definitions(-DBLUETOOTH)
  set(FIRMWARE_SRC
    ${FIRMWARE_SRC}
    targets/common/arm/stm32/bluetooth_driver.cpp)
  set(SRC
    ${SRC}
    bluetooth.cpp)
  add_definitions(-DHARDWARE_TRAINER_BLUETOOTH)
endif()

set(FIRMWARE_TARGET_SRC
  ${FIRMWARE_TARGET_SRC}
  ${LCD_DRIVER}
  ${TOUCH_DRIVER}
  board.cpp
  backlight_driver.cpp
  led_driver.cpp
  )

set(FIRMWARE_SRC
  ${FIRMWARE_SRC}
  targets/common/arm/stm32/sdram_driver.cpp
  targets/common/arm/stm32/rotary_encoder_driver.cpp
  targets/common/arm/stm32/heartbeat_driver.cpp
  targets/common/arm/stm32/ads79xx.cpp
  targets/common/arm/stm32/dma2d.cpp
  targets/common/arm/stm32/spi_flash.cpp
  targets/common/arm/stm32/diskio_spi_flash.cpp
  drivers/frftl.cpp
  )

if (VIDEO_SWITCH)
  add_definitions(-DVIDEO_SWITCH)
  set(FIRMWARE_SRC
    ${FIRMWARE_SRC}
    targets/horus/videoswitch_driver.cpp
  )
endif()

if(BOOTLOADER)
  set(FIRMWARE_SRC
    ${FIRMWARE_SRC}
    targets/common/arm/loadboot.cpp
    )
endif()

set(SRC ${SRC}
  io/frsky_firmware_update.cpp
  io/multi_firmware_update.cpp
  )

if (MULTIMODULE)
  add_definitions(-DMULTI_PROTOLIST)
    set(SRC ${SRC}
      io/multi_protolist.cpp
  )
endif()

# Make malloc() thread-safe
add_definitions(-DTHREADSAFE_MALLOC)

set(STM32LIB_SRC
  STM32F4xx_StdPeriph_Driver/src/stm32f4xx_sdio.c
  STM32F4xx_StdPeriph_Driver/src/stm32f4xx_fmc.c
  STM32F4xx_StdPeriph_Driver/src/stm32f4xx_ltdc.c
  STM32F4xx_StdPeriph_Driver/src/stm32f4xx_tim.c
  )
