

Get Runnning Sevice State
^^^^^^^^^^^^^^^^^^^^^^^^^^


Two methods of getting running service state is given:

* eventlet.backdoor_server
* GMR (Guru Meditation Reports)

eventlet.backdoor_server
"""""""""""""""""""""""""

When building, each service could build a backdoor server according to
the backdoor_port option in config. Backdoor_port allows user connect to
node to debug and get running service state by telnet.

Using the following code to create backdoor_server:

.. Link: http://docs.openstack.org/developer/designate/gmr.html
::

  from eventlet import backdoor
  import eventlet
  
  eventlet.spawn(backdoor.backdoor_server, eventlet.listen(('localhost', PORT)), locals=backdoor_locals)
  
`backdoor_locals` is a dirc <key, value>, defining functions which can be called by admin.


  
Detailed instances is in the link:
http://lingxiankong.github.io/blog/2014/07/30/eventlet-backdoor/

Weakness
------------------

In addition, `eventlet.backdoor_server` has several weaknesses:

* Each service, accompanying with backdoor server, need to set a TCP port which is recorded by admin.
* Once service restart, all key info will be lost.
* It's dangerous that Each service leaves a port.

GMR
""""""

User can send a `USER2` signal, then generate a report.

The report is a general-purpose debug report for developers and system admins which
contains the current state of a running Designate service process.

Generate a GMR
----------------

::

  A GMR can be generated by sending the USR2 signal to any Designate processes.

  For example, suppose designate-central has pid 15097, kill -USR2 15097 will trigger a GMR.

  If option logdir has been set in designate.conf, the GMR will be saved in the folder
  which logdir specified. Otherwise, the GMR will be printed to the stderr.
