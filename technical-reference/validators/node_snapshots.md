# Snapshot Usage

## Creating and Hosting Snapshot

### Install tooling requirements  
```bash
sudo apt update && \
sudo apt install curl git docker.io -y
```

### Setup dependencies
```bash
git clone https://github.com/SiddarthVijay/cosmos-snapshots.git && cd cosmos-snapshots
mkdir $HOME/snapshots/
```

### Start Nginx via docker  
```bash
cd $HOME; \
docker run --name snapshots \
--restart always \
-v $(pwd)/default.conf:/etc/nginx/conf.d/default.conf \
-v $(pwd)/snapshots/:/root/ \
-p 80:80 \
-d nginx
```

Edit config if required by editing variables in the file `canto_snapshot.sh`  
```
CHAIN_ID="canto_7700-1"
SNAP_PATH="$HOME/snapshots/canto"
LOG_PATH="$HOME/snapshots/canto_log.txt"
DATA_PATH="$HOME/.cantod/data/"
SERVICE_NAME="cantod.service"
```

### Create new snapshot  
`./canto_snapshot.sh`  

### Check snapshot  
```bash
MY_IP=$(curl -s 2ip.ru); \
curl -s http://${MY_IP}
```

### Automation
You can add script to the cron  
```cron
# start every day at 00:00
0 0 * * * /bin/bash -c '/root/canto_snapshot.sh'
```

## Consuming Snapshot

### Install Dependencies
```bash
sudo apt update
sudo apt install snapd -y
sudo snap install lz4
```

### Download Snapshot
```bash
wget -O <snapshot_file>.tar.lz4 <host_url>
```

### Use Snapshot
Backup priv_validator_key.json (cannot be recovered after following steps)

```bash
sudo systemctl stop cantod
cantod unsafe-reset-all
lz4 -c -d <snapshot_file>.tar.lz4  | tar -x -C $HOME/.cantod
```

### Restart Node
```bash
sudo systemctl start cantod
# Watch logs
journalctl -u cantod -f
```
