====================================================
OCLint package for Arch Linux and Arch-based distros
====================================================

First of all, big thanks to `OCLint <https://github.com/oclint/oclint>`_
developers. It's an awesome linter.

Building Arch package
=====================

.. code-block:: bash
   
   git clone https://github.com/thodnev/oclint-arch.git oclint
   cd oclint
   makepkg -s

Installing
==========

.. code-block:: bash
   
   sudo pacman -U oclint-*.tar.zst

Or, hopefully soon it will be added to `AUR <https://aur.archlinux.org/>`_
and you'll be able to build&install in one command with something like
`YAY <https://github.com/Jguer/yay>`_:

.. code-block:: bash
   
   yay -Sa ...

