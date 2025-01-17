# toolbox - bring your tools with you

rhcos-toolbox is a small script, designed for RHEL CoreOS, that launches a podman container to let you bring in your favorite debugging or admin tools.

## Usage

```
$ /usr/bin/toolbox
Spawning container core-fedora-latest on /var/lib/toolbox/core-fedora-latest.
Press ^] three times within 1s to kill container.
[root@localhost ~]# dnf -y install tcpdump
...
[root@localhost ~]# tcpdump -i ens3
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens3, link-type EN10MB (Ethernet), capture size 65535 bytes
```

## Advanced Usage

### Use a custom image

toolbox uses a Fedora-based userspace environment by default, but this can be changed to any Docker image. Simply override environment variables in `$HOME/.toolboxrc`:

```
core@core-01 ~ $ cat ~/.toolboxrc
REGISTRY=registry.redhat.io
IMAGE=rhel7/rhel-tools:latest
core@core-01 ~ $ toolbox
Spawning a container 'toolbox-test' with image 'registry.redhat.io/rhel7/rhel-tools:latest'
```

### Automatically enter toolbox on login

Set an `/etc/passwd` entry for one of the users to `/usr/bin/toolbox`:

```sh
useradd bob -m -p '*' -s /usr/bin/toolbox -U -G sudo,docker,rkt
```

Now when SSHing into the system as that user, toolbox will automatically be started:

```
$ ssh bob@hostname.example.com
Container Linux by CoreOS alpha (1284.0.0)
...
Spawning container bob-fedora-latest on /var/lib/toolbox/bob-fedora-latest.
Press ^] three times within 1s to kill container.
[root@localhost ~]# dnf -y install emacs-nox
...
[root@localhost ~]# emacs /media/root/etc/systemd/system/docker.service
```
