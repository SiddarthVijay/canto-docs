# Snapshot Usage

## Creating and Hosting Snapshot
Follow instructions at [Canto Snapshots](https://github.com/SiddarthVijay/cosmos-snapshots/tree/patch/canto)

## Consuming Snapshot

### Use Snapshot
Backup priv_validator_key.json (cannot be recovered after following steps)

```bash
sudo systemctl stop cantod
cantod unsafe-reset-all
cd $HOME/.cantod
wget -O <snapshot_file>.tar <host_url>
tar -xvf <snapshot_file>.tar 
```

### Restart Node
```bash
sudo systemctl start cantod
# Watch logs
journalctl -u cantod -f
```

Snapshots also available on [Polkachu](https://polkachu.com/tendermint_snapshots/canto)
