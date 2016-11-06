=====================================
alw: Simple dynamic loader for OpenAL
=====================================

Introduction
------------

alw_ is the easiest way to get your hands on the functionality offered by the
OpenAL API.

Its main part is a simple alw_gen.py_ Python script that generates alw.h and alw.c
from the OpenAL headers (al.h and alc.h).
Those files can then be added and linked (statically or dynamically) into your
project.

Requirements
------------

alw_gen.py_ requires Python version 2.6 or newer, with SSL support.
It is also compatible with Python 3.x.

Example
-------

Here is a simple example of loading and using the OpenAL API via alw_.
Note that AL/alw.h must be included before any other OpenAL related headers::

    #include <stdio.h>
    #include <AL/alw.h>

    // ...

    int main(int argc, char* argv[])
    {
            ALCdevice* dev;
            ALCcontext* ctx;

            if (alwInit() < 0) {
                fprintf(stderr, "Failed to load OpenAL\n");
                return 1;
            }

            // error checking omitted
            dev = alcOpenDevice(0);
            ctx = alcCreateContext(dev, 0);
            alcMakeContextCurrent(ctx);

            // use OpenAL...

            // clean up
            alcMakeContextCurrent(0);
            alcDestroyContext(ctx);
            alcCloseDevice(dev);

            alwTerminate();

            return 0;
    }

API Reference
-------------

The alw_ API consists of just three functions:

``int alwInit(void)``

    Initializes the library. Should be called before any call to an OpenAL entry
    point is made. Returns ``0`` when alw_ was initialized successfully or a
    negative value if there was an error.

``void alwTerminate(void)``

    Terminates the library and unloads the OpenAL module if possible.

``ALWalproc alwGetProcAddr(const char *proc)``

    Returns the address of an OpenAL function. Generally, you won't
    need to use it since alw_ loads all functions defined in the core OpenAL API on
    initialization. It allows you to load OpenAL functions outside of the core API.

License
-------

alw_ is in the public domain. See the file UNLICENSE for more information.

Credits
-------

Slavomir Kaslev <slavomir.kaslev@gmail.com>
    gl3w, which alw is based on

Niklas F. Pluem <niklas.crossedwires@gmail.com>
    Implementation

Copyright
---------

OpenAL is a trademark of Creative Labs, Inc.

.. _alw: https://github.com/kwertz/alw
.. _alw_gen.py: https://github.com/kwertz/alw/blob/master/alw_gen.py
.. _gl3w: https://github.com/skaslev/gl3w
.. _gl3w_gen.py: https://github.com/skaslev/gl3w/blob/master/gl3w_gen.py
.. _OpenAL headers: https://github.com/kcat/openal-soft/blob/master/include/AL
.. _OpenAL: http://www.openal.org/
.. _Creative Technologies: http://www.creative.com/
.. _Loki Software: http://www.lokigames.com/
