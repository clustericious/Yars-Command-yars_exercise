# NAME

yars\_exercise - Exercise a Yars server from the client side

# VERSION

version 0.02

# SYNOPSIS

    % yars_exercise [log options] [options]

# DESCRIPTION

This command provides performance testing for [Yars](https://metacpan.org/pod/Yars).  Forks a number
of processes and sends requests to the Yars server, using randomly
generated files.  Produces a report with the average `PUT`, `GET`
and `DELETE` times.

All actions are performed through [Yars::Client](https://metacpan.org/pod/Yars::Client).  It uses the
[upload](https://metacpan.org/pod/Yars::Client#upload), [download](https://metacpan.org/pod/Yars::Client#download)
and [remove](https://metacpan.org/pod/Yars::Client#remove) methods.

For each client, it randomly shuffles the order of uploads, downloads, and
removes.  The only guarantee is that for each individual file, the first
action is an upload and the last is a remove.  With multiple processes,
this can cause the various operations to intermingle.

Uses [Log::Log4perl](https://metacpan.org/pod/Log::Log4perl) and [Log::Log4perl::CommandLine](https://metacpan.org/pod/Log::Log4perl::CommandLine), so you can specify
any logging options they support, e.g. `--debug root` will log a note
with the elapsed time for each action, `--trace Yars::Client` will log
detailed trace log messages from the client, etc.

# OPTIONS

This command also recognizes all options supported by
[Log::Log4perl::CommandLine](https://metacpan.org/pod/Log::Log4perl::CommandLine).

## --numclients _n_

The number of processes to fork.

## --files _n_

The number of random files to produce.

## --size _size_

The size of the files.  You can use any suffix supported by
[Number::Bytes::Human](https://metacpan.org/pod/Number::Bytes::Human).

## --gets _n_

The number of GETs to perform for each client.

## --runs _filename_

Put your config options in a YAML file and specify it
with "--runs" or "-r":

    % cat runs_desc.yml
    ---
    clients: [2,4]
    files: [5,10]
    gets: [10,20,40,80]
    size: [256,256K,8M]

If you list more than one option, it iterates through various
parameters listed.

`--runs` also outputs CSV of stats from each run.

## --chunksize _size_

Chunksize is only used for creating the temp files, changing it won't
affect the Yars actions.  You can use any suffix supported by
[NumbeR::Bytes::Human](https://metacpan.org/pod/NumbeR::Bytes::Human)

## --help

Display help for this command.

## --version

Prints the [Yars](https://metacpan.org/pod/Yars) version.

# EXAMPLES

    $ yars_exercise --version -h|--help -m|--man

    $ yars_exercise -v|--verbose -q|--quiet [...other log options]

    $ yars_exercise --numclients 4 --files 20 --size 8KiB --gets 10
                    --temppath /tmp

    $ yars_exercise -n 4 -f 20 -s 8KiB -g 10 -t /tmp

    $ yars_exercise [with no options, uses the defaults above]

    $ yars_exercise -runs runs_desc.txt

# AUTHOR

Original author: Curt Tilmes

Current maintainer: Graham Ollis &lt;plicease@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2015 by NASA GSFC.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
