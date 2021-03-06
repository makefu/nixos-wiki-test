---
title: Howto keep multiple packages up to date at once
permalink: /Howto_keep_multiple_packages_up_to_date_at_once/
---

The standard way
----------------

to keep a set of packages up-to-date in Nix is to install them into a profile -- typically the default profile -- and to run

    nix-env -u \*

or possibly even

    nix-env -u \* --always

However there are some pitfalls:

1) lowPrio packages will be used last. Thus if lowPrio is added to nixpkgs for any reason it may happen that -u which should update packages even downgrades a package. Even experienced users do run into this problem at least once in their life :)

2) You don't know about packages which either got renamed or removed. Then month later you depend on something and its gone wondering why - cause you've always had it in your env.. (You can always install old versions using hydra, still you want to know about those changes). This is one way to detect missing or renamed packages

     nix-env -i $(nix-env -q \*) --dry-run

3) nix-env -u is slow. Because it does not use attr paths all of nixpkgs has to be evaluated always.

4) It may happen that you uninstall gimp keeping gimp plugins in your env. Then you install a newer gimp and wonder why the older plugin does no longer match the newer gimp, why it does no longer work. You'll spend time on debugging it. Don't take me verbatim. This is an example about which kind of issues may happen if you don't use collections for some purposes. [Aksesoris mobil](http://kiosauto.com), [Sewa mobil jakarta](http://www.awanirentcar.com), [Sewa mobil jakarta selatan](http://www.awanirentcar.com/about-us), [Toko bunga online jakarta](http://www.tokobungasabana.com), [Railing tangga](http://trimasjaya.com/products/railing_tangga/index.html), [Pintu dan jendela](http://www.trimasjaya.com/pintu-dan-jendela/index.html)

The collection way
------------------

The alternative is using package collections referring to packages by name. Nix offers a mechanism for that purpose, too, namely the `buildEnv` function, which can be used as follows:

    # ~/.nixpkgs/config.nix

    {
      packageOverrides = pkgs:
      {
        haskellEnv = pkgs.buildEnv {
          name = "haskell-packages";
          paths = with pkgs.haskellPackages; [ghc QuickCheck cmdlib];
        };
        # same can be done for
        # myPerlEnv = perl and libs
        # myPythonEnv = python and libs
        # myGimp = gimp and plugins
        # all = pkgs.buildEnv { name = "all"; paths = [haskellEnv myPerlEnv myPythonEnv ... ]; };
      };
    }

That code snippet creates a "collection package", which consists of the union of several others, and it can be installed by running either of the following commands:

    nix-env -iA haskellEnv # accessed by attribute thus faster
    nix-env -i haskell-packages

The easiest way to update a collection is to reinstall it because nix-env -i usually replaces packages having the same name. Thus `nix-env -iA all` will tell you that everything still evaluates and everything is fine. One way to be done.

The collection `name` attribute usually does not include a version number, thus "`nix-env -u \*`" is typically not going to update that package even if its components have changed. In that case, it is necessary to pass the `--always` flag to the update operation, to evaluate everything from the ground up.

Downside: `nix-env -q \*` will not show the contents of a collection. You have to fall back to nix-store -q operations or look up the contents in your ~/.nixpkgs/config.nix file

**Its hard to say which way is better. Try both and use what fits your needs.**

[Category:Installation](/Category:Installation "wikilink") [Category:Nixpkgs](/Category:Nixpkgs "wikilink")