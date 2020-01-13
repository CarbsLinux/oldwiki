Service Management
==================

Carbs Linux uses busybox-runit as the default system supervisor.


Enabling Services
-----------------

Services start immediately when you enable them, and run by default on boot.

<pre><code>
$ ln -s /etc/sv/acpid /var/service
</code></pre>


Disabling a service
-------------------

<pre><code>
$ unlink /var/service/acpid
</code></pre>


Starting a service
------------------

<pre><code>
$ sv start acpid
</code></pre>


Stopping a service
------------------

<pre><code>
$ sv stop acpid
</code></pre>


More
----

Runit is extremely flexible and simple. Refer to `sv`, `runsv`, `svc`, `runsvdir`
help outputs for more information.
