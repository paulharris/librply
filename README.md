# LibRPLY

[![Build Status](https://travis-ci.org/jirihnidek/librply.svg?branch=master)](https://travis-ci.org/jirihnidek/librply)

ANSI C Library for PLY file format input and output.

RPly is a library originally written by Diego Nehab, that lets applications
read and write PLY files. The PLY file format is widely used to store geometric
information, such as 3D models, but is general enough to be useful for other
purposes.

This library is based on RPly 1.1.3 http://w3.impa.br/~diego/software/rply/

Copyright © 2003-2013 Diego Nehab http://www.cs.princeton.edu/~diego

Modified 2009-2011 by Lars Kiesow http://larskiesow.de lkiesow@uos.de

Modified 2014 by Jiri Hnidek jiri.hnidek@tul.cz

## Requirements

* GCC or CLang
* CMake
* Make

## Compilation

For compilation of the library simply call

    $ mkdir build
    $ cd ./build
    $ cmake ../
    $ make
    $ su make install

## Usage

Most users that want to read a PLY file already know which elements and
properties they are interested in. In the following example, we will
implement a simple program that dumps the contents of a PLY file to
the terminal, in a different, simpler format that only works for triangles.

This simple format has a header that gives the number of vertices in the
first line and the number of triangles in the second line. Following header
come the vertices, and finally the triangles. Here is the sample code of
program:

```c
#include <stdio.h> 
#include "rply.h"

static int vertex_cb(p_ply_argument argument) {
    long eol;
    ply_get_argument_user_data(argument, NULL, &eol);
    printf("%g", ply_get_argument_value(argument));
    if (eol) printf("\n");
    else printf(" ");
    return 1;
}

static int face_cb(p_ply_argument argument) {
    long length, value_index;
    ply_get_argument_property(argument, NULL, &length, &value_index);
    switch (value_index) {
        case 0:
        case 1: 
            printf("%g ", ply_get_argument_value(argument));
            break;
        case 2:
            printf("%g\n", ply_get_argument_value(argument));
            break;
        default: 
            break;
    }
    return 1;
}

int main(void) {
    long nvertices, ntriangles;
    p_ply ply = ply_open("input.ply", NULL, 0, NULL);
    if (!ply) return 1;
    if (!ply_read_header(ply)) return 1;
    nvertices = ply_set_read_cb(ply, "vertex", "x", vertex_cb, NULL, 0);
    ply_set_read_cb(ply, "vertex", "y", vertex_cb, NULL, 0);
    ply_set_read_cb(ply, "vertex", "z", vertex_cb, NULL, 1);
    ntriangles = ply_set_read_cb(ply, "face", "vertex_indices", face_cb, NULL, 0);
    printf("%ld\n%ld\n", nvertices, ntriangles);
    if (!ply_read(ply)) return 1;
    ply_close(ply);
    return 0;
}
```

