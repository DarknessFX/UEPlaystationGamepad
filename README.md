     .----------------.  .----------------.  .----------------. 
    | .--------------. || .--------------. || .--------------. |
    | |  ________    | || |  _________   | || |  ____  ____  | |
    | | |_   ___ `.  | || | |_   ___  |  | || | |_  _||_  _| | |
    | |   | |   `. \ | || |   | |_  \_|  | || |   \ \  / /   | |
    | |   | |    | | | || |   |  _|      | || |    > `' <    | |
    | |  _| |___.' / | || |  _| |_       | || |  _/ /'`\ \_  | |
    | | |________.'  | || | |_____|      | || | |____||____| | |
    | |              | || |              | || |              | |
    | '--------------' || '--------------' || '--------------' |
     '----------------'  '----------------'  '----------------' 

           DarknessFX @ https://dfx.lv | Twitter: @DrkFX

# Playstation Gamepad For Unreal Engine 5.2

<img src="https://repository-images.githubusercontent.com/591609859/7d082190-0d09-44dd-9c24-2451a1da4dc7" width="640px" /> <br/>

Playstation Gamepad For Unreal Engine 5.2 using Raw Input and Enhanced Input. This project is setup to enable DualShock3/Sixaxis, DualShock4 and DualSense controllers to bind with Unreal Engine Gamepad object, you can use your Playstation controllers for Gameplay, CommonUI, EnhancedInput with the same Gamepad code like other XInput gamepads instead of fork to GenericUSBController.<br/>

## About

Originally to make DualShock 4 work with Unreal Engine 4 was just a list of RawInputWindows Plugin settings and was simpler to describe like in this forum post <a href="https://forums.unrealengine.com/t/tutorial-ue4-using-dualshock4-controller-via-usb-ps4-ds4-gamepad/133314" target="_blank">[Tutorial] UE4 using Dualshock4 controller (via USB, PS4 DS4 Gamepad)</a>. <br/><br/>
Now that Unreal Engine 5.2 is moving the input system to Enhanced Input there are more files and structures to setup and isn't that simple to explain in a forum post, so this project is the easier way to share this information and give a demonstration template working that can be copied to other projects. <br/>

## Notes

- You can have all 3 controllers connected to your PC USB.
- Basic Enhanced Input Action setup (only Buttons and Axes - Pressed and Released), for more information how to expand and create more Input Actions check <a href="https://docs.unrealengine.com/5.1/en-US/enhanced-input-in-unreal-engine/" target="_blank">Enhanced Input - An overview of the Enhanced Input Plugin.</a> and <a href="https://dev.epicgames.com/community/learning/tutorials/eD13/unreal-engine-enhanced-input-in-ue5" target="_blank">Enhanced Input in UE5 - Official Tutorial</a>.

## RawInput Changes

Download and update this files from RawInput Plugin:<br/>
<a href="https://raw.githubusercontent.com/DarknessFX/UEPlaystationGamepad/main/RawInput_Plugin/RawInputWindows.h" target="_blank">RawInputWindows.h</a><br/>
<a href="https://raw.githubusercontent.com/DarknessFX/UEPlaystationGamepad/main/RawInput_Plugin/RawInputWindows.cpp" target="_blank">RawInputWindows.cpp</a><br/>
<br/>
Compile RawInput plugin with this updates to apply fixes and make full use of Playstation Gamepads as Native Unreal Engine Gamepads.

## Credits

Unreal Engine from Epic Games - https://www.unrealengine.com/ <br/>
Playstation DualShock4 Icons by Arks - https://arks.itch.io/ps4-buttons <br/>
<a href="https://github.com/Equ1no0x">Equ1no0x</a> - DualShock4 Gen1 contribution. <br/>

## License

@Unlicensed - Free for everyone and any use. <br/><br/>
DarknessFX @ <a href="https://dfx.lv" target="_blank">https://dfx.lv</a> | Twitter: <a href="https://twitter.com/DrkFX" target="_blank">@DrkFX</a> <br/>https://github.com/DarknessFX/UEPlaystationGamepad
