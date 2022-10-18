# Snapshot Usage

## Creating Snapshot

### Create folder for snapshots
```bash
mkdir -p $HOME/snapshots/canto
```

### Clone github repo 
```bash
git clone https://github.com/SiddarthVijay/cosmos-snapshots.git
cd cosmos-snapshots
git checkout patch/v1-canto
```

### Create new snapshot  
```bash
cd scripts
./canto_snapshot.sh
```

### Automation  
You can add script to the cron  
```cron
# start every day at 00:00
0 0 * * * /bin/bash -c '/root/canto_snapshot.sh'
```
---

## Consuming Snapshot
Snapshots also available on [Polkachu](https://polkachu.com/tendermint_snapshots/canto)

### Use Snapshot
Backup $HOME/.canto/priv_validator_state.json (cannot be recovered after following steps)

```bash
sudo systemctl stop cantod
cd $HOME/.cantod
cp ./data/priv_validator_state.json ../
rm -rf ./data
mkdir data
cd ./data
wget -O <snapshot_file>.tar <host_url>
tar -xvf <snapshot_file>.tar
rm ./priv_validator_state.json
mv ../priv_validator_state.json ./
```

### Restart Node
```bash
sudo systemctl start cantod
# Watch logs
journalctl -u cantod -f
```
---
