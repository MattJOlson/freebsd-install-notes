# FreeBSD 12.1 installation notes

## fileserver / light compute - miskatonic

Screen by screen:

### Keymap selection

Standard US keyboard map (default)

### Hostname

miskatonic, because I'm a dork like that. I trust I'll be able to add a domain
name at some point later without misery; if there is misery involved I guess I
just repave.

oh no, the doc I didn't read until after I went through asks for a FQDN. welp.

### Distribution select

neat, this doesn't match the online doc. well, I hit enter rather than space so
I'm left with the default, that's probably for the best.

### Disk partitioning

let's do Auto (UFS) on the SSD target, I can ZFS the spinny disk later

- select ada0 (233GB ATA SSD)
- use the entire disk, bye Windows
- use GPT, that's what the doc recommends for amd64 targets
- I guess I could ZFS the spinny disk now but let's try to get up and running
  first

we've got the default, a 512k boot partition on ada0p1, 229g of UFS for / on ada0p2, and 3.9g of swap on ada0p3. ada1, the spinner, is full of NTFS.