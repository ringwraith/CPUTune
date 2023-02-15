
CPUTune
====
An open source kernel extension enables dynamic CPU performance tuning at runtime for macOS.

https://portugnole.blogspot.com/2020/05/running-macbook-without-battery-lets-go.html

#### Features
- Allows turning on or off Intel Turbo Boost for better battery life
- Allows turning on or off Intel Speed Shift for maximum performance
- Provides the fine granularity to change HWP (performance value) at runtime
- Provides the control over tuning turbo ratio limit at runtime when turbo boost is enabled
- Allows turning on or off Intel Proc Hot (```BD_PROCHOT```) for running without a battery. Please note that this is NOT recommended and can seriously damage your mac. Do at your own risk.
- Implements TimerEvent-based responses for dynamical switching Turbo Boost and Speed Shift at runtime
- Allows System Integrity Protection (SIP) control a bit easier via Info.plist setting 


#### Installation
- [Disable the System Integrity Protection](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection), reboot
- Download the repo and open the `CPUTuneCore/CPUTuneCore.xcodeproj` via Xcode
- Navigate to `Product` then click `Build`. Once the compiled completed, you will find `CPUTuneCore.kext` in the `Product` folder, copy it to `/Library/Extensions`
- Change the `CPUTuneCore.kext` ownership to `root:wheel`, mode to `rwxr-xr-x` (or `755`)
- Rebuild the kernel cache in terminal: `sudo touch /Library/Extensions; sudo kextcache -i /`, reboot
- [Enable the SIP](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection) back and reboot

#### Boot arguments
- Add `-cputoff` to disable CPUTune
- Add `-cputbeta` to enable CPUTune on unsupported os versions (10.15 and below are enabled by default).

#### Configuration
Open terminal:
- Type in ```echo 1>/tmp/CPUTuneTurboBoostRT.conf``` to enable turbo boost when CPUTune is loaded
- Type in ```echo 0>/tmp/CPUTuneTurboBoostRT.conf``` to disable turbo boost when needed
- Type in ```echo <request value> >/tmp/HWPRequest.conf``` to submit persistency hwp request at runtime. For example ```<requet value> = 0x80193008```
- Type in ```echo <turbo ratio limit> >/tmp/TurboRatioLimit.conf``` to submit cores maximum frequency at runtime. For example ```0x2b2c2d2f3030``` in ```i9-8950HK (6 cores)``` sets the maximum frequency for Core[1-6] at 43(0x2b), 44(0x2c), 45(0x2d), 46(0x2f), 48(0x30), 48(0x30) respectively.
- Type in  ```echo 1>/tmp/CPUTuneProcHotRT.conf``` to enable proc hot when needed
- Type in  ```echo 0>/tmp/CPUTuneProcHotRT.conf``` to disable proc hot when needed
- Change update time interval (millisecond) in `CPUTune.kext/Contents/Info.plist` to have a more  looser/tigher control over HWP request
- In case you want a simplify command to switch turbo boost, change the `TurboBoostAtRuntime` in `CPUTune.kext/Contents/Info.plist`

#### Contribution
All suggestions and improvements are welcome, don't hesitate to pull request or open an issue if you want this project to be better than ever.
Writing and supporting code is fun but it takes time. Please provide most descriptive bugreports or pull requests.


#### Change Log
[Change logs](https://github.com/syscl/CPUTune/blob/master/Changelog.md) for detail improvements

