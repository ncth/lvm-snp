# lvm-snp
## Automatic LVM snapshot management tool

This tool is based on <a href="https://github.com/nachoparker/btrfs-snp">nachoparker/btrfs-snp</a>.
 
```
Usage: lvm-snp <src> <vg> (<tag>) (<size>) (<limit>) (<seconds>)

  src     │ create snapshot of the logical volume <src>
  vg      │ virtual group the src is in
  tag     │ name the snapshot <tag>_<timestamp>
  size    | storage size allocated to the snapshot
  limit   │ keep <limit> snapshots with this tag. 0 to disable
  seconds │ don't create snapshots before <seconds> have passed from last with this tag. 0 to disable
```
## Examples 

### Manual

Snapshot of data in vg00

```
# lvm-snp data vg00
```

Tagged snapshot of files in vg00

```
# lvm-snp data vg00 preupgrade
```

Tagged snapshot of files in vg00 with size 15gb, but keep maximum 10

```
# lvm-snp data vg00 preupgrade 15 10
```

### Cron 

Hourly snapshot for one day, daily for one week, weekly for one month, and monthly for one year.

```
cat > /etc/cron.hourly/$BIN <<EOF
  #!/bin/bash
  /usr/local/sbin/lvm-snp data vg00 hourly 1  24 3600
  /usr/local/sbin/lvm-snp data vg00 daily  3  7 86400
  /usr/local/sbin/lvm-snp data vg00 weekly 7  4 604800
  /usr/local/sbin/lvm-snp data bg00 monthly 15 12 2592000
  EOF
  chmod +x /etc/cron.hourly/$BIN"
```

## Installation

```
sudo wget https://raw.githubusercontent.com/ncth/lvm-snp/master/lvm-snp -O /usr/local/sbin/lvm-snp
sudo chmod +x /usr/local/sbin/lvm-snp
```
