
### Ignore directory to be synced with Synology Drive Client
1. open `~/Library/Application Support/SynologyDrive/data/session/{1,2,3...}/conf/blacklist.filter`
2. add
```
[Directory]
black_name = "node_modules"
```

### Rsync
- [guide](https://www.truetoad.com/Blog/copy-data-between-two-synology-nas)
- enable Rsync (both devices) - Control Panel -> File Services -> Rsync -> Enable Sync Service
- On source NAS, Control Panel -> File Services -> Advanced -> Task List

### Download logs
Support Centre -> Support Services -> Log Generation
- this will download a `.dat` file. Unzip it in a new directory to get access to the contents