=========
CHANGELOG
=========

--------------------
version .4 - 5/4/16
--------------------

New Features
------------

- Live markdown tutorial in new editor
- Auto save guide text using HTML5 local storage
- Side-by-side markdown preview
- Optional scroll-sync between text and markdown preview panes
- Ability to add images to guides via standard file dialog
- Support for 301 redirects for guides (see :ref:`redirects file <redirects_file>`)
- Easier signup to Slack community via popup box on FAQ page

Bug Fixes
---------

- Links in editor preview open in new tabs
- Use proper HTTP status codes for redirects requiring authentication
- Properly escape characters in Table of Contents (see `issue <https://github.com/pluralsight/guides-cms/issues/29>`_
- Incorrect links to branched guides on main guide page
- Overlapping of table of contents with footer
- Do not show users' drafts on profile page unless logged in as user
- Prevent errors on redundant publish status changes
- Prevent making API calls for URLs that do not look like guides on guide page
- Issue losing list of branches when saving original article after branched
- Issue with /user/ returning articles of repo owner instead of error
- Making a commit with wrong user name by incorrectly reading user cache (see `commit <https://github.com/pluralsight/guides-cms/commit/495efee1149cc8d8731b218ef2a81c5787aa77b3>`_
- Maintain social share counts for po.st with new URL structure introduced in v.3

Changes
-------

- Changed editor from `Bootstrap Markdown editor <http://www.codingdrama.com/bootstrap-markdown/>`_ to `Ace <https://ace.c9.io/>`_
- Show published guides instead of error page when unable to find requested guide
- Improved caching of file listings for homepage and review pages
- Add better explanation of publish workflow after submitting a new guide
- Improve error message when creating duplicate guide with title/stack
- Removed redundant 'Edit guide' link in header on guide page
- Removed form to set featured article
- Use /author/<name>/ URL for authors instead of user, 301 redirect from /user/<name>

--------------------
version .3 - 11/3/16
--------------------

Bug Fixes
---------

- Fix bug with not checking for article existence on editor page
- Fix link for featured article after redesign
- Fix bug with file listing getting updated with publish status before it changed


--------------------
version .2 - 11/3/16
--------------------

Changes
-------

1. Three stage publish workflow
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Draft**

The initial stage where all guides start out in.  Guides in this stage are not
visible by anyone other than the original author. [1]

**All guides marked as unpublished will be moved to draft stage during the
upgrade process.**  Therefore, initially there will be no guides in the
in-review stage.

**In-review**

The second stage were guides go that are ready for community editing help.  Any
user can mark their guide as 'in-review' from dropdown at the bottom of the
guide page.

Guides should only be marked as 'in-review' when they are complete and ready
editing help.

**Please don't mark partially completed guides as in-review.** This will
necessarily waste community editors time reviewing guides that are not
completed.

Guides marked as 'in-review' will show up on the 'Review' page.

**Published**

The final stage for fully edited articles is published.  This is the stage
where the community editors have decided a guide is ready for the world to see.
Only community editors can move a guide into the published stage.

Published articles will be available on the homepage of the site.

2. Redesign of the content repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The content repository is currently a flat structure.  This means all the
guides are directly at the top level of the repository, which makes it
difficult to easily navigate on the github.com repository view.  This pull
request reorganizes the repository to use a more intuitive and nested layout
based on the publish status of the guide as well as the stack.  For example,
each publish stage will have a folder with a nested folder for each stack:

This will make quickly browsing the content much easier on github.com.

3. URL redesign (with backwards compatability)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The URL scheme has been redesigned to include the stack.  This gives visitors
more insight into the type of guide by looking only at the URL.

Therefore, the guide URL will now be something like:

- `/python/my-awesome-guide`

instead of

- `/my-awesome-guide`

All the old URLs with only the title remain intact with a 301 redirect at the
`/review/` endpoint.

Also, the status of a guide is represented by a query string, not directly in
the URL as before.  So, the following URL will point to a guide in the
in-review stage:

- `/python/my-awesome-guide?status=in-review`

instead of

- `/review/my-awesome-guide`

This will allow articles to keep the same URL through the entire publish
workflow, improving their SEO and link maintainability.  In addition, visitors
can clearly see in the URL the publish status of a guide.  Soon there will be a
more visual way to see the status on the guide page itself, but not in this
change.

Note that changing the stack of your article **will** change the URL of your
guide.  Therefore, change this with caution to avoid losing any SEO you might
have gathered on the old URL.  Typically you should not be changing your stack
after you're in the 'in-review' stage.

4. Github commits only involve guide author
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Previously all commits to guides were pushed to github with a different author
and committer.  The committer was marked as the owner of the content
repository.  This lead to a commit having a different author and committer,
which is confusing on github.com.  Now all commits will have the same committer
and author to avoid this confusion.  **You as the author still get full
contribution credit, which will show up on your github.com profile.** This
change just gives you commit credit **by youreself.**

5. Ability to change stack guide
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is not a recommended action because it will change a guides URL, which is
not ideal for SEO and link preservation.  However, it is now allowed.

Upgrading
^^^^^^^^^

See the upgrade_repo_layout_fromv.1.py script for details on the content
repository conversion process.  The upgrade script will use `git mv` to move
all guide diretories to their new locations thereby retaining the commit
history.

**All guides marked as unpublished will be moved to draft stage during the
upgrade process.**  Therefore, initially there will be no guides in the
in-review stage.

1. Run upgrade script on your content repository
2. Run merge_branches.py and use the branch you used from step 1 to merge with.
3. Push all remote branches to origin
4. Push your master branch to origin
5. Deploy new version of the CMS
6. Run `disqus redirect crawler <https://help.disqus.com/customer/en/portal/articles/912834-redirect-crawler>`_ to update URLs for all comments.

[1] We don't have strict privacy since the guides are also available on
github.com.  So, technically a draft guide can still be viewed directly on
github, but there will be no way for users to see draft guides directly on the
content website.

Bug Fixes
---------

- Improve commit messages when removing guides

--------------------
version .1 - 23/2/16
--------------------

Initial open source release during `<http://hacksummit.org>`_.
