# setup vscode for cubemx  
  
1) install plugin C/C++ IntelliSense, debugging and code browsing  
  
2) create Makefile project from CubeMX (used controller stm32l432kc)  
  
3) create .vscode directory inside the generated folder  
  
4) create c_cpp_properties.json in .vscode with content  
  
{  
	"configurations": [  
		{  
			"name": "ARM none eabi GCC",  
			"includePath": [  
				"C:/GNU_Tools_Arm_Embedded/7_2018/arm-none-eabi/include",  
				"C:/GNU_Tools_Arm_Embedded/7_2018/lib/gcc/arm-none-eabi/7.3.1/include",  
				"${workspaceRoot}",  
				"${workspaceRoot}/Inc",  
				"${workspaceRoot}/Drivers/CMSIS/Include",  
                "${workspaceRoot}/Drivers/CMSIS/Core/Include",  
				"${workspaceRoot}/Drivers/STM32L4xx_HAL_Driver/Inc",  
                "${workspaceRoot}/Drivers/CMSIS/Device/ST/STM32L4xx/Include",  
				"${workspaceRoot}/Middlewares/Third_Party/FreeRTOS/Source/include",  
				"${workspaceRoot}/Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS_V2",  
				"${workspaceRoot}/Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F"  
			],  
			"defines": [  
				"DEBUG=1"  
			],  
			"intelliSenseMode": "msvc-x64",  
			"compilerPath": "/usr/bin/arm-none-eabi-gcc",  
			"compilerArgs": [  
				"-mcpu=cortex-m4",  
				"-mthumb",  
				"-mfloat-abi=hard"  
			],  
			"cStandard": "c11",  
			"cppStandard": "c++17"  
		}  
	],  
	"version": 4  
}  
  
5) create settings.json inside .vscode with content  
  
{  
    "terminal.integrated.cwd": "C:\\Users\\yadaniel\\Desktop\\stm32_tests",  
	"terminal.integrated.shell.windows": "C:\\msys64\\usr\\bin\\bash.exe",  
    "terminal.integrated.env.windows": {"PATH": "C:\\msys64\\bin;"},  
    //"terminal.integrated.shellArgs.windows": ["--login", "-i", "-c '/C/Users/yadaniel/Desktop/stm32_tests'"],  
    "editor.lineNumbers": "relative",  
    "extensions.ignoreRecommendations": true,  
    "vim.useSystemClipboard": true,  
    "FSharp.lineLens.prefix": " // ",  
    "files.associations": {  
        "cmsis_os.h": "c"  
    },  
}  
  
6) fix CubeMX  
  
   open C:\Users\yadaniel\Desktop\stm32_tests\test_HAL\Drivers\CMSIS\Device\ST\STM32L4xx\Include\stm32l4xx.h  
   and uncomment the STM32L432xx define  
  
/* Uncomment the line below according to the target STM32L4 device used in your  
   application  
  */  
  
#if !defined (STM32L412xx) && !defined (STM32L422xx) && \  
    !defined (STM32L431xx) && !defined (STM32L432xx) && !defined (STM32L433xx) && !defined (STM32L442xx) && !defined (STM32L443xx) && \  
    !defined (STM32L451xx) && !defined (STM32L452xx) && !defined (STM32L462xx) && \  
    !defined (STM32L471xx) && !defined (STM32L475xx) && !defined (STM32L476xx) && !defined (STM32L485xx) && !defined (STM32L486xx) && \  
    !defined (STM32L496xx) && !defined (STM32L4A6xx) && \  
    !defined (STM32L4R5xx) && !defined (STM32L4R7xx) && !defined (STM32L4R9xx) && !defined (STM32L4S5xx) && !defined (STM32L4S7xx) && !defined (STM32L4S9xx)  
  /* #define STM32L412xx */   /*!< STM32L412xx Devices */  
  /* #define STM32L422xx */   /*!< STM32L422xx Devices */  
  /* #define STM32L431xx */   /*!< STM32L431xx Devices */  
#define STM32L432xx	          /*!< STM32L432xx Devices */  
  /* #define STM32L433xx */   /*!< STM32L433xx Devices */  
  /* #define STM32L442xx */   /*!< STM32L442xx Devices */  
  /* #define STM32L443xx */   /*!< STM32L443xx Devices */  
  
7) open Makefile and update with  flash target  
   update the <hexfile> with current name  
  
#######################################  
# flash  
#######################################  
flash:  
	ST-LINK_CLI.exe -c SWD -P ./build/<hexfile>.hex -V -Rst  
  
flash_ocd:  
	openocd -f stlink_v2.cfg -c "program build/<hexfile>.hex verify reset exit"  
  
8) open terminal with msys bash and run make to compile  
  

