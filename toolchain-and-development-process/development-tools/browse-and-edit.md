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
   2. **-stable** i.e. **2.6.x.y**
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

## Edit the source code and make your changes

1. Clone Linus Torvalds linux\_mainline repository(git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git).
2. If you already have repository you need to fetch latest changes. If your patch is out-of-date and doesn't apply to the latest tree, it may be rejected. create path on top of the latest commit from the tree. execute following to fetch the latest changes:\
   `git fetch origin`
3. We can pick a tag to rebase to. - If we need to update our branch to include the changes in the remote tree. The safest way to do this is to "rebase" your branch. This means that if you have any commits on your branch, they will be placed on top of the remote tree commits. Sometimes you may have to edit your commits if there are conflicts\
   `git rebase origin/master`\
   ``If you run `git log` to show your staging branch history and then `git log origin/master` to show the master branch history, you should see that they have exactly the same commits.
4. Create a new branch in the **linux\_mainline** repository master branch to write your patch.
5. Do changes you want to do in kernel subsytem/module/driver.
6. Once you have the name of a kernel subsytem/module/driver, it's time to find out where the **.c** and **.h** files for that driver are in the Linux kernel repository. Even though searching **Makefiles** will get you the desired result, **git grep** will get you there faster, searching only the checked-in files in the repository. **git grep** will skip all generated files such as **.o**’s, **.ko**’s and binaries. It will skip the **.git** directory as well. Run specifically on makefile so we can get path of source files.\
   e.g., let’s run **git grep** to look for **uvcvideo** files.
7. `git grep uvcvideo -- '*Makefile'`
8. After getting path of source files of required kernel subsytem/module/driver list down source/header files using `ls` command and do require changes in that file and compile.
9. You probably also want to make sure your kernels have magic **sysrq** support build in so you can (at least attempt to) do an emergency flush, and unmount of your filesystems when the box hangs followed by an emergency reboot. That's a lot better than just hitting the power button. **sysrq** can also be used to capture stack traces for running programs, get memory dumps etc. See _Documentation/admin-guide/sysrq.rst_ for details.
10. First check if your changes follow the coding guidelines outlined in the [_Linux kernel coding style_ guide](https://www.kernel.org/doc/html/latest/process/coding-style.html).
11. You can run [**checkpatch.pl**](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/scripts/checkpatch.pl) on the diff or the generated patch to verify if your changes comply with the coding style. It is good practice to check by running **checkpatch.pl** on the diff before testing and committing the changes. I find it useful to do this step even before I start testing my patch. This helps avoid redoing testing in case code changes are necessary to address the **checkpatch** errors and warnings.\
    `git diff > temp`\
    `scripts/checkpatch.pl temp`\
    ``

    <figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Development workflow [courtesy: Linux Kernel Foundation]</p></figcaption></figure>
12. Make sure you address **checkpatch** errors and/or warnings. Once **checkpatch** is happy.
