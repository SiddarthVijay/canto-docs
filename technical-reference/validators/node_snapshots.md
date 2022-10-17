# Snapshot Usage

## Creating and Hosting Snapshot
Follow instructions at [Canto Snapshots](https://github.com/SiddarthVijay/cosmos-snapshots/tree/patch/canto)

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
