VBoxTool
========

## Introduction
VBoxTool currently consist only of a set of scripts. With this scripts, virtual
machines of VirtualBox in a Linux headless server can be controlled. Start,
stop, save, backup and show status of sessions in batch mode from the command
line.

Usage and installation is tested only on Ubuntu. Please report if a specific 
function is not working in another environment, say OpenSUSE, Fedora, etc.

## Installation

> **Note.** Precede commands with 'sudo' when not operated as root.

* Place the main script `script/vboxtool` in `/usr/local/bin`
```bash
    cp script/vboxtool /usr/local/bin/vboxtool
```

* Make vboxtool executable: 
```bash
    chmod +x /usr/local/bin/vboxtool
```

* Place the init script `script/vboxtoolinit` in `/etc/init.d`
```bash
    cp script/vboxtoolinit /etc/init.d/vboxtoolinit
```

* Make vboxtoolinit executable: 
```bash
    chmod +x /etc/init.d/vboxtoolinit
```
  
* Activate the init script vboxtoolinit:
```bash
    update-rc.d vboxtoolinit defaults 99 10
```
  
* Create a folder `/etc/vboxtool`. In here, two config files have to be
  created, see configuration section below, type 'vboxtool help' for more
instructions.
  
> **Note.** To remove vboxtoolinit from autostart:
> ```bash
> update-rc.d -f vboxtoolinit remove
> ```

## Configuration

> **Note.** Configuration from vboxtool does *not* taking place on *running*
> sessions, so save or stop all sessions before issueing the autostart command.

* Create `/etc/vboxtool/machines.conf`:
```
    <session name>,<VRDP-port>
```

> **The VRDP-port enables RDP-clients like rdesktop to connect. It may be
    left blank.**

* Create `/etc/vboxtool/vboxtool.conf`:
```
    vbox_user='<user name>'
```

* Issue the following command:
```
    vboxtool autostart
```

> VBoxTool will configure sessions (VRDP-port). By now, session(s) should
> be up and running and configured.

* Check if sessions or running, with the assumed vrdp-port:
```
    vboxtool show 
```

  Show only the running sessions:
```
    vboxtool showrun
```

* Check if sessions configured in /etc/vboxtool/machines.conf are be
  automatically started at reboot. Reboot your system, check with:
```
 vboxtool showrun
```

## Upgrading from 0.2

Sorry for breaking things here, but it's all in the name of naming
consistency...

* Config folder is moved from /etc/vbox to /etc/vboxtool. Rename this folder.
* Main script 'vbox' is renamed to 'vboxtool'

## Usage

After installation, type 'vboxtool help' for more info.

## Known Issues

- Backup is not working as expected when using snapshots. When a snapshot is 
  present, the main vdi file is not copied, even if it's different from 
  previous backups. Problem is that once a snapshot is made, the main vdi 
  (according to info from 'VBoxManage showvminfo') is pointing to the snapshot 
  vdi instead of the expected, chained main vdi in the vdi folder.
  (Tracker #2132265)

## More Help

- Type: 'vboxtool help'
- See http://vboxtool.sourceforge.net for more details.    
