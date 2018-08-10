---
title: "Work begins"
date: 2018-08-10T09:25:10+12:00
type: "blog_post"
draft: false
---

This is the beginning of a project to design and implement a new backup tool called
packnback.

It makes sense to explain why we should bother in the first place,
and what interesting features a new tool can bring to the table.

# Ideal features

Let's list features that are important in a backup tool and
them to existing tools so we get a good idea of the current backup tool
choices you might have as an administrator trying to bullet proof a your server environment.

### Data deduplication

Don't store or upload the same data twice when not necessary. 
Share data across backups, this could be across identicle files,
or even within files with repetitive data.

With this feature we can do daily/hourly/whatever snapshots
while having low bandwidth and storage requirements. The 
first backup may take 20 GB, but the next may only be 200 MB.

### Write-only mode

Once a backup is transferred from machine A to machine B
we want there to be no way machine A can delete it, instead
the administrator of machine B must authorize deletion.

This is extremely important in the event that machine A becomes
infected by ransomware or another such attacker. A compromised
machine should only be able to compromise future backups,
but never past backups.

### Client side encryption

Much like an email server cannot read gpg encrypted emails as
it relays them to the desired addresses,
there is no reason a backup server needs to be able to read
any backup data being stored on it. If data is encrypted client
side before it reaches a server and decrypted client side after
retrieving data, there is no reason the server needs access to the 
unencrypted data at all.

Client side encryption limits trust in the server to storing data, and enforcing
write only mode actions, but never to reading data. If
the server fails it's storage duty, cryptography will notify
us of corrupt data or data that has been tampered with.

### Secure decryption keys

Once a backup has been made, it should not be able
to be decrypted using any encryption key material
stored on the uploading machine. This is for the same
reason we would like write only mode, to keep data away
from attackers in the event of a security breach.

This feature implies public key cryptography where we discard a 
portion of a generated encryption key per backup. Only the designated
reciever secret key will be able to decrypt the data, not the sender.

### Pruning

Being able to rotate and prune backups is a useful feature,
deduplication of data complicates things slightly as
it requires either [reference counting](https://en.wikipedia.org/wiki/Reference_counting)
or [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)),
which in turn requires some concurrency controls. This is a why some
tools mentioned do not support pruning of data.

With easy data pruning, being able to design backup systems with upper bounds on costs
without manual intervention is a win for everyone.

### Open source

We need to be able to see the source code of tools
to trust thier implementation is reliable, and to trust
the privacy guarantees it offers.

# Other tools

So now we have a list of features we would like, let's check what exists in the world
today.

Project | Dedup | Write-only | Encryption | Secure keys | Pruning | Open source 
:--- | :---: | :---: | :---: | :---: | :---: | :---: 
[borg](https://borgbackup.readthedocs.io/en/stable/) | y | y | y | n | y | y 
[bup](https://github.com/bup/bup) | y | n | n | n | n | y 
[zbackup](https://github.com/bup/bup) | y | n | y | n | y | y 
[restic](https://restic.net/)      | y | y | n | n | y | y 
[tarsnap](https://www.tarsnap.com/) | y | y | y | y | y | partial-proprietry 
[rclone](https://rclone.org/) | n | n | y | n | n | n

(There are definitely other tools, let me know and the feature sets and I will add them to the list.)

Currently, *to my knowledge* no current software matches the requirements. This
means there may be a point in the design space that is unaddressed that we can target. So until
we have a good reason not to proceed, I think it is a worthy project, and hence this post.

# Summary

This blog series will be about designing and implementing a backup tool
that should tick every box in our feature list (while trying to be as simple as possible).

The current plan is to release small, usable, command line tools incrementally as they are implemented,
along with blog posts and a big picture of how they will compose into a larger robust backup tool.

Please let me know if this interests you, what I have missed, or some backup horror stories
to keep me up at night.

That's all for now, and stay tuned for the next post with some code and examples...
