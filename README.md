---
description: >-
  This page describes general development workflow and best practices for Linux
  kernel Development
---

# Development Process

## Workflow

1. Clone Linus Torvalds linux\_mainline repository(git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git).
2. If you already have repository you need to fetch latest changes. If your patch is out-of-date and doesn't apply to the latest tree, it may be rejected. create path on top of the latest commit from the tree. execute following to fetch the latest changes:\
   `git fetch origin`
3. We can pick a tag to rebase to. - If we need to update our branch to include the changes in the remote tree. The safest way to do this is to "rebase" your branch. This means that if you have any commits on your branch, they will be placed on top of the remote tree commits. Sometimes you may have to edit your commits if there are conflicts\
   `git rebase origin/master`\
   ``If you run `git log` to show your staging branch history and then `git log origin/master` to show the master branch history, you should see that they have exactly the same commits.
4. Create a new branch in the **linux\_mainline** repository master branch to write your patch.
5. Do changes you want to do in kernel subsytem/module/driver.
6.  Once you have the name of a kernel subsytem/module/driver, it's time to find out where the **.c** and **.h** files for that driver are in the Linux kernel repository. Even though searching **Makefiles** will get you the desired result, **git grep** will get you there faster, searching only the checked-in files in the repository. **git grep** will skip all generated files such as **.o**’s, **.ko**’s and binaries. It will skip the **.git** directory as well. Run specifically on makefile so we can get path of source files.\
    e.g., let’s run **git grep** to look for **uvcvideo** files.

    `git grep uvcvideo -- '*Makefile'`
