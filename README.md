# git2rss

Perl script to generate an RSS file from a git repo's commit history.

There are probably dozens or hundreds of other scripts out there which do the same thing, but this one is mine.

## Using the script

I normally use this script in conjunction with "books" that I manage using repos copied from my [`mdbook-template`](https://github.com/kg4zow/mdbook-template/) repo, to create a `commits.xml` in the root directory of the rendered book, before the updated content is "published" to Github Pages, [Keybase Sites](https://book.keybase.io/sites), or to a traditional web server.

The script can be run with the name of a directory. If a directory is not specified, the current directory will be used.

The directory must be a "working copy" of the repo, with the branch whose commit history is being "published", checked out. In this branch, in the root of the repo, must be a `.git2rss` file which configures the script.

The script will run a "`git log`" command to get the commit history, and print the XML content to its output. If you redirect this output to a file ... there's your feed file.

### Config file

The script requires that the repo contain a file called "`.git2rss`" in the root of the repo, in the branch whose history you're "publishing". The file needs to contain a set of "key = value" lines. Comments (starting with "`#`") and blank lines are ignored.

Example:

```
# Per-repo settings to control what 'git2rss' does in this repo

title       = git2rss
link        = https://github.com/kg4zow/git2rss
description = The git2rss source code
image_url   = ""
show_email  = user
commit_url  = https://github.com/kg4zow/git2rss/commit/%s
limit       = 10
```

The items are:

* `title` (required) = The title of the feed.

* `link` (required) = The URL for the feed as a whole. Some RSS readers may offer a way to visit this link when viewing information about the feed itself.

* `description` (required) = A short description of the feed.

* `image_url` (optional) = An image to be used for the posts. If not supplied, some RSS readers may try to find an appropriate image on their own.

* `show_email` (optional) = "How much" of the author's email address to show.

    * `full` = Show the full email address, i.e. "`jms1@jms1.not`" (mis-spelled on purpose to avoid spam).

    * `user` = Only show the username, i.e. "`jms1`"

    * `none` = No email, only show the author's name. This is the default if `show_email` is not specified.

* `commit_url` (optional) = If specified, this will be used as the URL for each commit's "post" in the feed. The value should contain "`%s`" in it somewhere, this tag will have the actual commit hash substituted in it.

* `limit` (optional) = Maximum number of most recent commits to include in the generated RSS file.

    * Setting this to `0` will make the script include every commit in the repo's history.

    * If the repo has multiple branches, only the commits in the current branch will be shown.

Values *can* be enclosed with double-quotes (i.e. the `"` character), but do not have to be.

## Github

Github has almost the same functionality built into it, although the format of their feed isn't configurable.

* The Github user's avatar is used as *either* the feed image, or as the image for each *post* in the feed, not sure.

* They have a pre-configured limit on how many old commits are included.

* Their "posts" only contain the "body" of the commit message, without any other info (like the commit hash, author, date, or list of modified files).

Their URLs look like this:

&#x21D2; `https://github.com/OWNER/REPO/commits/BRANCH.atom`

For example, *this* repo's automatic feed would be ...

&#x21D2; [`https://github.com/kg4zow/git2rss/commits/main.atom`](https://github.com/kg4zow/git2rss/commits/main.atom)


## Future plans

(last updaed 2025-03-06)

The [RSS 2.0 Specification](https://www.rssboard.org/rss-specification) says that the feed's `<link>` element is required, however the [RSS reader app](https://reeder.app/) that I normally use, doesn't appear to *do* anything with it. When I get some time I want to investigate what, if anything, other RSS readers do with the value, and whether it really *needs* to be there or not.
