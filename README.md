Utility to create encrypted compressed archives with recovery data

# Usage examples

##  Compression

### Simplest use

This command will compress directory `dir` (including all its files and subdirectories) with `tgz` and name the archive as `YYYYMMDD-dir.tgz`:

```
archive dir
```

### Choosing the format

Same as before, mas produce a smaller archive using the format XZ (`YYYYMMDD-dir.xz`):

```
archive -xz dir
```

### Setting output directory and name

Don't use the date as prefix:

```
archive -n <files/dirs>
```

Add the current time:

```
archive -t <files/dirs>
```

Set the output directory:

```
archive -o /tmp/output <files/dirs>
```

### Adding recorevy data (parity)

Set the level of redundancy to 5%:

```
archive -n -r 5 important_dir/
```

This will generate the following:

```
Compressing files to ./important_dir.tgz...

Creating redundancy (parity files) @ 5% redundancy...
Opening: important_dir.tgz
Done

Output files:
4.0K    ./important_dir.tgz
4.0K    ./important_dir.tgz.par2
4.0K    ./important_dir.tgz.vol0+1.par2
4.0K    ./important_dir.tgz.vol1+1.par2
```

(all output files should be kept in order to repair the archive if it gets corrupted)


### Encrypting the archive

```
archive -e secret_dir/
```

This will ask for a password and will produce the following output:

```
Compressing files to ./20210831-secret_dir.tgz.gpg...                                              

Output files:
4.0K    ./20210831-secret_dir.tgz.gpg
```

## Decompression

```
archive -d important_dir.tgz
```

## Listing archive contents

```
archive -l important_dir.tgz
```

## Repairing corrupted archives

```
archive -R important_dir.tgz
```

(the parity files are needed)


# Dependencies

tar; xz; par2; gpg; du; a pass-phrase entry dialog (ex: pinentry-curses or pinentry-gnome3), which should set in `~/.gnupg/gpg-agent.conf` as `pinentry-program /usr/bin/pinentry-curses` for instance.
