Tags: #linux 
Created: 2022-11-10 21:11
References: 

# xinit
`xinit` is a program thats allows you to start a [[Xorg]] server manually (you can also use `startx`, which has *"a somewhat nicer user interface"*. This is generally used for starting [[Window Manager]]s or [[Desktop Environment]]s.

To configure xinit, you can copy the example config file to your home directory with this command:

```sh
$ cp /etc/X11/xinit/xinitrc ~/.xinitrc
```

## Troubleshooting
In case you run into troubles using `startx` or `xinit` (it says permission denied for `tty<x>`), try to use one of these:

```sh
chmod u+s /bin/Xorg
```

**Not really related:** In case you are trying to install i3 and use it together with ly, you need to pick the `i3 (with debug log)` option in the ly login prompt, otherwise it will fail to start.