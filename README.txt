Step 1. Modify Makefile：
    Modify drivers/input/touchscreen/Makefile，add a new line into it ： 
        obj-$(CONFIG_TOUCHSCREEN_GT9XX) += gt9xx/	

Step 2. Modify Kconfig
    Modify drivers/input/touchscreen/Kconfig, include our kconfig under TOUCHSCREEN
	source "drivers/input/touchscreen/gt9xx/Kconfig" 
	
Step 3. Port dtsi：
    Port kernel/arch/arm/boot/dts/goodix-gt9xx-i2c.dtsi into platform dts
	During the porting, please set right value into below fields
	    interrupt-parent = <&msm_gpio>;
	    interrupts = <13 0x2800>;
	    reset-gpios = <&msm_gpio 12 0x0>;
	    irq-gpios = <&msm_gpio 13 0x2800>;
	    touchscreen-size-x = <1080>;
	    touchscreen-size-y = <1920>;

Step 4. Implement Pinctrl
    Goodix dts declared 4 pinctrl：
	     <&ts_int_default>;	<&ts_int_output_low>;<&ts_int_output_high>;<&ts_int_input>;
    Please implement them in pinctrl dts.
	kernel/arch/arm/boot/dts/msm8916-pinctrl.dtsi is an example.

Step 5. Modify defconfig
	Please refer kernel/arch/arm/configs/msm_defconfig .
		CONFIG_TOUCHSCREEN_GT9XX=y
		CONFIG_TOUCHSCREEN_GT9XX_UPDATE=y
		CONFIG_TOUCHSCREEN_GT9XX_DEBUG=y

Step 6. The porting is done, please compile.
