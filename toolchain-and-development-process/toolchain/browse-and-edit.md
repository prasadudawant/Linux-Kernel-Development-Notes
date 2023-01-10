# Browse and Edit

## Source tree navigation

1. **lxr** - The Linux cross-referencer, lxr, downloadable from [http://lxr.sourceforge.net/](http://lxr.sourceforge.net/), lets you traverse the kernel tree using a web browser by providing hyperlinks to connect kernel symbols with their definitions and uses.
2. [https://elixir.bootlin.com/linux/latest/source](https://elixir.bootlin.com/linux/latest/source)

## Different Linux Kernel Trees

1. Review [https://www.kernel.org/category/releases.html](https://www.kernel.org/category/releases.html)
2. **some trees**
   1. **2.6.X** and **2.6.X-rcY**
      *   Maintainer: Linus Torvalds

          Goal is to release a new kernel every 8-12 weeks. Once release 2.6.X is released, 2.6.X+1 is opened for 2 weeks, and patches are merged, often from -mm and other maintainer trees, but also from LKML(Linux kernel mailing list) directly. After the 2 week 'open-season', -rc1 is released, which begins the slow freeze from 'slushy', and hardening slowly as -rc2, and successors are released, as Linus sees fit. These are also called **current**, and **mainline** Once new releases are made, other trees are re-based against the new release.&#x20;
   2. **-stable** ie **2.6.x.y**
      * Maintainer: Greg Kroah-Hartman This tree gets bug-fixes after 2.6.x is released. Patches must be:
        * small - less than 100 lines
        * already in current (or equivalent form)
        * fixes actual, known, bug
        * submitted to [stable@kernel.org](mailto:stable@kernel.org)
   3. **-mm**
      *   Maintainer: Andrew Morton.

          The main staging/integration tree for new features and changes that are being considered for merging into the **current** kernel tree maintained by Linus.

          * typically has 1000-1900 patches, packaged in a single, large, compressed diff.
          * provides an easy way to get all the work, and is intended to expose them to much wider testing, on many platforms, than the submitters can cajole others to try.
          * patches which survive this testing/integration are eventually moved into mainline, and dropped from -mm
          * patches which have problems may be dropped if insufficient progress is made to correct them, but which are often re-added later.
          * generally excludes work that has another home tree, until its formally submitted for inclusion into mainline.
          * new drivers tend to spend less time here than patches to the kernel itself, or subsystems.

          New patches enter -mm whenever Andrew thinks its warranted, but generally is done in-phase with open-season. This is probably due to the preferences of the various maintainers who push patch sets to him.
   4. **-rt**
      * Maintainer: Ingo Molnar. \
        Realtime-preempt is a set of patches for the latest version of the Linux kernel. They bring the pre-emption capability to the next level. It's goal is to make the full fledged Linux kernel have as close to real-time response as possible.



****
