## Using df

Displays disk usage information based on file system (ie: entire drives, attached media, etc)

```
$ df -h

Filesystem      Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk2s5   186Gi   10Gi  108Gi     9%  487441 1952637559    0%   /
devfs          190Ki  190Ki    0Bi   100%     658          0  100%   /dev
/dev/disk2s1   186Gi   66Gi  108Gi    38% 1063165 1952061835    0%   /System/Volumes/Data
/dev/disk2s4   186Gi  2.0Gi  108Gi     2%       2 1953124998    0%   /private/var/vm
/dev/disk1s1    47Gi  2.2Gi   45Gi     5%  145760  494830560    0%   /Volumes/Product
map auto_home    0Bi    0Bi    0Bi   100%       0          0  100%   /System/Volumes/Data/home

```

## Using du

Displays disk usage information for each file and directory (ie: home directories, folders, etc)

### To display disk uses for current directory
- -s : summary
- -h : human readable form
- ~ : current directory


``` 
$ du -sh ~

31G	/Users/rajantwnabashuzph

```

### To show all the files in current directory

``` 
$ du -sh *

4.0K	README.md
1.4M	android
4.0K	app.json
1.1M	assets
4.0K	babel.config.js
 72K	config
4.0K	index.js
157M	ios
4.0K	jest.config.js
4.0K	metro.config.js
815M	node_modules
852K	package-lock.json
8.0K	package.json
 36K	public
4.0K	react-native.config.js
4.0K	rn-cli.config.js
 20K	scripts
1.4M	src
4.0K	tsconfig.json
```
