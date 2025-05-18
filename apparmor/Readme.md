# AppArmor 

- Supported from v1.31 [docs](http://kubernetes.io/docs/tutorials/security/apparmor/)

- AppArmor is a kernel module available by default on many distributions and supports Mandatory Access Control. It enhances normal control file-based permissions, enabling finer-grained control and greater defence in-depth.

- The key difference between apparmor and seccomp is seccomp restrict syscalls to kernel and apparmor restrict access to specific objects like directories and files.'

- Profiles are defiend under /etc/apparmor.d and we can know the status and profiles loaded using command `aa-status` and for loading the profile using command `apparmor_parser -q /etc/apparmor.d/usr.sbin.nginx`.

```console
#include <tunables/global>

profile custom-nginx flags=(attach_disconnected) 
  #include <abstractions/base>

  # Enable setgid
  capability setgid

  # disable IPv4 TCP
  deny network inet tcp,

  # Deny file writes in /data/pages.
  deny /data/pages/** w
}
```

