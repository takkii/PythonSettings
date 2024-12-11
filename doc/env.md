### Windows11/WSL2, I can launch Sublime Debugger.

> bash.exe -c "ip r |tail -n1|cut -d ' ' -f9"

※ Change to the IP address (v4) specified by your WSL2

#### Check python's parent process ID at startup
```markdown
import os

# Get parent process of python
pid = os.getppid()

# Need to check at startup
print(f"Parent Process ID: {pid}")
```
### Add this to the top of your python file to start debugging
```python
import debugpy
import os

from typing import Optional```
def debug_wait_for_attach(listen_to):
    scoop = os.path.expanduser('~/scoop/apps/python/current/python.exe')
    pyenv: Optional[str] = os.path.expanduser('/.pyenv/shims/python')
    ay: Optional[str] = os.path.expanduser('/.anyenv/envs/pyenv/shims/python')
    
    if os.path.exists(os.path.expanduser(scoop)):
        # use scoop
        debugpy.configure(python=str(scoop))
        debugpy.listen(listen_to)
        debugpy.wait_for_client()
    elif os.path.exists(pyenv):
        # ~/.pyenv
        debugpy.configure(python=str(pyenv))
        debugpy.listen(listen_to)
        debugpy.wait_for_client()
    elif os.path.exists(ay):
        # ~/.anyenv
        debugpy.configure(python=str(ay))
        debugpy.listen(listen_to)
        debugpy.wait_for_client()
```
※ scoop/.pyenv/anyenv, add other paths yourself

> python -m debugpy --wait-for-client --listen 5678 facematch.py ${@:2}

※ Windows environment

> python -m debugpy --wait-for-client --listen 0.0.0.0:5678 facematch.py ${@:2}

※ WSL2 environment

>  C:\Users\user\AppData\Roaming\Sublime Text\Packages\SublimeDebugger

#### Preference(基本設定) / Packagesフォルダ

>  git clone https://github.com/takkii/PythonSettings.git

#### new Build System / ビルドシステム追加

> Python3.sublime-build

```json
    "debugger_tasks" : [
    {
        "name" : "Python Debugger run task",
        "type" : "python",
        "settings": {
            "enabled": true,
            "command":
            [
                "C:/Users/user/scoop/apps/python/current/python.exe", "$file"
            ],
            "selector": "source.python | text.html.python",
        },
    	"platform": "windows",
    	"file_regex": "^\\s*File \"(...*?)\", line ([0-9]*)",
    	"PYTHONIOENCODING": "utf-8",
    },
    {
        "name": "Python Debug Symbols",
        "type" : "python",
        "settings": {
            "enabled": true,
            "command":
            [
                "C:/Users/user/scoop/apps/python/current/python.exe", "-D", "$file"
            ],
            "selector": "source.python | text.html.python",
        },
        "platform": "windows",
        "file_regex": "^\\s*File \"(...*?)\", line ([0-9]*)",
        "PYTHONIOENCODING": "utf-8",
    }
```

#### project_name.sublime-project

```json
{
	"folders":
	[
		{
			"path": ".",
		}
	],
	"debugger_configurations":
	[
        {
            "name": "Python Debugger: Attach remote debug.",
            "type": "python",
            "request": "attach",
            "connect": {
                "host": "localhost",
                "port": 5678
            },
            "justMyCode" : false,
        },
        {
            "name": "Python Debugger: WSL2 Attach remote debug.",
            "type": "python",
            "request": "attach",
            "connect": {
                // My WSL2 IP Adress
                "host": "",
                "port": 5678
            },
            "justMyCode" : false,
        },
        {
            "name" : "Python Debugger: Attach using process Id",
            "type" : "python",
            "request" : "attach",
            "selector": "source.python | text.html.python",
            "debugServer": 5678,
            // At startup, check.
            "processId": "",
            "pathMappings": [{
                "localRoot": "${workspaceFolder}",
                "remoteRoot": "."
            }],
            "justMyCode" : false,
        },
        {
            "name": "Python Debugger: Current File",
            "type": "sublime",
            "platform": "windows",
            "request": "launch",
            "selector": "source.python | text.html.python",
            "program": "\\${file}",
            "console": "integratedTerminal",
            "justMyCode" : false,
        },
	],
}
```
