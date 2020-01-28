Module Management
=================

With **busybox-init**, a module can be loaded at boot by adding such a line to your `inittab`

<pre><code>
::once:/bin/modprobe module-name
</code></pre>

With **sinit**, a module can be loaded from your `/etc/rc.local` file. Add this to your file
<pre><code>
/bin/modprobe module-name
</code></pre>
