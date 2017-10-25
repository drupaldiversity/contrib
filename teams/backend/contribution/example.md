## Step 0: Make sure you have an account on drupal.org with the confirmed user role

If you just made an account, you’ll need to get a “confirmed” user role before you can post issues and comments without a delay.

[Drupal User Role Guide](https://www.drupal.org/node/1887616)

Someone with the community user role can give other people confirmed user roles.

Note: this guide assumes that you have a Drupal development environment set up already. [You can find another guide for that elsewhere in our documentation](/teams/README.md).

## Step 1: How are you planning on contributing?
You have so many options when contributing to drupal.org. You can improve documentation, review issues in the issue queue, make patches for known issues, write tests for bugfixes, or report new issues that you’ve found.
* [Here are some ideas about contributing to Drupal](https://www.drupal.org/node/1424442).
* [Novice issues in particular a great place to start!](https://www.drupal.org/contributor-tasks/triage-novice-issues)

This example is going to be focused on finding an issue in a contrib module and creating a patch for it. I'm going to walk through a contribution I recently made to show a simple example of the general process.

While looking through the [examples module](https://www.drupal.org/project/examples), I discovered that [the queue_example had broken links in a comment in the queue_example.module file](http://cgit.drupalcode.org/examples/tree/queue_example/queue_example.module#n31-32).

```
L31 *  @link http://www.ent.iastate.edu/it/Batch_and_Queue.pdf Batch vs Queue Presentation slides by John VanDyk @endlink
L32 *  @link http://sf2010.drupal.org/conference/sessions/batch-vs-queue-api-smackdown session video. @endlink
```

Great! Now we have our contribution method. We're going to fix those broken links in the examples module.

## Step 2: Navigating the Issue Queue
Now we have our issue so we can move on to the next step. Before we create our issue, though, we need to make sure that another duplicate issue doesn't exist.
This step might not seem that big, but it's pretty important to remember. If you found a bug in a module,
someone else might have made a patch already or there may be an existing workaround.

You can find the issue queue for any module by looking on the module page. It will be in the right block:

![Issue queue location on drupal.org module page](/teams/backend/contribution/images/example_issue_queue.png)

If you're looking through the core issue queue, you can narrow down your search to apply to only the module/component you want [with the advanced search options](https://www.drupal.org/project/issues/search/drupal).
The issue queue filters are your best friend.

Let's look to see if someone else has found the broken links in the example module. I want to know if anyone's made an issue for broken links in the queue_example.
[I searched for broken links in the examples issue queue](https://www.drupal.org/project/issues/examples?text=broken+links&status=All&priorities=All&categories=All&version=All&component=All)
and didn't find any issues related to mine.
[I also searched for queue link](https://www.drupal.org/project/issues/examples?text=queue%20links&status=All&priorities=All&categories=All&version=All&component=All&order=last_comment_timestamp&sort=desc)
but didn't see anything either. At this point, it's probably safe to say that we can make our issue.

## Step 3: Creating Your Issue
Time to create an issue!

When creating an issue in the issue queue, it's important to remember that your issue will be looked at by other volunteers. Remember to be kind and patient in your issue.

Drupal.org has a handy guide for [determining issue queue priority](https://www.drupal.org/node/45111) right in the issue creation form.

Generally, most issues you create will start out as active. Since you need an issue number in your patch, you probably want to add it as a comment after you make the issue. [More details on issue status can be found on this page.](https://www.drupal.org/node/156119)

Onto the body! Generally, you want to be as verbose as possible to be helpful for other people reviewing your issue.
Try to explain the who, what, where, when, and why as well as you can.

The core issue creation form has a handy guide for making bug reports:
```
If you are reporting a bug, it needs to consist of three things:

What are the steps required to reproduce the bug?
What behavior were you expecting?
What happened instead?
Please include as much information as you can: OS, webserver name and version, PHP version, Drupal version, Drupal path, and everything else you might feel is relevant. There is no such thing as a bug report that is too detailed.

If you are submitting a feature request, please review the criteria for evaluating proposed changes.

Post general support requests in Drupal.org forum.
```
There's also another nice issue creation walkthrough [elsewhere on drupal.org](https://www.drupal.org/node/73179).

Now I think I'm ready to fill out the issue creation form:
![Issue creation form mockup](/teams/backend/contribution/images/angelam_issue_queue.png)

You can [see the issue I made here](https://www.drupal.org/node/2918544).

## Step 4: Patch It Up with the Command Line! (optional)
I put optional in the title because sometimes you don't have a patch for your bug or feature request and that's perfectly okay.

Generally, when making a patch, you want to grab the repository you want to change via git and then make a patch with git.

[This guide is so good for making a patch](https://www.drupal.org/node/707484)

I'll walk through what I did for my simple patch. First I cloned the examples module repository. You can find the ssh git location at the bottom of the module/core's drupal.org repository page.
![ssh git location of a drupal.org repo at the bottom of its repository page](/teams/backend/contribution/images/git_repo.png)

` git clone git://git.drupal.org/project/examples.git `

Next, we've got to see which branch we should be making the patch from. This matters because sometimes changes on the devleopment branch will conflict with the changes you want to make. You can also easily see this on the drupal.org repository page:
![branch drop-down at top of github repository page](/teams/backend/contribution/images/branch_list.png)
The examples module has no development branch so I'm going to stay on the default branch: 8.x-1.x
You can see this easily with a `git status`:
```
angelam$ git status
On branch 8.x-1.x
Your branch is up-to-date with 'origin/8.x-1.x'.
nothing to commit, working tree clean
```
If I needed to checkout a new branch, I'd use `git checkout [branchname]`. You can see the branch names in a drop-down in the upper right-hand corner.

Now, onto the fun part: let's make our changes! Open up your favorite text editor and make the changes to your code as you need.
When you're all done, use `git status` again to verify that you've changed the file you needed to change.
```
angelam$ git status
On branch 8.x-1.x
Your branch is up-to-date with 'origin/8.x-1.x'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   queue_example/queue_example.module

no changes added to commit (use "git add" and/or "git commit -a")
```
All right! Since we've made our issue, we can make a patch. You want your patch file to follow a specific naming convention: `[description]-[issue-number]-[comment-number].patch`
From the [drupal.org documentation](https://www.drupal.org/node/1319154):
```
The description can be the lowercased title of the issue, if it's descriptive enough. Replace any punctuation or spaces with hyphens.
The issue-number is the node ID of the issue (e.g. http://drupal.org/node/1266348 is issue-number
1266348).

The comment-number is the order number of the comment that the patch will be uploaded into, since there may be multiple iterations of a patch prior to the issue being fixed. Eg : description-1266348-5.patch where 1266248 is the issue number and the patch is attached to the fifth comment.
```
For mine, I ran `git diff queue_example/queue_example.module > queue_example_broken_links-2918544.patch`

## Step 5: Review
Now it's time to get your code reviewed.
