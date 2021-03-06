# NAME

Carmel - CPAN Artifact Repository Manager

# SYNOPSIS

    # Run with a directory with cpanfile or cpanfile.snapshot
    carmel install

    # Manually pull a module if you don't have it
    carmel install DBI@1.633 Plack@1.0000

    # list all the modules to be loaded
    carmel list

    # list all the modules in a tree
    carmel tree

    # show a location where a module is installed
    carmel show Plack

    # Runs your perl script with modules from artifacts
    carmel exec perl ...

    # See which INC entry is added
    carmel exec perl -V

    # Runs your perl script with a checker to guarantee everything is loaded from Carmel
    carmel exec perl -MDevel::Carmel script.pl

    # prints export PERL5LIB=... etc for shell scripting
    carmel export

    # find a module in a repository
    carmel find DBI

    # find a module matching the version query
    carmel find Plack ">= 1.0000, < 1.1000"

# DESCRIPTION

**THIS IS EXPERIMENTAL!**

Carmel is yet another CPAN module manager.

Unlike traditional CPAN module installer, Carmel keeps the build of
your dependencies in a central repository, then select the library
paths to include upon runtime.

# HOW IT WORKS

Carmel will keep the build directory (artifacts) after a cpanm
installation in a repository, which defaults to `$HOME/.perl-carmel/{version}-{archname}/builds`,
and your directory structure would look like:

    $HOME/.perl-carmel/5.20.1-darwin-2level/builds
      Plack-1.0033/
        blib/
          arch/
          lib/
      URI-1.64/
        blib/
          arch/
          lib/
      URI-1.63/
        blib/
          arch/
          lib/

Carmel scans this directory and creates the mapping of which version
of any package belongs to which build directory.

Given the list of modules and requirements (using `cpanfile` or even
better `cpanfile.snapshot` from [Carton](https://metacpan.org/pod/Carton)), Carmel lists all the
build directories you need, and then prepend the `blib` directories
to `PERL5LIB` environment variables.

For example, if you have:

    requires 'URI', '== 1.63';

Carmel finds URI package with `$VERSION` set to 1.63 in
`URI-1.63/blib/lib` so it will set that directory. Instead, if you
have:

    requires 'URI';

it will find the latest that satisfies the (empty) requirement, which
is in `URI-1.64/blib/lib`.

The fact that it prefers the latest, rather than the oldest, might
change in the future once a mechanism to make snapshot is instroduced,
since you will not like to upgrade one of the dependencies
unexpectedly.

You have a choice to execute a subprocess from Carmel, by using the
`exec` sub command. If you prefer a fine grained control, you can
also use `env` or `export` command to integrate with your own shell
script wrapper.

# AUTHOR

Tatsuhiko Miyagawa <miyagawa@bulknews.net>

# COPYRIGHT

Copyright 2015- Tatsuhiko Miyagawa

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

[App::cpanminus](https://metacpan.org/pod/App::cpanminus) [Carton](https://metacpan.org/pod/Carton)
