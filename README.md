valorant triggerbot here change as needed made using chat gpt #NoEnv
#Persistent
#MaxThreadsPerHotkey 2
#KeyHistory 0
ListLines Off
SetBatchLines, -1
SetKeyDelay, -1, -1
SetMouseDelay, -1
SetDefaultMouseSpeed, 0
SetWinDelay, -1
SetControlDelay, -1
SendMode Input
CoordMode, Pixel, Screen
SoundBeep, 300, 200
SoundBeep, 400, 200

; Configuration
key_stay_on := "Up"
key_hold_mode := "Right"
key_fastclick := "Left"
key_off := "Down"
key_gui_hide := "Home"
key_exit := "End"
key_hold := "XButton1"

pixel_box := 6
pixel_sens := 65
pixel_color := 0xFEFE40
tap_time := 1  ; Minimal tap time for fast clicking

; GUI Setup
Gui, 2:+LastFound +ToolWindow +AlwaysOnTop -Caption
Gui, 2:Font, Cdefault, Fixedsys
Gui, 2:Color, Black
Gui, 2:Color, EEAA99
Gui, 2:Add, Progress, x10 y20 w100 h23 Disabled BackgroundBlue vC3
Gui, 2:Add, Text, xp yp wp hp cWhite BackgroundTrans Center 0x200 vB3 gStart, On
Gui, 2:Add, Progress, x10 y20 w100 h23 Disabled BackgroundBlue vC2
Gui, 2:Add, Text, xp yp wp hp cWhite BackgroundTrans Center 0x200 vB2 gStart, Hold mode
Gui, 2:Add, Progress, xp yp wp hp Disabled BackgroundRED vC1
Gui, 2:Add, Text, xp yp wp hp cWhite BackgroundTrans Center 0x200 vB1 gStart, Off
Gui, 2:Show, x10 y1 w200 h60
WinSet, TransColor, EEAA99

; Hotkeys
Hotkey, %key_stay_on%, StayOn
Hotkey, %key_hold_mode%, HoldMode
Hotkey, %key_off%, OffLoop
Hotkey, %key_gui_hide%, GuiHide
Hotkey, %key_exit%, Terminate
Hotkey, %key_fastclick%, FastClick

; Initial GUI state
GuiControl, 2:Hide, B1
GuiControl, 2:Hide, C1
GuiControl, 2:Hide, B2
GuiControl, 2:Hide, C2
GuiControl, 2:Show, B3
GuiControl, 2:Show, C3

return

; Functions

Start:
    Gui, 2:Submit, NoHide
    return

Terminate:
    SoundBeep, 300, 200
    SoundBeep, 200, 200
    Sleep 400
    ExitApp
    return

StayOn:
    SoundBeep, 300, 200
    SetTimer, Loop1, 10  ; Fast timer interval
    SetTimer, Loop2, Off
    GuiControl, 2:Hide, B1
    GuiControl, 2:Hide, C1
    GuiControl, 2:Hide, B2
    GuiControl, 2:Hide, C2
    GuiControl, 2:Show, B3
    GuiControl, 2:Show, C3
    return

HoldMode:
    SoundBeep, 300, 200
    SetTimer, Loop1, Off
    SetTimer, Loop2, 10  ; Fast timer interval
    GuiControl, 2:Hide, B1
    GuiControl, 2:Hide, C1
    GuiControl, 2:Show, B2
    GuiControl, 2:Show, C2
    GuiControl, 2:Hide, B3
    GuiControl, 2:Hide, C3
    return

OffLoop:
    SoundBeep, 300, 200
    SetTimer, Loop1, Off
    SetTimer, Loop2, Off
    GuiControl, 2:Show, B1
    GuiControl, 2:Show, C1
    GuiControl, 2:Hide, B2
    GuiControl, 2:Hide, C2
    GuiControl, 2:Hide, B3
    GuiControl, 2:Hide, C3
    return

GuiHide:
    GuiControl, 2:Hide, B1
    GuiControl, 2:Hide, C1
    GuiControl, 2:Hide, B2
    GuiControl, 2:Hide, C2
    GuiControl, 2:Hide, B3
    GuiControl, 2:Hide, C3
    return

Loop1:
    PixelSearch()
    return

Loop2:
    While GetKeyState(key_hold, "P") {
        PixelSearch()
    }
    return

FastClick:
    SoundBeep, 300, 200
    toggle := !toggle
    return

#if toggle
*~$LButton::
    Sleep 50  ; Reduced sleep interval for fast clicking
    While GetKeyState("LButton", "P") {
        Click
        Sleep 5  ; Reduced sleep interval for faster clicking
    }
    return
#if

PixelSearch() {
    global
    PixelSearch, FoundX, FoundY, A_ScreenWidth/2 - pixel_box, A_ScreenHeight/2 - pixel_box, A_ScreenWidth/2 + pixel_box, A_ScreenHeight/2 + pixel_box, pixel_color, pixel_sens, Fast RGB
    If !ErrorLevel {
        If !GetKeyState("LButton") {
            Click
            Sleep %tap_time%
        }
    }
}
