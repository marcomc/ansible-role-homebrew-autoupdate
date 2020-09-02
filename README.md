# Homebrew Autoupdate ansible role
Ansible role to setup Hobrew to regularly autoupdated
It is idempotent and will only setup autoupdated if it is not setup already (unless it is forced to override the current setup)

Homebrew autoupdate is configured:
* installing [Dominyk Tiller](https://github.com/DomT4)'s [homebrew autoupate extension](https://github.com/DomT4/homebrew-autoupdate)
* confguring hombrew's `HOMEBREW_AUTO_UPDATE_SECS` environment variable

## How it works
A user owned LaunchAgent is installed. It will run `autoupdate` periodically. If enabled it can run `upgrade` and `cleanup`.

The `HOMEBREW_AUTO_UPDATE_SECS` will define a threshold in seconds that will prevent homebrew to update it's database if an update has already been run within the defined threshold.

## (Soft) Requirements & Dependencies
* [Jeff Geerling](https://github.com/geerlingguy)'s' [geerlingguy.homebrew](https://github.com/geerlingguy/ansible-role-homebrew) which is defined as Ansible Galaxy dependency
* [Julien Blanchard](https://github.com/julienXX)'s [terminal-notified](https://github.com/julienXX/terminal-notifier) which is automatically installed from hombrew during the role's execution (if not already present)

### Ansible
It was tested on the following versions:
 * 2.9

### Operating systems
Target MacOS 10.15 possibly earlier versions too (not yet tested)

## Example Playbook
Just include this role in your list.
For example

```
- host: all
  roles:
    - marcomc.hombrew-autoupate
```

## Variables
See `test/integration/default/default.yml` for examples of applications installation.

```
homebrew_user:                             "{{ ansible_user }}"  # user you want to target
homebrew_auto_update_for_all_shell_types:   no #  no: apply the settings only to the user's default shellcheck
                                               # yes: apply the settings to all the shell supported: bash, zsh and fish
homebrew_user_launch_agents_dir:            "/Users/{{ homebrew_user }}/Library/LaunchAgents"
homebrew_user_application_support_dir:      "/Users/{{ homebrew_user }}/Library/Application Support/"
homebrew_user_default_shell                 "" # if not set it will be autodetected

homebrew_auto_update_plist:                 "{{ homebrew_user_launch_agents_dir }}/com.github.domt4.homebrew-autoupdate.plist" # file required by homebrew-autoupdate
homebrew_auto_update_updater:               "{{ homebrew_user_application_support_dir }}/com.github.domt4.homebrew-autoupdate/updater" # file required by homebrew-autoupdate

homebrew_auto_update_tap:                   'domt4/autoupdate'       # `homebrew-autoupdate` homebrew tap
homebrew_auto_update_dependencies:           [ 'terminal-notifier' ] # `homebrew-autoupdate` homebrew dependency
homebrew_auto_update_override_config:        no                      # override any existing config
homebrew_auto_update_interval:               43200                   # expressed in seconds (i.e. 43200 = 12 hours)
homebrew_auto_update_upgrade:                no                      # yes: uprgade all formulae and casks, no: update the homebrew database only
homebrew_auto_update_cleanup:                no                      # also run cleanup after running the update and/or upgrade
homebrew_auto_update_enable_notification:    no                      # Enable terminal notifications
homebrew_auto_update_tollerance:             300                     # Do not update homebrew database if last run was less then the define tolerance ins seconds
```
## Notes on Veriables

###  Homebrew User: `homebrew_user`
You can specify the username you want to configure the `hombrew autoupdate` LaunchAgent (defaults to the user that is running ansible).

## Continuous integration
This role has (not yet) a travis basic test (for github) only.

## Troubleshooting & Known issues

## Copyright
Marco Massari Calderone (c) 2020

## License

MIT
