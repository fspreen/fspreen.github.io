---
layout: post
title:  SSH Client Configuration
date:   Tue, 02 Aug 2016 19:33:11 -0400
---
One way to work with repositories on GitHub is through an [SSH remote].  This is
very handy as it allows development and source management to happen solely on
the command line, if desired.  It also provides improved security by using an
SSH public/private key pair.  (Of course, this assumes that your private key
file is not compromised!)

Since I often use Git in Linux or Cygwin environments, I prefer to use OpenSSH
as my SSH client.  Often I want to change the SSH settings away from the default
options, but Git has only a limited capacity to do so.  Enter the SSH client
configuration file!

# Where is it? #
Like many configuration arrangements on Linux, SSH client settings are actually
listed in _two_ files.  The first is usually at `/etc/ssh/ssh_config` which is
used for system-wide settings (i.e. settings that apply to all users).  Anything
that deals with the basic mechanics of authentication should be placed here;
generally the defaults provided by your Linux distribution are enough.

The second file is a per-user file.  This is located at `~/.ssh/config` but new
users may have to create it for themselves.  A user-specific file is the best
place to provide settings for specific remote hosts.

# Overall Settings #
My SSH configuration file is fairly short, but I like to include some
housekeeping details as well.  Any lines before a `Host` directive apply to any
outgoing SSH connection attempt, so this is my first line:

```
Protocol 2
```

This tells OpenSSH that I want to make connections only using SSH protocol
version 2.  This is the overwhelming default in recent years, but for backwards
compatibility OpenSSH supports protocol version 1.  However, this version is
less secure.  By explicitly specifying only protocol 2, any outgoing SSH
connection will not fall back to protocol 1, but will fail if it is not
supported.  The upside of this is that my connections will all be properly
secured.

# Per-Host Settings #
Settings can also be specified per remote host or per matching criteria.  A list
of host settings begins with the `Host` keyword and a name.  I like to indent
each host's settings, but it isn't strictly necessary.

Common settings are `User` for the remote host's username and `HostName` for the
address of the remote host.  If you have a public/private key arrangement,
`IdentityFile` specifies the private key file.  There are also settings to
change the port number, traffic forwarding, X11 tunneling, and even the
encryption settings.

If you have multiple private keys, or if you use a non-default name for your
private key file, `IdentityFile` is a must-use.

Once you define a host block, you can use the name of the host at the command
line as shorthand to specify all the listed settings.

# Putting it all Together #
Here's my short-and-simple configuration file with settings for GitHub:

```
Protocol 2

Host gh
    User git
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_github
```

Now I can simply use `gh` as shorthand for the basic GitHub settings.  Normally,
GitHub recommends [testing your SSH connection][ssh-test] with this command:

```
ssh -T git@github.com
```

With my new configuration file, I can shorten that:

```
ssh -T gh
```

And the best part is that I can use it for cloning Git repositories or setting
SSH remote URLs:

```
$ git remote -v
origin  gh:fspreen/dotfiles.git (fetch)
origin  gh:fspreen/dotfiles.git (push)
```

# More Information #
Of course, this really only scratches the surface of what can be done with
configuration settings in OpenSSH.  A much more complete list of settings can be
seen by invoking `man ssh_config` at the command line.  Check it out and explore
for yourself!

[SSH remote]: https://help.github.com/categories/ssh/
[ssh-test]: https://help.github.com/articles/testing-your-ssh-connection/
