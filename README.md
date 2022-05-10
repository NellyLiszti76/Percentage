# Percentage
#include &lt;array.au3> #include &lt;GUIConstantsEx.au3> #include &lt;ProgressConstants.au3>  Global $Title = "7zip progress echo demo" $hGUI = GUICreate($Title, 320, 80,-1,-1) $progressbar = GUICtrlCreateProgress(10, 10, 300, 30) $btn = GUICtrlCreateButton("Start", 125, 45, 70, 30)    GUISetState()  While 1     $nMsg = GUIGetMsg()     Switch $nMsg         Case $GUI_EVENT_CLOSE             Exit         Case $btn             $ExtrTarget = 'E:\DP'                  ;Release path                                                             $ExtrSource = 'E:\mtest.7z'            ;Target file                                                            $UnPack_line = ' x -y -o' &amp; '"'&amp; $ExtrTarget &amp; '"' &amp; ' ' &amp; '"'&amp; $ExtrSource &amp; '"'             _GetPIODataSend2Bar('7za.exe', $UnPack_line, FileGetSize($ExtrSource), $progressbar)             GUICtrlSetData($progressbar, 100)             MsgBox(0, '', 'Successful release')             GUICtrlSetData($progressbar, 0)             GUICtrlSetData($hGUI, "")             Exit     EndSwitch WEnd  Func _GetPIODataSend2Bar($Execute, $Commandline, $Param, $Ctrl)     Local $Pid, $PIOData, $iPercentage, $iPercentageBefore     $Pid = Run ($Execute &amp; $Commandline, '', @SW_HIDE)     While ProcessExists($Pid)         $PIOData = ProcessGetStats($Execute, 1)         If @error Then             MsgBox(0, '', $Execute)         Else             $iPercentage = Round($PIOData[3]/$Param*100)             If $iPercentage &lt;> $iPercentageBefore And $iPercentage > 0 And $iPercentage &lt;= 100 Then                ConsoleWrite('->-' &amp; $iPercentage &amp; @CRLF)                GUICtrlSetData($Ctrl , $iPercentage)                WinSetTitle($hGUI, "", $Title &amp; " " &amp; $iPercentage &amp; "%")             EndIf         EndIf     WEnd     Return $iPercentage EndFunc
