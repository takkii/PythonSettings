### Windows11/WSL2, I can launch Sublime Debugger.

> bash.exe -c "ip r |tail -n1|cut -d ' ' -f9"

※ Change to the IP address (v4) specified by your WSL2

> python -m debugpy --wait-for-client --listen 5678 facematch.py ${@:2}

※ Windows environment

> python -m debugpy --wait-for-client --listen 0.0.0.0:5678 facematch.py ${@:2}

※ WSL2 environment

