# home-assistantconfig

New deployment of Home Assistant Green
Predominantly uses the UI to configure integrations

## Studio Code Server
This app can be used to edit config files, its best to stop the service when its not required as it has issues with memory leaks over time.

This container has a mapped directory for `/root/.ssh` whih has an invalid symlink on subsequent starts, this will need to be fixed before any git operations can be performed using `./ssh_fix.sh`, unless a new version of this container is created with a correct fix.

# Links
https://dfederm.com/managing-home-assistant-config-from-github/#google_vignette