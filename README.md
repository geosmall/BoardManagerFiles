# STM32 Robotics Core - Board Manager

Arduino Board Manager package index for **STM32 Robotics Core** - A comprehensive Arduino core for STM32 microcontrollers with specialized libraries for UAV flight controller development.

## Quick Install

### Arduino IDE

1. Open **Arduino IDE**
2. Go to **File → Preferences**
3. Add this URL to **"Additional Boards Manager URLs"**:
   ```
   https://github.com/geosmall/BoardManagerFiles/raw/main/package_stm32_robotics_index.json
   ```
4. Go to **Tools → Board → Boards Manager**
5. Search for **"STM32 Robotics"**
6. Click **Install**

### Arduino CLI

```bash
# Add Board Manager URL
arduino-cli config add board_manager.additional_urls \
  https://github.com/geosmall/BoardManagerFiles/raw/main/package_stm32_robotics_index.json

# Update index and install
arduino-cli core update-index
arduino-cli core install STM32_Robotics:stm32

# List available boards
arduino-cli board listall STM32_Robotics
```

## What's Included

### Core Features

- **STM32 Arduino Core** - Fork of [stm32duino](https://github.com/stm32duino/Arduino_Core_STM32) with robotics-focused enhancements
- **CI/HIL Testing Framework** - Build traceability with Git SHA + UTC timestamps
- **SEGGER RTT Support** - Real-time debugging and logging without hardware serial
- **BoardConfig System** - Multi-board hardware abstraction for easy porting

### Robotics Libraries

#### Communication & Control
- **SerialRx** - RC receiver protocol parser (IBus ✅, SBUS ✅, CRSF 📋)
  - Hardware-validated with FlySky FS-iA6B
  - Software idle detection for frame synchronization
  - BoardConfig integration

- **TimerPWM** - Hardware timer-based PWM for servos and ESCs
  - 1 MHz resolution (1µs pulse width control)
  - Multi-channel support (up to 4 per timer)
  - Dual timer operation (servos + ESCs simultaneously)
  - Hardware validated with input capture

#### Sensors
- **imu** - High-level C++ wrapper for InvenSense IMU sensors
  - Multi-instance support with context-based design
  - Chip detection (ICM-42688-P, MPU-6000, MPU-9250)
  - Interrupt support (EnableDataReadyInt1)

- **ICM42688P** - Low-level ICM-42688-P 6-axis IMU driver
  - 100% InvenSense factory algorithms preserved
  - Manufacturer self-test with bias calculation
  - Interrupt-driven data acquisition at 1kHz
  - Clock calibration for processed sensor data

#### Storage
- **LittleFS** - SPI flash filesystem with wear leveling
  - Support for 20+ flash chips (Winbond, GigaDevice, ISSI, etc.)
  - Complete file I/O and directory operations

- **SDFS** - SD card filesystem via SPI (FatFs backend)
  - LittleFS-compatible API for seamless switching
  - Runtime detection of card capacity and sector size

- **Storage** - Unified storage abstraction layer
  - Single API for LittleFS or SDFS backends
  - Automatic backend selection via BoardConfig

- **minIniStorage** - INI configuration file management
  - String/int/float/bool support
  - Section and key enumeration
  - Storage backend integration

#### Utilities
- **libPrintf** - Embedded printf library (eyalroz/printf v6.2.0)
  - Eliminates nanofp confusion
  - ~20% binary size reduction (8KB+ savings)
  - Float formatting works correctly

- **STM32RTC** - Real-time clock functionality

### Development Tools

- **Betaflight Config Converter** - Convert Betaflight unified target configs to Arduino BoardConfig headers
  - 53 passing tests with pytest validation
  - PeripheralPins.c cross-validation
  - ALT variant handling for timer/AF conflicts
  - Motor timer grouping for PWM

- **Build Scripts** - Automated build and HIL testing
  - `build.sh` - Compile with environment validation
  - `aflash.sh` - Build + flash + HIL test automation
  - Device auto-detection (50+ STM32 device IDs)

- **AUnit Testing** - Unit testing framework (v1.7.1)
  - RTT/Serial dual-mode support
  - 18 total tests validating all storage libraries
  - Hardware validation on real SPI flash and SD cards

## Supported Boards

### Development Boards
- **NUCLEO-F411RE** (Primary)
  - STM32F411RE @ 100MHz
  - 512KB Flash, 128KB RAM
  - On-board ST-Link (can be reflashed to J-Link for HIL)

- **BlackPill F411CE** (Secondary)
  - STM32F411CE @ 100MHz
  - 512KB Flash, 128KB RAM
  - Compact form factor

### Flight Controller Boards
- **NOXE V3** (JHEF411)
  - STM32F411CE-based flight controller
  - 5 motor outputs, SPI flash, dual SPI buses
  - Generated via Betaflight converter

## Hardware Validation

All libraries have been validated on real hardware:

### Storage Testing
- **LittleFS**: W25Q128JV-Q SPI flash (8/8 tests passed, 100+ assertions)
- **SDFS**: SD card via SPI (7/7 tests passed)
- **Generic Storage**: Unified abstraction (all tests passed)
- **minIniStorage**: Configuration management (6 test suites passed)

### IMU Testing
- **ICM42688P**: 6-axis IMU with complete test suite
  - WHO_AM_I verification ✅
  - Manufacturer self-test (gyro + accel PASS) ✅
  - Interrupt-driven data at 1kHz ✅
  - Processed data with clock calibration ✅

- **imu wrapper**: High-level API validation
  - Self-test integration ✅
  - Interrupt-driven data acquisition ✅

### Communication Testing
- **SerialRx IBus**: FlySky FS-iA6B receiver
  - 501/501 frames (0% loss) in loopback test
  - Real receiver: 15s continuous, 10 channels, 1000-2000µs range

### PWM Testing
- **TimerPWM**: Hardware validated with TIM2 input capture
  - Single timer: 49.50 Hz measured (±2% tolerance) ✅
  - Dual timer: 49.50 Hz servo + 990.10 Hz ESC simultaneous ✅

## Development Repository

**Source Code**: https://github.com/geosmall/Arduino_Core_STM32
- **Branch**: `dev` (consolidated development branch)
- **Documentation**: Comprehensive `CLAUDE.md` with project overview
- **Session Notes**: `SESSION_NOTES.md` with development history

## License

BSD-3-Clause (same as upstream stm32duino)

## Support

- **Issues**: https://github.com/geosmall/Arduino_Core_STM32/issues
- **Discussions**: https://github.com/geosmall/Arduino_Core_STM32/discussions

## Migration from Legacy Packages

If you previously used `package_uvos_stm32_index.json` or `package_stm32_ll_index.json`:

1. These packages have been deprecated and replaced by `package_stm32_robotics_index.json`
2. Uninstall old packages via Board Manager
3. Follow the installation instructions above for the new STM32 Robotics Core
4. All functionality is preserved and enhanced in the new unified package

## Credits

- **Upstream**: [stm32duino](https://github.com/stm32duino/Arduino_Core_STM32) - STM32 Arduino Core
- **Development**: Collaborative work with Claude Code (Anthropic)
- **Libraries**: Various open-source projects (see individual library credits)