7. After getting path of source files of required kernel subsytem/module/driver list down source/header files using `ls` command and do require changes in that file and compile.
8. You probably also want to make sure your kernels have magic **sysrq** support build in so you can (at least attempt to) do an emergency flush, and unmount of your filesystems when the box hangs followed by an emergency reboot. That's a lot better than just hitting the power button. **sysrq** can also be used to capture stack traces for running programs, get memory dumps etc. See _Documentation/admin-guide/sysrq.rst_ for details.
9. First check if your changes follow the coding guidelines outlined in the [_Linux kernel coding style_ guide](https://www.kernel.org/doc/html/latest/process/coding-style.html).
10. You can run [**checkpatch.pl**](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/scripts/checkpatch.pl) on the diff or the generated patch to verify if your changes comply with the coding style. It is good practice to check by running **checkpatch.pl** on the diff before testing and committing the changes. I find it useful to do this step even before I start testing my patch. This helps avoid redoing testing in case code changes are necessary to address the **checkpatch** errors and warnings.\
    `git diff > temp`

    `scripts/checkpatch.pl remp`
11.

    ![Development workflow](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/vxbogxwyk2hp-Patchworkflow.png)


12. Make sure you address **checkpatch** errors and/or warnings. Once **checkpatch** is happy.
13. [#config](toolchain-and-development-process/toolchain/config-and-build.md#config "mention")- You'll need to make sure the driver you're changing is configured as a module/built in and config file has been saved.
14. [#build](toolchain-and-development-process/toolchain/config-and-build.md#build "mention") - You may need to fix some compilation errors. Also fix any new warnings that are due to your changes. Make sure that drivers do not produce warnings on anyone's system (this includes 32-bit and 64-bit systems, as well as different architectures, such as PPC, ARM, or x86). New features or bug fix patches that add additional warnings may not get merged.
15. Install kernel
16. Test your changes by loading module runtime and at the boot time(builtin) both.
17. Once a new kernel is installed, the next step is to try to boot it and see what happens. Once the new kernel is up and running, check **dmesg** for any regressions.
18. Run a few usage tests:
    * Is networking (wifi or wired) functional?
    * Does ssh work?
    * Run rsync of a large file over ssh
    * Run **git clone** and **git pull**
    * Start a web browser
    * Read email
    * Download files: **ftp**, **wget**, etc.
    * Play audio/video files
    * Connect new USB devices - mouse, USB stick, etc.
19. Examining Kernel Logs -\
    Checking for regressions in **dmesg** is a good way to identify problems, if any, introduced by the new code. As a general rule, there should be no new crit, alert, and emerg level messages in dmesg. There should be no new err level messages. Pay close attention to any new warn level messages as well. Please note that new warn messages are not as bad. At times, new code adds new warning messages which are just warnings.

    `dmesg -t -l emerg` \
    `dmesg -t -l crit` \
    `dmesg -t -l alert` \
    `dmesg -t -l err` \
    `dmesg -t -l warn`\
    `dmesg -t -k` \
    `dmesg -t`\
    ``Are there any stack traces resulting from WARN\_ON in the dmesg? These are serious problems that require further investigation.
20. Stress Testing -\
    Running three to four kernel compiles in parallel is a good overall stress test. Download a few Linux kernel gits, stable, linux-next etc. Run timed compiles in parallel. Compare times with old runs of this test for regressions in performance. Longer compile times could be indicators of performance regression in one of the kernel modules.

    Performance problems are hard to debug. The first step is to detect them. Running several compiles in parallel is a good overall stress test that could be used as a performance regression test and overall kernel regression test, as it exercises various kernel modules like memory, filesystems, dma, and drivers.

    `time make all`
21. Do other required tests specific to changes. If driver built as module load the driver with `modprobe`. You'll be able to see that the driver is loaded by running `lsmod`.
22. Debug Options and Proactive Testing -\
    There are several configuration options to test for locking imbalances and deadlocks. It is important to remember to enable the Lock Debugging and CONFIG\_KASAN for memory leak detection. Enabling the following configuration option is recommended for testing your changes to the kernel:

    CONFIG\_KASAN \
    CONFIG\_KMSAN \
    CONFIG\_UBSAN \
    CONFIG\_LOCKDEP \
    CONFIG\_PROVE\_LOCKING \
    CONFIG\_LOCKUP\_DETECTOR\
    Running git grep -r DEBUG | grep Kconfig can find all supported debug configuration options.
23. Commit Your changes.
24. When you commit a patch, you will have to describe what the patch does. The commit message has a subject or short log and longer commit message. It is important to learn what should be in the commit log and what doesn’t make sense. Including what code does isn’t very helpful, whereas why the code change is needed is valuable. Please read [_How to Write a Git Commit Message_](https://chris.beams.io/posts/git-commit/) for tips on writing good commit messages.
25. Now, run the commit and add a commit message. Document your change and include relevant testing details and results of that testing. As a general rule, don't include change lines in the commit log.
26. After you make the commit, git post commit hook will output any **checkpatch** errors or warnings that your patch creates. If you see warnings or errors that you know you added, you can amend the commit by changing the file, using **git add** to add the changes, and then using **git commit --amend** to commit the changes. Follow steps from 5 to 17 till you resolve
27. Make sure your commit looks fine by running these commands:
    * `git show HEAD` - This will show the latest commit. If you want git to show a different commit, you can pass the commit ID (the long number that's shown in `git log`, or the short number that's shown in `git log --pretty=oneline --abbrev-commit`
    * `git log`&#x20;
    * `git log --pretty=oneline --abbrev-commit`
28. When working on a patch based on a suggested idea, make sure to give credit using the **Suggested-by** tag. [Other tags](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#using-reported-by-tested-by-reviewed-by-suggested-by-and-fixes) used for giving credit are **Tested-by**, **Reported-by**. **Signed-off-by** should be the last tag.
29. The next step is learning the how to send a patch to the Linux Kernel mailing lists for review. The **get\_maintainer.pl** script tells you whom to send the patch to. The **get\_maintainer.pl** show the list of people to send patches to. You should send the patch to maintainers, commit signers, supporters, and all the mailing lists shown in the **get\_maintainer.pl**’s output. Mailing lists are on the “**cc**” and the rest are on the “**To**” list when a patch is sent. The perl script in the kernel source directory `scripts/get_maintainer.pl` will either take a git commit or a file, and tell you who to send your patch to. Note that run get\_maintainer.pl script on file which has been modified.\
    e.g., git show HEAD | perl scripts/get\_maintainer.pl\
    `scripts/get_maintainer.pl -f drivers/media/usb/uvc/uvc_driver.c`
30. Create a patch that describes the change, using \
    `git format-patch`. \
    That command takes a starting commit ID (and optionally) an ending commit ID, in order to create patches for the commit after the starting commit ID. \
    The -o flag specifies where to put the patch. \
    e.g., following command will generate patch

    `git format-patch -1 -o <directory name where patch will be created> <commit ID> --to=<email id from output of get_maintainer.pl script> --to=<email id from output of get_maintainer.pl script> --cc=<email id from output of get_maintainer.pl script> --cc=<email id from output of get_maintainer.pl script>`
31. If you need to, squash your commits you'd like to email as a patch into a single commit. There are multiple squash techniques, but here is one common way:

    ```
    git rebase -i my_first_commit~  # do NOT forget the tilde (~) at the end!
    # In the git editor that opens up, manually set the first entry to 
    # "pick", and set all others afterwards to "squash", or just "s" for short.
    # Save, then close the editor and let it squash.

    ```
32. Create a patch file for your latest, squashed commit:\
    `git format-patch -1 HEAD`
33. Run scripts**/checkpatch.pl** before sending the patch. (Note that **checkpatch.pl** might suggest changes that are unnecessary! Use your best judgement when deciding whether it makes sense to make the change **checkpatch.pl** suggests. The end goal is for the code to be more readable. If **checkpatch.pl** suggests a change and you think the end result is not more readable, don't make the change. For example, if a line is 81 characters long, but breaking it makes the resulting code look ugly, don't break that line.) Also review patch manually.
34. copy the patch file. send your copied patch to yourself(you need remove to & cc list in patch and add only your email id in to). Save it as raw text including all the headers. Run `git am raw_email.txt` and then review the changelog with `git log`. When that works then send the patch to the appropriate mailing list(s).
35. now you can send this original patch using:

    `mutt -H <patch_file>`

    `git send-email <patch_file>`

    _****_[_**Note than your system shall be configured properly for .gitconfig before sending email**_](#user-content-fn-1)[^1]_**.**_
36. After this you may get feedback/review comments/changes requested in commit

## Best Practices

### Communication and  behavioral

* learn from mistakes - assess comments and note down errors made and avoid them in future
* Be patient and wait for a minimum of one week before requesting for comments. It could take longer than a week during busy periods such as the merge windows.
* Always thank the reviewers for their feedback and address them.
* Don’t hesitate to ask a clarifying question if you don’t understand the comment.
* Remember that the reviewers help improve code. Don’t take it personally and handle the feedback gracefully
* Keep in mind that the community doesn’t have any obligation to accept your patch. Patches are pulled, not pushed. Always give a reason for the maintainer to take your patch.
* Be patient and be ready to make changes and working with the reviewers. It could take multiple versions before your patch gets accepted. It is okay to disagree with maintainers and reviewers. Please don't ignore a review because you disagree with it. Present your reasons for disagreeing, along with supporting technical data such as benchmarks and other improvements.
* In general, getting response and comments is a good sign that the community likes the patch and wants to see it improved. Silence is what you want to be concerned about. If you don't hear any response back from the maintainer after a week, feel free to either send the patch again, or send a gentle "ping" - something like _"Hi, I know you are busy, but have you found time to look at my patch?"_
* Expect to receive comments and feedback at any time during the review process.
* When a maintainer accepts a patch, the maintainer assumes maintenance responsibility for that patch. As a result, maintainers have decision power on how to manage patch flow into their individual sub-system(s) and they also have individual preferences. Be prepared for maintainer-to-maintainer differences in commit log content and sub-system specific coding styles.

### Patch

* Make sure to include a blank line between your short description (what will become the Subject line of your patch) and the body of your patch. Make sure there is a blank line between the body of your patch and your Signed-off-by line.
* Please don’t do top post when responding to emails. Responses should be inline.
* Stay engaged and be ready to fix problems, if any, after the patch gets accepted into **linux-next** for integration into the mainline. Kernel build and Continuous Integration (CI) bots and rings usually find problems.
* When a patch gets accepted, you will either see an email from the maintainer or an automated patch accepted email with information on which tree it has been applied to, and some estimate on when you can expect to see it in the mainline kernel. Not all maintainers might send an email when the patch gets merged. The patch could stay in **linux-next** for integration until the next merge window, before it gets into Linus's tree. Unless the patch is an actual fix to a bug in Linus's tree, in which case, it may go directly into his tree.
* Sometimes you need to send multiple related patches. This is useful for grouping, say, to group driver clean up patches for one particular driver into a set, or grouping patches that are part of a new feature into one set. **git format-patch -2 -s --cover-letter --thread --subject-prefix="PATCH v3" --to= “name” --cc=” name”** will create a threaded patch series that includes the top two commits and generated cover letter template. It is a good practice to send a cover letter when sending a patch series.
* Including patch series version history in the [cover letter](https://lkml.org/lkml/2019/9/17/657) will help reviewers get a quick snapshot of changes from version to version.
* You also don't want to mix different types of changes in the same patch. If for example you have been doing a warning fix as well as a general [CodingStyle](https://kernelnewbies.org/CodingStyle) cleanup of a file, then don't include both in the same patch but submit two distinct patches instead (in 2 emails) that each do just one thing.

[^1]: 
