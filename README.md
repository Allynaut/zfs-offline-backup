# zfs-offline-backup

This script wakes up a server via wake on lan, sends all zfs snapshots (except hourly and frequent snapshots) generated by [zfs-auto-snapshot](https://github.com/zfsonlinux/zfs-auto-snapshot) via zfs send/receive and deletes old snapshots on the remote system

## Getting Started

Syntax:
```
./run_backup.py aa:bb:cc:dd:ee:ff 10.30.1.123 2G 9090 backuppool -i -p -v3
```
Explanation:
- aa:bb:cc:dd:ee:ff - Backup server mac-address
- 10.30.1.123 - Backup server ip-address
- 2G - Memory to allocate by mbuffer
- 9090 - Port to use by mbuffer
- backuppool - name of Backuppool
- -i/--initbackup - switch for the first time use (if you don't have snapshots on the remote system to refer to)
- -p/--poweroff - Power down remote system at the end
- -v3/--verbosty 3 - Level of verbosity

If you dont have any snapshots on the remote system run the script with the --initbackup switch for the first time
```
./run_backup.py --initbackup aa:bb:cc:dd:ee:ff 10.30.1.123 2G 9090 backuppool
```
else the script trys to find a common snapshot for an icremental send.

### Prerequisites

This script requires the following python packages:
[pywakeonlan](https://github.com/remcohaszing/pywakeonlan)
[ordered-set](https://github.com/LuminosoInsight/ordered-set)

On both systems:
- mbuffer
- python 2 or higher
- zfs and a pool (duh)

On the server to backup:
- zfs-auto-snapshot

I also assume you have a ssh key installed on the backup system.
Currently the username "backup" is hardcoded, you may want to change that.

### Installing

(in no particular order)
- Install packets and [requirements](requirements.txt)
- Put the script somewhere you like and make it executable.
- On the Backup system create the Backup user and create a ssh key
- You may add a sudoers file for the backup user or add required permissions via zfs allow/unallow

If you use puppet for configuration, I added my puppet class to the repo

## TODO
- [ ] Make the user a parameter (not hardcoded)
- [ ] Make other snapshot name pattern possible
- [ ] Make transport over ssh an option
- [ ] Add ZFS and S.M.A.R.T. Health checks

**This script was tested with CentOS 7.4 and ZFS version 5**

## License

This project is licensed under the GPL v3.0 License - see the [LICENSE](LICENSE) file for details