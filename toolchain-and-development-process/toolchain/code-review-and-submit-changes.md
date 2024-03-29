---
description: >-
  This page explains system and tool configuration required for code review and
  submit changes.
---

# Code Review and Submit Changes

## Configure your system for sending patches for review

[#git-email-configurations](code-review-and-submit-changes.md#git-email-configurations "mention")

[#gmail-account-setup-server](code-review-and-submit-changes.md#gmail-account-setup-server "mention")

[#git-send-email-gitconfig-setup-client](code-review-and-submit-changes.md#git-send-email-gitconfig-setup-client "mention")

[#setting-up-mutt-with-gmail-on-ubuntu-email-client](code-review-and-submit-changes.md#setting-up-mutt-with-gmail-on-ubuntu-email-client "mention")

[#git-post-commit-hooks](code-review-and-submit-changes.md#git-post-commit-hooks "mention")

## Commit your changes

1. Commit Your changes.
2. When you commit a patch, you will have to describe what the patch does. The commit message has a subject or short log and longer commit message. It is important to learn what should be in the commit log and what doesn’t make sense. Including what code does isn’t very helpful, whereas why the code change is needed is valuable. Please read [_How to Write a Git Commit Message_](https://chris.beams.io/posts/git-commit/) for tips on writing good commit messages.
3. Now, run the commit and add a commit message. Document your change and include relevant testing details and results of that testing. As a general rule, don't include change lines in the commit log.
4. After you make the commit, git post commit hook will output any **checkpatch** errors or warnings that your patch creates. If you see warnings or errors that you know you added, you can amend the commit by changing the file, using **git add** to add the changes, and then using **git commit --amend** to commit the changes. Refer [#revising-the-patches](code-review-and-submit-changes.md#revising-the-patches "mention") Follow steps from [#workflow](<../../README (1).md#workflow> "mention") untill you resolve all errors.
5. When working on a patch based on a suggested idea, make sure to give credit using the **Suggested-by** tag. [Other tags](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#using-reported-by-tested-by-reviewed-by-suggested-by-and-fixes) used for giving credit are **Tested-by**, **Reported-by**. **Signed-off-by** should be the last tag.
6. Make sure your commit looks fine by running these commands:
   * `git show HEAD` - This will show the latest commit. If you want git to show a different commit, you can pass the commit ID (the long number that's shown in `git log`, or the short number that's shown in `git log --pretty=oneline --abbrev-commit`
   * `git log`&#x20;
   * `git log --pretty=oneline --abbrev-commit`
7. If you are working on patch-set then use guideline mentioned for topics Versioning patchsets, Submitting a patchset at [https://kernelnewbies.org/FirstKernelPatch](https://kernelnewbies.org/FirstKernelPatch)

## Create and send Patch

1. The next step is learning the how to send a patch to the Linux Kernel mailing lists for review. The **get\_maintainer.pl** script tells you whom to send the patch to. The **get\_maintainer.pl** show the list of people to send patches to. You should send the patch to maintainers, commit signers, supporters, and all the mailing lists shown in the **get\_maintainer.pl**’s output. Mailing lists are on the “**cc**” and the rest are on the “**To**” list when a patch is sent. The perl script in the kernel source directory `scripts/get_maintainer.pl` will either take a git commit or a file, and tell you who to send your patch to. Note that run get\_maintainer.pl script on file which has been modified.\
   e.g., git show HEAD | perl scripts/get\_maintainer.pl\
   `scripts/get_maintainer.pl -f drivers/media/usb/uvc/uvc_driver.c`
2.  Create a patch that describes the change, using \
    `git format-patch`. \
    That command takes a starting commit ID (and optionally) an ending commit ID, in order to create patches for the commit after the starting commit ID. \
    The -o flag specifies where to put the patch. \
    e.g., following command will generate patch

    `git format-patch -1 -o <directory name where patch will be created> <commit ID> --to=<email id from output of get_maintainer.pl script> --to=<email id from output of get_maintainer.pl script> --cc=<email id from output of get_maintainer.pl script> --cc=<email id from output of get_maintainer.pl script>`
3.  If you need to, squash your commits you'd like to email as a patch into a single commit. There are multiple squash techniques, but here is one common way:

    ```
    git rebase -i my_first_commit~  # do NOT forget the tilde (~) at the end!
    # In the git editor that opens up, manually set the first entry to 
    # "pick", and set all others afterwards to "squash", or just "s" for short.
    # Save, then close the editor and let it squash.
    ```
4. Create a patch file for your latest, squashed commit:\
   `git format-patch -1 HEAD`
5. Run scripts**/checkpatch.pl** before sending the patch. (Note that **checkpatch.pl** might suggest changes that are unnecessary! Use your best judgement when deciding whether it makes sense to make the change **checkpatch.pl** suggests. The end goal is for the code to be more readable. If **checkpatch.pl** suggests a change and you think the end result is not more readable, don't make the change. For example, if a line is 81 characters long, but breaking it makes the resulting code look ugly, don't break that line.) Also review patch manually.
6. copy the patch file. send your copied patch to yourself(you need remove to & cc list in patch and add only your email id in to). Save it as raw text including all the headers. Run `git am raw_email.txt` and then review the changelog with `git log`. When that works then send the patch to the appropriate mailing list(s).
7.  now you can send this original patch using:

    `mutt -H <patch_file>`

    `git send-email <patch_file>`

    _****_[_**Note than your system shall be configured properly for .gitconfig before sending email**_](#user-content-fn-1)[^1]_**. Follow**_ [#configure-your-system-for-sending-patches-for-review](code-review-and-submit-changes.md#configure-your-system-for-sending-patches-for-review "mention")_****_
8. _Refer_ [#send-patches](code-review-and-submit-changes.md#send-patches "mention")_****_
9. After this you may get feedback/review comments/changes requested in commit

## Send patches

[#general-guidelines-for-email-client-and-sending-patches-for-review](code-review-and-submit-changes.md#general-guidelines-for-email-client-and-sending-patches-for-review "mention")

[#using-mutt-email-client](code-review-and-submit-changes.md#using-mutt-email-client "mention")

[#responding-to-emails](code-review-and-submit-changes.md#responding-to-emails "mention")

### git email configurations

we need one server/host and one email client for successful email configuration. I am using gmail as server/host and MUTT as email client. You are free to choose anything. Refer [https://useplaintext.email/](https://useplaintext.email/), [https://www.kernel.org/doc/html/latest/process/email-clients.html](https://www.kernel.org/doc/html/latest/process/email-clients.html). \
git email is also email client but we will use it only for sending patch email and mutt to be able send responses, to review comments, and other communication with the community.

### **Gmail account setup(Server)**

1. create new google account for linux kernel developement. This is optional but I recommend.
2. Configure your Google Account to have&#x20;
   1. **2-factor authentication** (to enable app-specific passwords)
   2. an **app-specific password** for gmail to be used by `git send-email`:
3.  **Configure 2-factor authentication:** \
    go to [https://myaccount.google.com/security](https://myaccount.google.com/security) --> scroll down to "2-Step Verification" and follow the process to turn it on.

    <figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p><strong>Configure 2-factor authentication for gmail</strong></p></figcaption></figure>
4.  **Generate an app-specific password of Gmail for git to use:** \
    go to the link just above, scroll down to "App passwords" and click on it. Select "Mail" as the app and Select "Other _(Custom name)_" as the device. Name device as _git send-email_ or similar. Click the "GENERATE" button. It will pop up with a 16-digit full-access password. Write it down, without the spaces in it. It is just the 16 chars.

    <figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>select "MAIL" as app</p></figcaption></figure>

    <figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>select "other" as device</p></figcaption></figure>


5. Similarly generate password for mutt.
6. In **Gmail,**
   1. click on gear icon and go to settings
   2. &#x20;go to tab _**Forwarding POP/IMAP**_ and click the _**Configuration instructions**_ link in IMAP Access row.
   3. click "I want to enable IMAP"
   4. At the bottom of the page, under the paragraph about configuring your mail client, select _**Other**_.&#x20;
   5. Note the mail server information and use that information for further settings as shown in the next section.

### **git send-email/gitconfig setup(client)**

* we need to do git-email configuration to send patches using send-email command.
* Once you place the configuration in your **.gitconfig** file, running **git send-email mypatch.patch** is all you have to do to send patches; **mypatch.patch** is generated by the **git format-patch** command.

1.  update \~/.gitconfig as follows:

    _Note: change your name, email id and SMTP server details setting if not using Gmail._\
    __read for more at [https://git-send-email.io/](https://git-send-email.io/), [https://useplaintext.email/](https://useplaintext.email/), [https://www.kernel.org/doc/html/latest/process/email-clients.html](https://www.kernel.org/doc/html/latest/process/email-clients.html)

    {% code overflow="wrap" lineNumbers="true" %}
    ```
    [user]
    	name = Prasad Sunil Udawant
    	email = prasad.udawant.linux@gmail.com
    	
    [sendemail]
    	from = Prasad Sunil Udawant <prasad.udawant.linux@gmail.com>
    	smtpuser = prasad.udawant.linux@gmail.com
    	smtpencryption = tls
    	smtpserver = smtp.gmail.com
    	smtpserverPort = 587
    	chainreplyto = false
    	smtpPass = <get this password from google app password we configured in above steps for git-send email app>
    	confirm = always
    	annotate = true

    [format]
        	signoff=true
    [core]
    	editor = vi
    ```
    {% endcode %}
2. set following configuration for safety&#x20;
   1.  to open email in git editor for review.

       _`git config --global sendemail.annotate true`_
   2.  to always confirm before sending email

       _`git config --global sendemail.confirm always`_
   3.  After setting this perform dry-run by executing following to confirm setup is ready. it will not send email.

       _git send-email --to="\<youry email id >" --dry-run \<patch path>_
   4. &#x20;Make sure that the email you specify here is the same email you used to set up sending mail. The Linux kernel developers will not accept a patch where the "From" email differs from the "Signed-off-by" line, which is what will happen if these two emails do not match. Make sure you store your full, legal name in the 'name' line

### **Setting up mutt with Gmail on Ubuntu (email client)**

1. Install mutt\
   _`sudo apt install mutt`_
2. Create directories for mutt to store the cache message headers and bodies, and store certificates, by entering the following commands:\
   _`mkdir -p ~/.mutt/cache/headers`_ \
   _`mkdir ~/.mutt/cache/bodies`_ \
   _`touch ~/.mutt/certificates`_
3. Create a configuration file of mutt: muttrc\
   `touch ~/.mutt/muttrc`
4.  Update muttrc file with following code. Replace Email id and password wherever implied.

    {% code overflow="wrap" lineNumbers="true" %}
    ```
    # .muttrc
    # ================  IMAP ====================
    set imap_user = 'username@gmail.com'
    set imap_pass = <get this password from google app password>
    set folder = imaps://imap.gmail.com/
    set spoolfile = imaps://imap.gmail.com/Inbox
    set record="imaps://imap.gmail.com/Sent"
    set postponed="imaps://imap.gmail.com/[Gmail]/Drafts"
    set mbox="imaps://imap.gmail.com/[Gmail]/All Mail"
    set trash = "imaps://imap.gmail.com/[Gmail]/Trash"
    set header_cache = "~/.mutt/cache/headers"
    set message_cachedir = "~/.mutt/cache/bodies"
    set certificate_file = "~/.mutt/certificates"
    # ================  SMTP  ====================
    set smtp_url = "smtp://username@smtp.gmail.com:587/"
    set smtp_pass = $imap_pass
    set ssl_force_tls = yes # Require encrypted connection
    # ================  Composition  ====================
    set editor = "gedit"      # Set your favourite editor.
    set edit_headers = yes  # See the headers when editing
    set send_charset="us-ascii:utf-8"     # value of $LANG; also fallback for send_charset
    # Sender, email address, and sign-off line must match
    unset use_domain        # because joe@localhost is just embarrassing
    set from = "username@gmail.com"
    set realname = "<should match with your account name with which you are going to signoff and commit>"
    set use_from = yes
    ```
    {% endcode %}
5. To checkout received mails in your Mutt Mailbox, you need to first **enable IMAP settings** from your Gmail account using the below method.
   1. Login to your [**Gmail account**](https://mail.google.com/) from your browser.
   2. From the top right, click **settings** ⚙️ > Then **“See all settings”**.
   3. Click the **Forwarding and POP/IMAP** tab.
   4. In the “IMAP access” section, select **Enable IMAP**.
   5. Finally click on **save changes**
6.  Use this to send the last 3 commits:

    ```
    git send-email HEAD~3
    ```

    Or all commits since a particular one:

    ```
    git send-email 209210d
    ```

    Or just the second-to-last commit:

    ```
    git send-email -1 HEAD^^
    ```
7.  Some projects use a single mailing list for several git repositories. Try this to clarify that you're working on the "foobar" project:

    ```
    git config format.subjectPrefix "PATCH foobar"
    ```

### Git Post-Commit Hooks

* Git includes some _hooks_ for scripts that can be run before or after specific git commands are executed. Checking the patch for compliance and errors can be automated using git pre-commit and post-commit hooks. The _post-commit hook_ is run after you make a git commit with the **git commit** command.
  1. If you don't already have **/usr/share/codespell/dictionary.txt**, do:\
     _`sudo apt-get install codespell`_
  2.  If you already have a **.git/hooks/post-commit** file, move it to **.git/hooks/post-commit.sample**. git will not execute files with the **.sample** extension. Then, edit the **.git/hooks/post-commit** file to contain only the following two lines:

      <pre data-overflow="wrap" data-line-numbers><code><strong>#!bash 
      </strong><strong>#!/bin/sh 
      </strong>exec git show --format=email HEAD | ./scripts/checkpatch.pl --strict --codespell 
      # Make sure the file is executable: 
      chmod a+x .git/hooks/post-commit
      </code></pre>
  3. After you make the commit, this hook will output any **checkpatch** errors or warnings that your patch creates. If you see warnings or errors that you know you added, you can amend the commit by changing the file, using **git add** to add the changes, and then using **git commit --amend** to commit the changes.

### Using mutt email client

1. open mutt gui with executing _`mutt`_ command in terminal
2. send an email by executing following command in terminal\
   `mutt -s "Test!"`\
   ``Press '**Enter'** and it will show up **'To: \<email-id>'**, press '**Enter'** once again. Next, it will display the specified subject **'Subject: Test!'**, again press '**Enter'**. Now it will open your default editor set in your terminal (Use **'echo $EDITOR'** to know) to write Email Body. \
   Once you are done with writing save it. Once you did, it will open up with an interface to confirm everything once again. If you want to make any changes like adding **CC** or **BCC** you can do it now. \
   If you are using VIM editor Press **ESC** and then **SHIFT+ZZ**. \
   For NANO users press **CTRL+X** and then **Y** for Yes. \
   For gedit goto terminal window. Once you are done press **Y** for Yes to send a message to the recipient.
3. You can use the echo command with the Mutt to pass the body of the email with a single command.\
   _`echo "Your Body Message" | mutt -s "Your Subject"  recipientusername@gmail.com`_\
   _``_Press **Enter** and wait for a few second while Mutt authenticating and send the email.
4. Send an email with attachment\
   _`echo "Your Body Message" | mutt -s "Your Subject"  recipientusername@gmail.com -a path-to-file1/file1.txt -a path-to-file2/file2.txt path-to-file3/file3.txt`_
5. Add Recipients in CC and BCC Mode\
   `mutt -s “Your Subject” -c username1_cc@gmail.com -b username2_bcc@gmail.com`\
   ``You can specify multiple CC and BCC, separate them using commas.
6. mutt to use that patch as a draft email, with the -H flag:\
   `mutt -H < your patch filename >`
7. refer [https://www.linux.com/training-tutorials/setup-mutt-gmail-centos-and-ubuntu/](https://www.linux.com/training-tutorials/setup-mutt-gmail-centos-and-ubuntu/), [https://trendoceans.com/how-to-install-and-configure-mutt-command-line-email-client/](https://trendoceans.com/how-to-install-and-configure-mutt-command-line-email-client/), [https://linuxconfig.org/how-to-install-configure-and-use-mutt-with-a-gmail-account-on-linux](https://linuxconfig.org/how-to-install-configure-and-use-mutt-with-a-gmail-account-on-linux)

### General guidelines for email client and sending patches for review

1. **Only Inline text/No attachments** - Patches for the Linux kernel are submitted via email, preferably as inline text in the body of the email. Some maintainers accept attachments, but then the attachments should have content-type `text/plain`. However, attachments are generally frowned upon/disapproved because it makes quoting portions of the patch more difficult in the patch review process.
2. **Plain Text/No HTML** - It’s also strongly recommended that you use plain text in your email body, for patches and other emails alike( No HTML mails. No attachments. ). [https://useplaintext.email](https://useplaintext.email/) may be useful for information on how to configure your preferred email client, as well as listing recommended email clients.
3. Email clients that are used for Linux kernel patches should send the patch text untouched. For example, they should not modify or delete tabs or spaces, even at the beginning or end of lines.( that’s why we don't use Gmail/outlook for Linux kernel development)
4. Don’t send patches with `format=flowed`. This can cause unexpected and unwanted line breaks.
5. Don’t let your email client do automatic word wrapping for you. This can also corrupt your patch.
6. Email clients should not modify the character set encoding of the text. Emailed patches should be in ASCII or UTF-8 encoding only. If you configure your email client to send emails with UTF-8 encoding, you avoid some possible charset problems.
7. Email clients should generate and maintain “References:” or “In-Reply-To:” headers so that mail threading is not broken.
8. Copy-and-paste (or cut-and-paste) usually does not work for patches because tabs are converted to spaces.
9. Don’t use PGP/GPG signatures in mail that contains patches. This breaks many scripts that read and apply the patches. (This should be fixable.)
10. **Plain text** - Please make sure that your email client is configured to use plain text emails. By default, many email clients compose emails with HTML
11. **bottom posting** - Some email clients will paste the entire email you're replying to into your response and encourage you to write your message over it. This behavior is called "top posting" and is discouraged . Instead, cut out any parts of the reply that you're not directly responding to and write your comments inline. Feel free to edit the original message as much as you like. For example: I email you,

    {% code overflow="wrap" lineNumbers="true" %}
    ```
    Hey ABC,
    Can you look into the bug which is causing 2.34 clients to disconnect
    immediately? I think this is related to the timeouts change last week.

    Also, your fix to the queueing bug is confirmed for the next release,
    thanks!
    ```
    {% endcode %}

    You might respond with:

    ```
    Hey XYZ, I can look into that for sure.
    > I think this is related to the timeouts change last week.

    I'm not so sure. I think reducing the timeouts would *improve* this issue,
    if anything.

    > Also, your fix to the queueing bug is confirmed for the next release,
    > thanks!

    Sweet! Happy to help.
    ```

### Responding to emails

1. Respond to emails inline, rather than top posting.
2. This is a [good example of responding inline](http://marc.info/?l=linux-usb\&m=139325835826056\&w=2).
3. Make sure your email client appends '>' characters to inline mail when you respond to it.
4. When you reply inline to a message, the lines you type shouldn't have a '>' symbol at the beginning of the line. Put a blank line before and after their response. This makes it easier to find where some new text has been added.
5.  Example :

    ```
    From: Kludge Crufty <example@email.com>
    Subject: Design decisions for next release

    On Fri, Sep 12, 2014 at 03:00:56PM -0700, Baz Quux wrote:
    > On Fri, 12 September 2014 at 02:30:17PM -0700, Foo Bar wrote:
    > >
    > > I think we should do X.
    >
    > I think we should do Y.

    I think we should do Z.

    Kludge
    ```

    * The email was sent by Kludge. Kludge is responding to an email sent by Baz at 3PM. Baz was responding to an email sent from Foo at 2:30PM. From this snippet of mail, we can tell who said what by looking at the number of '>' symbols in front of each line:
    * Kludge wants to do Z, because their dialog has no '>' in front of it.
    * Baz wants to do Y, because their dialog has one '>' in front, and we follow that '>' level up to Baz's "wrote" line.
    * Foo wants to do X, because their dialog has two '> >' in front, and we follow the last '>' up to Foo's "wrote" line.

### Revising the patches

1. Make the changes - Update the files to match the changes requested by the maintainers.
2. Follow[#workflow](<../../README (1).md#workflow> "mention")
3. commit - add changes to the staging area with `git add`, you can add those changes to your commit with the amend command, along with your previously committed changes.
   1. If the patch is your HEAD commit, you can run:\
      `git commit -a --amend -v` \
      That will allow you to edit the commit message.
   2. If you want to take the previous commit out of the git history, but leave the changes in your working tree, you can run:\
      `git reset --mixed HEAD^`&#x20;
   3. If you want to completely get rid of all your changes, and revert all files to their state before your commit, you can use the --hard flag instead of the --mixed flag. Use this flag with care!
   4. Follow [#editing-patches-in-series](code-review-and-submit-changes.md#editing-patches-in-series "mention") if you need to modify patch-set(one of the patch from multiple patches)

### Editing patches in series

1. If patch that's not the head commit (say patch 1 of 3), you can use the "interactive" mode of `git rebase`. Simply pass `git rebase` the `-i` flag, followed by the commit ID of the patch before the patch you want to edit.\
   `git rebase -i <commit ID of the patch which you want to edit>^`\
   ``Git will then pop up a window with a list of commits. Change the word "pick" to "edit" corresponding to commit you want to edit, and write and quit. Git will then rewind history to that commit, and you will be able to edit it with `git add` and `git commit --amend -v.`
2. Once you've made your changes, you should run:\
   `git rebase --continue`
3. Sometimes when you amend commits, you will run into conflicts. Git will place conflict markers ('<<<<' and '>>>>') in the effected files. You need to resolve those conflicts by editing the file, adding them with `git add` and then running `git rebase --continue`. Make sure to remove the "Conflicts" lines from your commits that git adds by default when there's a conflict.
4. You can always abort the rebase by running:\
   `git rebase --abort`
5. Example
   *   say we had this git log:

       ```
       40336d6 USB: xHCI: override bogus bulk wMaxPacketSize values
       eaadde4 xhci: fix list access before init
       1f35618 xhci-mem: init list heads at the beginning of init
       c1be5a5 Linux 3.9
       ```


   * If I needed to edit commit 1f35618, I would run:\
     `git rebase -i 1f35618^`
   *   Git will then pop up a window with a list of commits:

       ```
       pick 1f35618 xhci-mem: init list heads at the beginning of init
       pick eaadde4 xhci: fix list access before init
       pick 40336d6 USB: xHCI: override bogus bulk wMaxPacketSize values

       # Rebase c1be5a5..40336d6 onto c1be5a5
       #
       # Commands:
       #  p, pick = use commit
       #  r, reword = use commit, but edit the commit message
       #  e, edit = use commit, but stop for amending
       #  s, squash = use commit, but meld into previous commit
       #  f, fixup = like "squash", but discard this commit's log message
       #  x, exec = run command (the rest of the line) using shell
       #
       # If you remove a line here THAT COMMIT WILL BE LOST.
       # However, if you remove everything, the rebase will be aborted.
       ```
   * We want to edit commit 1f35618, so we change the word "pick" to "edit", and write and quit. Git will then rewind history to that commit, and you will be able to edit it with `git add` and `git commit --amend -v`.
6. Refer [#commit-your-changes](code-review-and-submit-changes.md#commit-your-changes "mention")
7.  you need to version the patches that you re-send. A new version number lets reviewers know you made changes to the patch, and they should review it again.

    An example of what this would look like is:

    ```
    [PATCH] Foo: Fix these things
    ```

    And the updated versioning for a second revision:

    ```
    [PATCH v2] Foo: Fix these things better
    ```



    *   It's fairly simple to accomplish this, and there's certainly a few ways to do this. If you generate your patches using `git format-patch`, then it's simple to do this. Just add the --subject-prefix option like this:

        ```
        git format-patch --subject-prefix="PATCH v2"
        ```

        or whatever version you are currently on (3, 4, etc.).
    *   When you send a new version of your patch, add version history describing the changes made in the new version. The right place for the version history is after the "**---**" below the **Signed-off-by** tag and the start of the changed file list, as shown in the screenshot below. Everything between the **Signed-off-by** and the diff is just for the reviewers, and will not be included in the commit. Please don’t include version history in the commit log.

        ```
        Subject: [PATCH v2] Demonstrate that I can use git send-email

        ---

        This fixes the issues raised from the first patch.

        your-name | 1 +
        1 file changed, 1 insertion(+)

        ```
    *   This text gives the maintainers some extra context about your patch, but doesn't make it into the final git log.\
        \


        <figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
8. Here's a [good example of a patch](http://marc.info/?l=linux-usb\&m=139277576606875\&w=2) with a change log:
9. In case of more than 2 versions, make sure to include what has changed in each version below the -- so that there is a logical flow and the maintainers do not have to dig up previous versions. The most recently changed version should be described first followed by the subsequent changes. Have a look at this [patch example](https://lkml.org/lkml/2018/2/22/972) with 3 versions to get a better idea.

[^1]: 
