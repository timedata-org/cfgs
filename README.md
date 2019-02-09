`cfgs`
-------------

Simple, correct handling of config, data and cache files
==========================================

Like everyone else, I wrote a lot of programs which saved config files
as dotfiles in the user's home directory - `~/.my-program-name` and now
everyone's home directory has dozens of these.

Great was my embarrasment to discover that there was a neat little specification
for data, config and cache directories in Linux that prevents this problem, and
that I was not using it:  https://0x46.net/thoughts/2019/02/01/dotfile-madness/

So I implemented a small and simple Python API as a single file, `cfgs.py`.
It works on all versions of Python from 2.7 to 3.7, has full test coverage,
and all the functionality is reachable from a single class, `cfgs.Project`

How to install `cfgs`
===============

You can either use pip:

    pip install cfgs

Or if you don't like dependencies (and who does?), you can drop the source file
[`cgfs.py`](https://raw.githubusercontent.com/timedata-org/cfgs/master/cfgs.py)
right into your project.

Usage examples
==================

    import cfgs
    pr = cfgs.Project('my-project')
    print(pr.xdg.XDG_CACHE_HOME)
    #   /home/tom/.cache/my-project

    pr.xdg.XDG_CONFIG_DIRS
    #   /etc/xdg

    with pr.config.open() as f:
        f['name'] = 'oliver'
        f['description'] = {'size': 'small", 'fur': 'brown'}
        print(f.filename)
    #    /home/tom/.cache/my-project/my-project.json

    # Later:
    with pr.config.open() as f:
        print(f['name'], f.as_dict())
    #    oliver {'name': 'oliver',
    #            'description': {'size': 'small", 'fur': 'brown'}



Using `cfgs` In legacy code
=================

If you already have code to handle your config, data and cache files, then you
can just use `cgfs` to get the
[XDG variables](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html):

    from cfgs import XDG

    xdg = XDG()
    config_dir = xdg.XDG_CONFIG_HOME

    # Your code here - eg:
    my_config_file = os.path.join(config_dir, 'my-file.json')
    with open(my_config_file) as f:
        legacy_write_my_file(f)


`cfgs` automatically handles data and config files, and independently, cache
directories.

Data and config files



Complies with the XDG Base Directory Specification.
