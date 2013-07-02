[![Build Status](https://secure.travis-ci.org/progit/progit.png?branch=master)](https://travis-ci.org/progit/progit)

# Am Git Book Contents

_Am Git_, by Allen B. Downey, is a modified version of _Pro Git_ by Scott Chabon.

Like the original, it is licensed under
the Creative Commons Attribution-Non Commercial-Share Alike 3.0 license.


# Making Ebooks

On Fedora (16 and later) you can run something like this::

    $ yum install ruby calibre rubygems ruby-devel rubygem-ruby-debug rubygem-rdiscount
    $ makeebooks en  # will produce a mobi

On MacOS you can do like this::
	
1. INSTALL ruby and rubygems
2. `$ gem install rdiscount`
3. DOWNLOAD Calibre for MacOS and install command line tools
4. `$ makeebooks zh` #will produce a mobi

On Ubuntu you can run something like this:

    $ sudo apt-get install pandoc texlive-xetex

    $ makepdfs en

