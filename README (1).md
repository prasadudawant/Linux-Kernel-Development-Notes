---
description: >-
  This page describes general development workflow and best practices for Linux
  kernel Development
---

# Development Process

## Workflow

1. [#edit-the-source-code-and-make-your-changes](toolchain-and-development-process/development-tools/browse-and-edit.md#edit-the-source-code-and-make-your-changes "mention")
2. [#configure-your-kernel](toolchain-and-development-process/development-tools/config-and-build.md#configure-your-kernel "mention") - You'll need to make sure the driver you're changing is configured as a module/built in and config file has been saved.
3. [#build-your-kernel](toolchain-and-development-process/development-tools/config-and-build.md#build-your-kernel "mention") - You may need to fix some compilation errors. Also fix any new warnings that are due to your changes. Make sure that drivers do not produce warnings on anyone's system (this includes 32-bit and 64-bit systems, as well as different architectures, such as PPC, ARM, or x86). New features or bug fix patches that add additional warnings may not get merged.
4. [#test-your-changes](toolchain-and-development-process/development-tools/test-and-verify.md#test-your-changes "mention")
5. [#commit-your-changes](toolchain-and-development-process/development-tools/code-review-and-submit-changes.md#commit-your-changes "mention")
6. [#create-and-send-patch](toolchain-and-development-process/development-tools/code-review-and-submit-changes.md#create-and-send-patch "mention")

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
