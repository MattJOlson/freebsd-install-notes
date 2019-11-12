# miskatonic - fileserver / light compute

## FreeBSD 12.1 installation notes

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

## Post-installation config

so what's my hit list?

- vim obviously
- maybe bash first, this is all pkg/ports stuff
- sudo or whatever utils (see below)

oh god I need to remap capslock, let's do that first.

### Remapping capslock

this looks plausible: http://ake.in.th/2015/04/17/remap-capslock-to-ctrl-on-freebsd/

> Assuming that you are using `us.iso.kbd`
uh, I wonder how I'd find out. (the answer is hinted to be `kbdcontrol(1)` by
the `kbdmap(5)` man page, but while technically correct that just dumps the
keymap rather than telling you what file's in use)

anyway, let's just try it.

`sudo: command not found`

fuck. well, `su` it is. Add `sudo` to the list of packages to install.

it did not work. not for my user account, nor for root.

looks like `kbdmap(1)` can be used to set keyboard maps, maybe I have a syntax
error and running it interactively will show me.

d'oh! there's a `us.ctrl.kbd` map that does what I want. unfortunately I'm not
sure that `kbdmap(1)` can be called in a startup script, it's one more trashbag
TUI thing from first appearances.

the mystery deepens: diffing the file is `us.pc-ctrl.kbd` and diffing it
against my custom `us.iso-swap.kbd` shows no diffs outside the first line
version tag. as far as I can tell from `rc.conf(5)` the syntax from the 2015
blog post still applies. wonder if this config file is getting superseded by
something else, or if a cache needs to be updated. (I don't see a second copy
of the "United States" keymap in `kbdmap(1)`'s menu.)

well, let's try setting `keyboard=us.pc-ctrl` and rebooting

yey! I mean, either that worked, or whatever `kbdmap(1)` did persisted
somewhere else. either way, my keyboard is now levelled up in utility.o

### some packages

okay, let's make this a bit nicer. start with an MOTD revision - wait, this
brings up memories of a blog post where someone tried "just editing `/etc/motd`
and it didn't work, so this'll be fun. actually let's wait until I have a
working `su` first.

feels ironic to have to `su root` to install `sudo` but here we are. here's
hoping I don't spend a lot of time hand-configuring machines, it's 2019 for
god's sake.

[x] `sudo`

oh right, sudoers. I'd half hoped that wheel group users would be added by
default. and lol, I keep getting errors because it's trying to "report the
incident" via sendmail. anyway, `visudo` as root and uncommenting the line
that starts with `%wheel` did the job. and now I'm installing vim, which also
installs ruby?! I love the unix philosophy.

[x] `vim`

okay, now I have a vim installation without my favourite vim files. I've
fretted occasionally that I don't have any dotfiles in version control,
probably I should set that up at some point. so let's install git and create a
dotfiles repo.

[x] `git`

(well, maybe we'll do the dotfiles repo thing later.)

[x] `bash`

