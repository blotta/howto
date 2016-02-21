Pacman Guide
============

### Installing specific packages

```bash
# pacman -S pkg1 pkg2
```

From different repos
```bash
# pacman -S extra/pkg
```

### Removing packages

Leaving dependencies
```bash
# pacman -R pkg
```

Remove package and its dependencies which are not required by another package
```bash
# pacman -Rs pakg
```
Remove package, required by another, without removing dependent package
```bash
# pacman -Rdd
```

### Upgrading Packages

Update repos
```bash
# pacman -Sy #-Syy to force
```

Upgrade packages
```bash
# pacman -Su #-Suu enables downgrades
```
Update repos and upgrade packages
```bash
# pacman -Syu
```

### Querying package database

pacman queries local database with `-Q` and sync database with `-S`

Search packages in the sync database
```bash
$ pacman -Ss pkg
```

Search already installed packages in the local database
```bash
$ pacman -Qs pkg
```
To see what packages belong to a group
```bash
$ pacman -Sg pkgGroup
```

Display info about a package
```bash
$ pacman -Si pkg #-Qi for local
```

Two `-i` flags to display list of backup files and modification sates
```bash
$ pacman -Qii pkg #-Sii shows MD5 and SHA-256 checksums
```

Retrieve list of files installed by a package
```bash
$ pacman -Ql pkg
```

Total number of files installed by package
```bash
$ pacman -Qk pkg #-Qkk for thorough check
```

Know what package a files belongs to
```bash
$ pacman -Qo /path/to/file_name
```

List 'orphan' packages (installed but no longer required as dependencies
```bash
$ pacman -Qdt
```

List all explicitly installed and not required as dep
```bash
$ pacman -Qet
```

Display dependency tree of a package
```bash
$ pactree pkg # -r flag to reverse the search
```


###Cleaning package cache

Pacman stores its downloaded packages in /var/cache/pacman/pkg and does not remove old or uninstalled versions automatically.


>Warning: Use with caution

To remove all cached packages that are not currently installed
```bash
# pacman -Sc
```
This will leave only the latest version

For a more secure approach, consider:
```bash
# paccache -r
```
This will remove all cached versions of each package, except for the most recent 3
Although it will not check if the package is installed, leaving uninstalled packages in the cache

To remove ALL versions of uninstalled packages, follow the previous command with:
```bash
# paccache -ruk0
```


### Additional Commands

Download package without installing it
```bash
# pacman -Sw pkg
```

Install local package (e.g downloaded from AUR)
```bash
# pacman -U /path/to/package/pkg_name-version.pkg.tar.xz
```

Keep copy of local package in pacman's cache
```bash
# pacman -U file://path/to/package/pkg_name-version.pkg.tar.xz
```

Install remote package (not in a pacman listed repo)
```bash
# pacman -U http://www.example.org/repo/pkg_name.pkg.tar.xz
```


### Configuration

Settings are on `/etc/pacman.conf` file. 

Uncomment `Color` line to have color output

Uncomment `VerbosePkgList` to see old and new versions when upgrading

Have a specific package or package group from upgrading

* `IgnorePkg=nvidia-utils`

>Note: You may use additional lines or a space-separated list
>additionally, `--ignore` can be used on the command line, for
>one or multiple comma-separated packages

* `IgnoreGroup=xfce4`


