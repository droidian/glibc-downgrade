glibc-downgrade
===============

Rather simple tool to clear symbol versions for newer glibc releases from a shared library or executable.  
The heavy lifting is done by [a modified version of patchelf](https://github.com/NixOS/patchelf/pull/394), this is just a small tool that has
been split in order to use that inside Droidian without copying the original script (coming from our fork of
libhybris) around.

Note: Droidian repositories already ship a working patchelf.

Usage
-----

	glibc-downgrade 2.33 /path/to/library.so

Keeps all the symbols up to GLIBC_2.33, strips the version from the others.

This means that

                 U pthread_getattr_np@GLIBC_2.32
                 U pthread_getschedparam@GLIBC_2.2.5
                 U pthread_getspecific@GLIBC_2.34
                 U pthread_join@GLIBC_2.34
                 U pthread_key_create@GLIBC_2.34
                 U pthread_key_delete@GLIBC_2.34
                 U pthread_kill@GLIBC_2.34
                 U pthread_mutex_destroy@GLIBC_2.2.5
                 U pthread_mutex_init@GLIBC_2.2.5
                 U pthread_mutex_lock@GLIBC_2.2.5
                 U pthread_mutex_timedlock@GLIBC_2.34
                 U pthread_mutex_trylock@GLIBC_2.34
                 U pthread_mutex_unlock@GLIBC_2.2.5


Becomes

                 U pthread_getattr_np@GLIBC_2.32
                 U pthread_getschedparam@GLIBC_2.2.5
                 U pthread_getspecific
                 U pthread_join
                 U pthread_key_create
                 U pthread_key_delete
                 U pthread_kill
                 U pthread_mutex_destroy@GLIBC_2.2.5
                 U pthread_mutex_init@GLIBC_2.2.5
                 U pthread_mutex_lock@GLIBC_2.2.5
                 U pthread_mutex_timedlock
                 U pthread_mutex_trylock
                 U pthread_mutex_unlock@GLIBC_2.2.5


The linker will then fallback to the symbol version in the running system.

Notice
------

Don't use this unless you know what you're doing.
