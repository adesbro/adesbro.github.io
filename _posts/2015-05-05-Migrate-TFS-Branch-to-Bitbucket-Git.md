---
layout: post
title: Migrate a TFS Branch to a Bitbucket (Git) Repository
---

After using Team Foundation Server (TFS) for many years here at work and managing the VM it runs on myself, the decision has been made to move to Bitbucket. The reasons for this are many, some outside of my control, but the biggest advantage for me is I don't have to manage the VM. Yey.

### Tools required ###

There are two tools you need. The first is Git. If you already have a version of Git installed, ensure it is available from the command line. If not, [download Git from here](http://git-scm.com/downloads). 

The second tool is a program called **git-tfs**. From the help:

>**git-tfs** is a two-way bridge between TFS (Team Foundation Server) and git, similar to git-svn. It fetches TFS commits into a git repository, and lets you push your updates back to TFS.

The [git-tfs GitHub](https://github.com/git-tfs/git-tfs) page has a lot of good documentation on how to use it, but for this migration we only need to use a small bit of functionality. [Download git-tfs from here](https://github.com/git-tfs/git-tfs/releases).
 
Make sure that **git-tfs** and Git are both in your `PATH` environment variable so they are accessible from the command line.

### Migration steps ###

Create a new private Repository from the Bitbucket web portal, giving it a name, description and select Git as the repository type. For this example, I will call the Repository **Huckleberry**.

Open a command prompt and navigate to the folder where you want your local clone to live, creating it if necessary. **It must be empty.** For this example, this folder is `C:\Projects\Bitbucket\huckleberry`.

Run the command, adjusting the TFS location and source branch location as required:

`git tfs clone http://<tfs-host>:8080/tfs/DefaultCollection $/Huckleberry/Trunk . --with-branches --with-labels`

Note that you **must** specify an actual branch in TFS, otherwise it will not work. You can create one through Source Control Explorer if required.

The `--with-labels` options ensures that all version labels are brought across as Git Tags

If, like me, you want to adjust the folder structure you can by moving folders around from the command line. For example:

`git mv "./src/Subfolder/Subsubfolder/MyProject" ./src/`

Now you should have all the folders you want with a complete history from TFS in your repository folder.

Connect the repository folder to your Bitbucket repository created earlier:

`git remote add origin https://bitbucket.org/<account name>/huckleberry`

Push the complete history to BitBucket:

`git push --all origin`

For some reason tags did not get pushed with everything else, so I had to issue a separate command to do that: 

`git push --tags`

And you are done! I'd been putting this off for a while because I thought it was hard, but it turns out to be quite straightforward once you know what to do.

### Want more information? ###

I got a lot of useful information from the **git-tfs** help page, [Migrate TFS to Git](
https://github.com/git-tfs/git-tfs/blob/master/doc/usecases/migrate_tfs_to_git.md
), but I never managed to get the cleansing of the git-tfs metadata to work.