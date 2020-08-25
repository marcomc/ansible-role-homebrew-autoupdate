# Homebrew Autoupdate ansible role

Ansible role to setup Hobrew to regularly autoupdated
It is idempotent and will only setup autoupdated if it is not setup already (unless it is forced to override the current setup)

Homebrew autoupdate is configured:
* installing [Dominyk Tiller](https://github.com/DomT4)'s [homebrew autoupate extension](https://github.com/DomT4/homebrew-autoupdate)
* confguring hombrew's `HOMEBREW_AUTO_UPDATE_SECS` environment variable

## How it works
A user owned LaunchAgent is installed. It will run `autoupdate` periodically. If enabled it can run `upgrade` and `cleanup`.

The `HOMEBREW_AUTO_UPDATE_SECS` will define a threshold in seconds that will prevent homebrew to update it's database if an update has already been run within the defined threshold.

## Requirements & Dependencies
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
target_user_id:                             "{{ ansible_user_id }}"  # user you want to target

user_launch_agents_dir:                     "/Users/{{ target_user_id }}/Library/LaunchAgents"
user_application_support_dir:               "/Users/{{ target_user_id }}/Library/Application Support/"

homebrew_auto_update_plist:                 "{{ user_launch_agents_dir }}/com.github.domt4.homebrew-autoupdate.plist" # file required by homebrew-autoupdate
homebrew_auto_update_updater:               "{{ user_application_support_dir }}/com.github.domt4.homebrew-autoupdate/updater" # file required by homebrew-autoupdate

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


### Target User: `target_user_id`
This role support a 'Target User', meaning that you can specify the username you want to configure the `hombrew autoupdate` LaunchAgent.


## Continuous integration

This role has (not yet) a travis basic test (for github) only.


## Troubleshooting & Known issues


## Copyright
Marco Massari Calderone (c) 2020

## License

MIT
