Instructions
============

### Usage: vboxtool OPTION [session]

Show info about VirtualBox sessions or control those sessions.

### Options:
  Command           |   Description
--------------------|---------------
  show              | Show status of all sessions.
  showrun           | Only show status of running sessions.
  showconfig        | Show configuration.
  start [session]   | Start all saved sessions or only the given session. When no session name is given, all saved sessions will be started; powered off and aborted sessions are left alone.
  autostart         | Starts all sessions in a predefined configuration file.
  save [session]    | Save all running sessions or only the given session.
  stop [session]    | Stop (by saving) all running sessions or only the given session.
  backup [session]  | Backup all running sessions or only the given session.
  --version or version | Version info.
  --help or help       | This help.

### Configuration.

Vboxtool depends on two config files, located in /etc/vboxtool.

Configuration file `/etc/vboxtool/machines.conf`.
* Each line in this file is a separate machine 
* Structure of each line: `<session name>,<vrde-port>,<host port>-<guest
  port>|...`
* The delimiter `,` between name and VRDE-port only required when configuring
  port forwarding
* Do not use spaces before and after the first `,` delimiter  
* Lines can be commented out by `#`
  
Example for `/etc/vboxtool/machines.conf`:
```
Ubuntu Desktop #1
Ubuntu Desktop #2,3391
Ubuntu JeOS #1,3392,2022-22|80-80
Ubuntu JeOS #2,,2022-22|80-80
```

Example for `/etc/vboxtool/vboxtool.conf`:
```
vbox_user='user'
backup_folder=/home/user/vboxbackup
```

### Autostart.
 Sessions can be started in a controlled way from the command line, only the
echo sessions in `/etc/vboxtool/machines.conf` will be started. As a bonus, the
VRDE port and port forwarding can be set at startup time. These options are
controlled by `/etc/vboxtool/machines.conf`. The given ports are set statically
to the session, prior to starting. When VRDE port has to be changed, state is
discarded when session is in savestate.
  
### Start at boot, save on halt.
 VBoxTool is capable for autostart sessions at boot time and autosave sessions
when host is stopped. This depends on `/etc/vboxtool/vboxtool.conf`. In here,
the variable `vbox_user` must be filled:
```
vbox_user='<user name>' 
```
> Note the quotes. Fill for <user name> the name of the user under which
> sessions are installed/running.  
  
When vboxtool.conf is not present, no session will start at boot, nor will
auto save on host down take place. When vboxtool.conf is present, all sessions
in machines.conf will be started because actually, a 'vboxtool autostart'
command is issued. Saving sessions when host goes down does not depend on
machines.conf: all running sessions will be saved by a 'vboxtool save' command.

### Stopping sessions. 
Saving sessions is preferred above stopping: this is faster when restoring and
safer because session can appear to be cold booted.

### Backup.
The backup command copies all session files to a safe location. This includes
the configuration file(s), main VDI file and all snapshots. Running sessions
are saved and started after backup has completed. The default backup folder is
relative to the vbox folder: `<vbox_folder>/.backup`. Underneath, subfolders
VDI and Machines are created.
Backups can be automated by putting something like in `/etc/crontab`:
```bash
05 2    * * *   <user name>    vboxtool backup &
```
A different backup folder can be used, by defining this in
`/etc/vboxtool/vboxtool.conf`:
```
backup_folder=/home/user/vboxbackup
```

### Logging.
All commands will be logged to $log_file
    
See http://vboxtool.sourceforge.net for more details.  

