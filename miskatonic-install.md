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

### Root password

oh god I need a root password

### Network configuration

em0 on the motherboard is recognized out of the box, that's how you can tell it
isn't linux I guess.

let's try DHCP for IPv4, I think that worked, and no IPv6 for now.

okay I've got 192.168.1.1 for primary DNS apparently, and let's throw in
8.8.8.8 for secondary.

### Select time zone

lol time zones again, no this thing isn't UTC on the CMOS. Yes I'm in PST, yes
it's November 11th, no wow my system clock is way off jeez. oops, I mistakenly
accepted the time rather than resetting, I suck at this TUI.

### System configuration

local_unbound looks like a DNS cache, don't think I need that by default.

sshd yes please

moused no thanks

ntpdate yes, especially given I just fucked up. (but managed to use the TUI
properly for the first time!)

ntpd sure

powerd and dumpdev, why not

### System hardening

this all looks like "make it harder for people with shell access" with some
sensible things (like "disable Sendmail", omg). gonna check every box but
syslogd (7) and hope for the best.

### Add users

mostly this should have app users, I think, but let's add a regular user so I
can sudo and disable root login. the documentation references a `wheel` group,
let's hope that actually exists.

oh god I forgot how much I hate default shells, let's do csh until I can get
bash installed and then play with zsh and fish and all the other trendy shit.

### Final configuration

oh good I can go back and fix the localtime shit. just set the time to the
second based on a mechanical watch, that's a smart move.

installing the FreeBSD Handbook, mostly as proof that I can has internet.

neat, clicking "exit" took _forever_ to get to "The installation is now
finished". good tiems