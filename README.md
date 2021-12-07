## Disclaimer

This firmware is for testing purposes only. Do not use if you do not
have the equipment and knowledge to recover from a bad flash. Use this
firmware at your own risks. This firmware is untested and I take no
responsibility for your device getting bricked as a result of flashing
this firmware.

## Firmware features

This firmware attempts to enable hardware flow control in the
firmware maintained by [Koenkk](https://github.com/Koenkk/Z-Stack-firmware/blob/develop/coordinator/Z-Stack_3.x.0/). The firmware appears to require the hardware flow switch to be activated on the dongle and in the configuration file.


## Firmware Description

This firmware was compiled by following the instruction provided
[here](https://github.com/Koenkk/Z-Stack-firmware/blob/develop/coordinator/Z-Stack_3.x.0/COMPILE.md)
and modifying two files in the syscfg directory as described by the
following diffs:



```
diff ti_drivers_config.h ~/syscfg.or/ti_drivers_config.h 
97,104d96
<   /* Owned by CONFIG_DISPLAY_UART as  */
< extern const uint_least8_t CONFIG_GPIO_3_CONST;
< #define CONFIG_GPIO_3 19
< 
< /* Owned by CONFIG_DISPLAY_UART as  */
< extern const uint_least8_t CONFIG_GPIO_4_CONST;
< #define CONFIG_GPIO_4 18
< 
225,226d216
<  *  CTS: DIO19
<  *  RTS: DIO18
```

and
```
diff ti_drivers_config.h ~/syscfg.or/ti_drivers_config.h 
97,104d96
<   /* Owned by CONFIG_DISPLAY_UART as  */
< extern const uint_least8_t CONFIG_GPIO_3_CONST;
< #define CONFIG_GPIO_3 19
< 
< /* Owned by CONFIG_DISPLAY_UART as  */
< extern const uint_least8_t CONFIG_GPIO_4_CONST;
< #define CONFIG_GPIO_4 18
< 
225,226d216
<  *  CTS: DIO19
<  *  RTS: DIO18
PCAXS::syscfg>diff ti_drivers_config.c ~/syscfg.or/ti_drivers_config.c
269,272c269,270
<     /* Owned by CONFIG_DISPLAY_UART as RTS */
<     GPIO_CFG_OUTPUT_INTERNAL | GPIO_CFG_OUT_STR_MED | GPIO_CFG_OUT_LOW, /* CONFIG_GPIO_4 */
<     /* Owned by CONFIG_DISPLAY_UART as CTS */
<     GPIO_CFG_INPUT_INTERNAL | GPIO_CFG_IN_INT_NONE | GPIO_CFG_PULL_DOWN_INTERNAL, /* CONFIG_GPIO_3 */
---
>     GPIO_CFG_NO_DIR, /* DIO_18 */
>     GPIO_CFG_NO_DIR, /* DIO_19 */
307,308d304
< const uint_least8_t CONFIG_GPIO_3_CONST = CONFIG_GPIO_3;
< const uint_least8_t CONFIG_GPIO_4_CONST = CONFIG_GPIO_4;
813,814c809,810
<     .ctsPin             = CONFIG_GPIO_3,
<     .rtsPin             = CONFIG_GPIO_4,
---
>     .ctsPin             = GPIO_INVALID_INDEX,
>     .rtsPin             = GPIO_INVALID_INDEX,
```

## More info

[Github](https://github.com/Koenkk/Z-Stack-firmware/issues/324)
