---
title: "Work begins"
date: 2018-08-10T09:25:10+12:00
type: "blog_post"
draft: false
---

This is the beginning of a project to design and implement a new backup tool called
packnback designed to solve some of the limitations of existing tools.

It makes sense to explain why I we should bother in the first place,
and what interesting features a new tool can bring to the table.

# Ideal features

Let's list features that are important in a backup tool and
them to existing tools, this will hopefully give you a good
idea of the current backup tool choices you might have
as an administrator trying to bullet proof a production 
environment.

### Data deduplication

Don't store or upload the same data twice, and share data across 
backups. This is important as we can do daily snapshots
while having low bandwidth and storage requirements.

### Write-only mode

Once a backup is transferred from machine A to machine B
we want there to be no way machine A can delete it, instead
the administrator of machine B must authorize deletion.

This is extremely important in the event that machine A becomes
infected by ransomware or another such attacker. It should not
be able to compromise the integrity of its 

### Client side encryption

Much like an email server cannot read gpg encrypted emails as
it relays them to the desired addresses,
there is no reason a backup server needs to be able to read
any backup data being stored on it. If data is encrypted client
side before it reaches a server and decrypted client side after
retrieving data, there is no reason the server needs access to the 
unencrypted data.

Client side encryption limits trust in the server to storing data, and enforcing
write only mode actions, but never to reading it. If
the server fails it's storage duty, cryptography will notify
us of corrupt data or data that has been tampered with.

### Secure decryption keys

Once a backup has been made, it should not be able
to be decrypted using any encryption key material
stored on the uploading machine. This is for the same
reason we would like write only mode, to keep data away
from attackers in the event of a security break.

This implies feature implies public key cryptography where we discard a 
portion of a generated encryption key per backup. The private
key of the backup owner decrypts the backup, not any keys saved
on the machine uploading backups.

### Pruning

Be able to rotate and prune backups is an important feature,
deduplication of data complicates things slightly as
it requires either [reference counting](https://en.wikipedia.org/wiki/Reference_counting)
or [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)),
which in turn requires some concurrency controls. This is a why some
tools mentioned do not support pruning of data.

### Open source

We need to be able to see the source code of tools
to trust thier implementation is reliable, and to trust
the privacy guarantees it offers.

## Prior works

Project | Dedup | Write-only | Encryption | Secure keys | Pruning | Open source 
:--- | :---: | :---: | :---: | :---: | :---: | :---: 
[borg backup](https://borgbackup.readthedocs.io/en/stable/) | y | y | y | n | y | y 
[bup](https://github.com/bup/bup) | y | n | n | n | n | y 
[zbackup](https://github.com/bup/bup) | y | n | y | n | y | y 
[restic](https://restic.net/)      | y | y | n | n | y | y 
[tarsnap](https://www.tarsnap.com/) | y | y | y | y | y | partial-proprietry 
[rclone](https://rclone.org/) | n | n | y | n | n | n

(There are definitely other tools, let me know and the feature sets and I will add them to the list.)

Currently, *to my knowledge* no current software matches my requirements... (or yours perhaps?). This
means there may be a point in the design space that solves some problems. Now we have a starting point
for a set of requirements, the needed design will be much more clear.

I have private code snippets that implement and prototype a great deal of my solution,
but this blog will serve as both documentation, a way to get feedback in a trickled and controlled way.

# Summary

This blog series will be about designing and implementing a backup tool
that should tick every box, while trying to be as simple as possible.

Stay tuned for the next post...
