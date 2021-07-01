Redmine plugins
===============

The redmine-sameersbn Docker image allows us to put Redmine Plugins in this
folder, and they will be copied into the right place when the container starts
up.

redmine_merge_request_links
---------------------------

Due to wanting to use a PR that hasn't been merged yet, I'm pulling this plugin
from https://github.com/alexandermalfait/redmine_merge_request_links instead of
from the original author's github account. I've also added a few more tweaks to
make the plugin's content mesh better with the existing UI. If those continue to
bitrot then we shoudl make our own fork of the project to keep our
customizations in an easier to use form.

Unfortuntatly, this plugin also requires a tiny patch to the Redmine sourcetree
as well. Not sure yet how best to handle that, it probably depends on how our
production Redmine is set up and updated. Making a fork of Redmine could work
here too.

To install in redmine-sameersbn:

 * Clone the plugin repo from the URL above

 * Apply the appropriate patch in plugins/redmine_merge_request_links/patches/
   to the Redmine source tree. Note that this patch will need to be redone when
   the docker container is restarted as it is not saved in the persistent data
   folders.

        patch -p1 <  plugins/redmine_merge_request_links/patches/view_hook_issues_show_after_details_redmine_4.1.patch

 * Run something like this in the container to refresh the active plugins
   folder, and restart the docker application:

        source ${REDMINE_RUNTIME_ASSETS_DIR}/functions
        install_plugins
        supervisorctl restart unicorn

  * Don't forget to add the "View associated merge requests" permission to one
    or more roles, and enable the "Merge request links" module in the Redmine
    projects.

Set up the webhook in the GitLab projects as desribed in the README for the
redmine_merge_request_links project.
