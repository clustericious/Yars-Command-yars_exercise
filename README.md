# NAME

yars\_exercise - Exercise a Yars server from the client side

# VERSION

version 0.01

# SYNOPSIS

    $ yars_exercise --version -h|--help -m|--man

    $ yars_exercise -v|--verbose -q|--quiet [...other log options]

    $ yars_exercise --numclients 4 --files 20 --size 8KiB --gets 10
                    --temppath /tmp

    $ yars_exercise -n 4 -f 20 -s 8KiB -g 10 -t /tmp

    $ yars_exercise [with no options, uses the defaults above]

# DESCRIPTION

Forks &lt;numclients>.  Each client first creates &lt;files> files of size
&lt;size> filled with random bytes, Then it PUTs them to Yars, GETs them
&lt;gets> times, then DELETEs.

For each client, it randomly shuffles the order of PUTs, GETs, and
DELETEs, so it may PUT 1 file, PUT another, GET the first, PUT a
third, GET the second, DELETE the first, etc.  The only guarantee is
that for each individual file, the first action on that file is a PUT,
the last is a DELETE.  With multiple clients, this causes GETs/PUTs to
intermingle.

All actions are performed through Yars::Client -- it uses the
upload(), download() and remove() methods.

size and chunksize can be specified with K, KB, KiB, M, MB, MiB, etc.

chunksize is only used for creating the temp files, changing it won't
affect the Yars actions.

# LOGGING

Uses Log::Log4perl and Log::Log4perl::CommandLine, so you can specify
any logging options they support, e.g. "--debug root" will log a note
with the elapsed time for each action, "--trace Yars::Client" will log
detailed trace log messages from the client, etc.

# AUTHOR

Original author: Curt Tilmes

Current maintainer: Graham Ollis &lt;plicease@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2015 by NASA GSFC.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
