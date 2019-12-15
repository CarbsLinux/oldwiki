Installing and Using Keymaps
============================

Unlike most other distributions out there
Carbs Linux does not come preinstalled with console 
keys. You probably won't be using more than 
two either. There are two ways of using keymaps

Installing the "bkeymaps" package
---------------------------------

You can get the `bkeymaps` package from the
[KISS community repository](https://github.com/kisslinux/community).

Build and install `bkeymaps`.

<pre><code>

-> kiss b bkeymaps
-> kiss i bkeymaps

</code></pre>

Now you can find the keymaps in `/usr/share/bkeymaps`.


Downloading the keymaps you require
-----------------------------------

*This will assume that you are using the us keyboard layout*

You firstly will need to go to <https://dev.alpinelinux.org/bkeymaps/>
and download the keymap you need

Loading keymap, doing it on login/boot
--------------------------------------

- Run `loadkmap < file` to load the keymap.

**Load keymap at login**

If you want to load a keymap at login, you can
add the command above to your `.profile` file.

**Load keymap at boot**

You can also load keymap during boot. You should edit
`/lib/init/rc.boot` file and add something like

<pre><code>

# assuming you are using trq as your keymap

log "Setting keymap to trq..."; {
    loadkmap < /path/to/trq.bmap
}

</code></pre>

to your main function.
