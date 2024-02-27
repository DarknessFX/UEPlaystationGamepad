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
Now that Unreal Engine 5.2 is moving the input system to Enhanced Input there are more files and structures to setup and isn't that simple to explain in a forum post, so this project is the easier way to share this information and give a demostration template working that can be copied to other projects. <br/>

## Notes

- You can have all 3 controllers connected to your PC USB.
- If you have a DualShock4 1st Generation (Product ID 0x05C4) the project will inform you with a screen message, I don't have one to test but if you have (and want to contribute) send the RawInput Settings to me.
- Basic Enhanced Input Action setup (only Buttons and Axes - Pressed and Released), for more information how to expand and create more Input Actions check <a href="https://docs.unrealengine.com/5.1/en-US/enhanced-input-in-unreal-engine/" target="_blank">Enhanced Input - An overview of the Enhanced Input Plugin.</a> and <a href="https://dev.epicgames.com/community/learning/tutorials/eD13/unreal-engine-enhanced-input-in-ue5" target="_blank">Enhanced Input in UE5 - Official Tutorial</a>.

## DPad compatibility with CommonUI

Playstation gamepads DPad use Axis1D directional inputs, the Axis1D register from 0.0f to 8.0f : 0=Top, 2=Right, 4=Bottom, 6=Left, 8=No Input.<br/><br/>
Unreal Engine don't expect DPad directions from axis values, only as buttons pressed/released, to enable compatibility with CommonUI, EnhancedInput and other XInput gamepads you need to modify the *RawInput Plugin* with the following changes:<br/><br/>
If you prefer you can download my source files from:<br/>
<a href="https://github.com/DarknessFX/UEPlaystationGamepad/tree/main/.git_files/RawInputWindows.h" target="_blank">RawInputWindows.h</a><br/>
<a href="https://github.com/DarknessFX/UEPlaystationGamepad/tree/main/.git_files/RawInputWindows.cpp" target="_blank">RawInputWindows.cpp</a><br/>
<br/>
*Engine\Plugins\Experimental\RawInput\Source\RawInput\Public\Windows\RawInputWindows.h*<br/>
*Line 329 at the end of the file, add* :<br/>
```c++
//Playstation DPad
FName DPadMap[7] = {};
struct PlaystationID {
	int32 vID; // VendorID
	int32 pID; // ProductID
	int32 aID; // Array Index
} psID[3];
```
<br/>

*Engine\Plugins\Experimental\RawInput\Source\RawInput\Private\Windows\RawInputWindows.cpp*<br/>
*Line 84, inside void FRawWindowsDeviceEntry::InitializeNameArrays(), add* : <br/>
```c++
	DPadMap[0] = FGamepadKeyNames::DPadUp;
	DPadMap[2] = FGamepadKeyNames::DPadRight;
	DPadMap[4] = FGamepadKeyNames::DPadDown;
	DPadMap[6] = FGamepadKeyNames::DPadLeft;

	psID[0] = { 1356, 1476, 4 }; // DS4 GEN1
	psID[1] = { 1356, 2508, 4 }; // DS4 GEN2
	psID[2] = { 1356, 3302, 7 }; // DualSense
```
<br/>

*Line 1049, inside if (DeviceEntry.bNeedsUpdate) { before the endif }, add* : <br/>
```c++
			for (PlaystationID& ps : psID) {
				if (DeviceEntry.DeviceData.VendorID == ps.vID && DeviceEntry.DeviceData.ProductID == ps.pID) {
					FAnalogData* DPadAxis1D = &DeviceEntry.AnalogData[ps.aID];
					int iPrevValue = FMath::FloorToInt(DPadAxis1D->PreviousValue);
					int iValue = FMath::FloorToInt(DPadAxis1D->Value);
					bool bIsRepeat = iValue == iPrevValue;

					if (!bIsRepeat) {
						if (iPrevValue != 8) {
							if (iPrevValue % 2 == 1) {
								MessageHandler->OnControllerButtonReleased(DPadMap[iPrevValue - 1], UserId, DeviceId, bIsRepeat);
								iPrevValue++;
								if (iPrevValue == 8) iPrevValue = 0;
							}
							MessageHandler->OnControllerButtonReleased(DPadMap[iPrevValue], UserId, DeviceId, bIsRepeat);
						}

						if (iValue != 8) {
							if (iValue % 2 == 1) {
								MessageHandler->OnControllerButtonPressed(DPadMap[iValue - 1], UserId, DeviceId, bIsRepeat);
								iValue++;
								if (iValue == 8.0f) iValue = 0.0f;
							}
							MessageHandler->OnControllerButtonPressed(DPadMap[iValue], UserId, DeviceId, bIsRepeat);
						}
						DPadAxis1D->PreviousValue = DPadAxis1D->Value;
					}
				}
			}
		}
	}
}
```

## Credits

Unreal Engine from Epic Games - https://www.unrealengine.com/ <br/>
Playstation DualShock4 Icons by Arks - https://arks.itch.io/ps4-buttons <br/>

## License

@Unlicensed - Free for everyone and any use. <br/><br/>
DarknessFX @ <a href="https://dfx.lv" target="_blank">https://dfx.lv</a> | Twitter: <a href="https://twitter.com/DrkFX" target="_blank">@DrkFX</a> <br/>https://github.com/DarknessFX/UEPlaystationGamepad
