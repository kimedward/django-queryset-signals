#######################
Django Queryset Signals
#######################

.. image:: https://travis-ci.org/magenta-aps/django-queryset-signals.svg?branch=master
    :target: https://travis-ci.org/magenta-aps/django-queryset-signals
===========
A library that will send signals on queryset data manipulation methods. 
| get_or_create       | X    |        |        |             |        | X             |                  |        |
+---------------------+------+--------+--------+-------------+--------+---------------+------------------+--------+
| get_or_create_exist |      |        |        |             |        | X             |                  |        |
+---------------------+------+--------+--------+-------------+--------+---------------+------------------+--------+
| update_or_create    | X    |        |        |             |        |               | X                |        |
+---------------------+------+--------+--------+-------------+--------+---------------+------------------+--------+
| update              |      |        |        |             |        |               |                  | X      |
+---------------------+------+--------+--------+-------------+--------+---------------+------------------+--------+

The _exists entries are when data matching already exists, and are only included when it affects behavior.

How do I install it?
====================
.. sourcecode:: shell

  pip install django-queryset-signals

And then add 'django_queryset_signals' to your installed apps.

How do I use it?
================
From the namespace django_query_signals you can import the below signals which
you can connect to via the usual way.

 - pre_bulk_create
 - post_bulk_create,
 - pre_delete
 - post_delete
 - pre_get_or_create
 - post_get_or_create
 - pre_update_or_create
 - post_update_or_create
 - pre_update
 - post_update

For example:

.. sourcecode:: shell

  >>> @receiver(post_bulk_create)
  >>> def callback(signal, sender, args):
  >>>       pass

The argument 'signal' is the signal that is connected, 'sender' is the
underlying model class and 'args' is a dictionary which the method in queryset
is called with, this is supplemented with 'self' which contains the queryset
instance and if the connecting signal is a 'post' type the key 'return' is also
added which contains the value the method has returned. 

If you connect to the 'pre' type signal, changing the 'args' and 'self' will
also change the actual execution of the method.

Caveat
======
This library relies on monkey patching django.db.models.query.QuerySet, thus if
your instance also monkey patches the same thing or you use a custom QuerySet in
your Manager, then there is a good chance that this library will not work at all
for you, however most likely you can work around this issue by examining the
signals.py file in this library.  

What license is this?
=====================
BSD-2-Clause
