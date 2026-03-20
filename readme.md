# ZMK Config – 开发中 ⚙️
本项目用于开发和调试自定义 ZMK 键盘配置，目前仍在搭建结构阶段，固件尚未准备完毕，请勿用于实际构建。

## 📁 项目结构说明
```text
vam05_rev_a 2025年8月16日修改:修改了三色led只显示层不显示电池。点击BT_CLR清除当前连接，点击BT_CLR_ALL清除全部连接 capslock 橙灯
├── build.yaml                # 告诉 GitHub 编译哪个板子（暂未启用构建时可改为 build.yml.disabled）
├── config/
│   ├── mykeyboard.keymap     # 定义键位和功能层（✅）
│   ├── mykeyboard.conf       # 设置编译参数和功能开关（✅）
│   ├── west.yml              # 指定使用哪个 ZMK 主仓库（✅）
│   └── boards/
│       └── arm/
│           └── mykeyboard/                 #
│               ├── mykeyboard.yaml         #1   Zephyr 主板定义文件（✅）
│               ├── mykeyboard_defconfig    #2   构建这个板子固件时，自动启用哪些功能（✅）
│               ├── mykeyboard.dts          #3   设备树（✅）
│               ├── mykeyboard-pinctrl.dtsi #4-1 pinctrl dts 片段（❌已经集成进dts文件里，这个就不用单独做一个文件里）
│               ├── mykeyboard-layouts.dtsi #4-2 layouts dts 片段（✅）
│               ├── board.cmake             #5   告诉 west 和 CMake 这个板子可以用哪些“runner”（烧录器或调试器）来上传固件或调试程序（✅）
│               ├── Kconfig.board           #6-1 注册板子标识名用的.不写功能不涉及引脚配置，仅告诉 Zephyr “我这个板子叫 XXX，用的是 nRF52840”（✅）
│               ├── Kconfig.defconfig       #6-2 为你的自定义板子（BOARD）设置默认的 Kconfig 配置值（✅）
│               ├── Kconfig                 #7   处理DCDC（做了但能不能用不知道，这个文件corn有4pp没有）（✅）
│               ├── 
│               ├── 
│               ├── 
│               ├── mykeyboard.keymap       #-1 默认键位模板，理论上不参与构建（✅）
│               └── ...         
│    
└── .github/
    └── workflows/
        └── build.yml         # CI 自动构建脚本（建议开发完成后再启用）


### 🚀 一行推送指令（开发中使用，不触发构建）

```bash 一键推送项目并保存
git add . && git commit -m "wip: update config" && git push

```最后要构建的时候，把
build.yml.disabled 文件名 改回 build.yml


备忘录
1. DCDC 其他案例的处理：https://github.com/search?q=repo%3Azmkfirmware%2Fzmk+dcdc&type=code

GPIO对应
引脚号	引脚名称	已连接器件 / 功能说明
P0.00	XL1	晶振输入
P0.01	XL2	晶振输出

P0.02	COL8（键盘矩阵）
P0.03	COL5（键盘矩阵）

P0.04	Extra_LED_Color1（额外三色LED）
P0.05	SDA（I2C）

P0.06	Layer_LED_G（三色LED）
P0.07	wk（唤醒功能）
P0.08	Layer_LED_R（三色LED）

P0.09	COL0（键盘矩阵）
P0.10	ROW4（键盘矩阵）

P0.12	Extra_LED_Color2（额外三色LED）
P0.13	UG_LV（接 WS2812 数据输入）

P0.15	COL3（键盘矩阵）
P0.17	ROW3（键盘矩阵）

P0.18	RESET	复位引脚

P0.20	ROW2（键盘矩阵）
P0.22	ROW1（键盘矩阵）

P0.24	UG_EN（WS2812 电源控制）
P0.26	Layer_LED_B（三色LED）

P0.28	COL6（键盘矩阵）
P0.29	COL9（键盘矩阵）
P0.30	COL11（键盘矩阵）
P0.31	COL10（键盘矩阵）

P1.00	ROW0（键盘矩阵）
P1.02	Batt_LED_B（电源三色LED）
P1.04	Batt_LED_G（电源三色LED）
P1.06	COL1（键盘矩阵）
P1.09	SCL（I2C）

P1.10	COL4（键盘矩阵）
P1.11	COL2（键盘矩阵）
P1.13	COL7（键盘矩阵）

---------矩阵---------
P1.00	ROW0（键盘矩阵）
P0.22	ROW1（键盘矩阵）
P0.20	ROW2（键盘矩阵）
P0.17	ROW3（键盘矩阵）
P0.10	ROW4（键盘矩阵）

P0.09	COL0（键盘矩阵）
P1.06	COL1（键盘矩阵）
P1.11	COL2（键盘矩阵）
P0.15	COL3（键盘矩阵）
P1.10	COL4（键盘矩阵）
P0.03	COL5（键盘矩阵）
P0.28	COL6（键盘矩阵）
P1.13	COL7（键盘矩阵）
P0.02	COL8（键盘矩阵）
P0.29	COL9（键盘矩阵）
P0.31	COL10（键盘矩阵）
P0.30	COL11（键盘矩阵）

---------灯---------
P1.02	Batt_LED_B（电源三色LED）
P1.04	Batt_LED_G（电源三色LED）

P0.08	Layer_LED_R（三色LED）
P0.06	Layer_LED_G（三色LED）
P0.26	Layer_LED_B（三色LED）

P0.04	Extra_LED_Color1（额外三色LED）
P0.12	Extra_LED_Color2（额外三色LED）

P0.13	UG_LV（接 WS2812 数据输入）
P0.24	UG_EN（WS2812 电源控制）

------RGB indicator-----
| Color        | Value |
| ------------ | ----- |
| Black (none) | `0`   |
| Red          | `1`   |
| Green        | `2`   |
| Yellow       | `3`   |
| Blue         | `4`   |
| Magenta      | `5`   |
| Cyan         | `6`   |
| White        | `7`   |
