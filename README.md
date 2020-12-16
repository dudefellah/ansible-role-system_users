system_users
=========

This is a very simple role meant to make playbooks that manage users a little
bit cleaner by allowing you to avoid manually running the
[ansible.builtin.user](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html),
[ansible.buitin.groups](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/group_module.html)
and [ansible.posix.authorized_key](https://docs.ansible.com/ansible/latest/collections/ansible/posix/authorized_key_module.html)
modules yourself. Instead, you just need to pass word `system_users_users`,
`system_users_groups` and `system_users_authorized_keys` into each list
and this role will run those modules for you.

**WARNING**:

This role is pretty dumb and passes much of the role values to the appropriate
modules as dicts, which is something that Ansible doesn't like for
[security reasons](https://docs.ansible.com/ansible/devel/reference_appendices/faq.html#argsplat-unsafe).
For that reason, it is recommended that you disable
[INJECT_FACTS_AS_VARS](https://docs.ansible.com/ansible/devel/reference_appendices/config.html#inject-facts-as-vars)
in Ansible. The security benefit of disabling this value extends beyond just
this role as well.

Requirements
------------

None.

Role Variables
--------------

See [defaults/main.yml](defaults/main.yml) for the most up-to-date values.

They're repeated here for convenience, and since there's so few of them:

|Variable Name      |Description|Default Value|
|-------------------|-----------|-------------|
|`system_users_users`|This is a list of user dicts to be passed to [ansible.builtin.user](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html). See module documentation for more information. | `[]` |
|`system_users_groups`|This is a list of group dicts to be passed to [ansible.builtin.group](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/group_module.html). See module documentation for more information. | `[]` |
|`system_users_authorized_keys`|This is a list of group dicts to be passed to [ansible.posix.authorized_key](https://docs.ansible.com/ansible/latest/collections/ansible/posix/authorized_key_module.html). See module documentation for more information. | `[]` |
|`system_users_private_keys`|The private keys provided in this list will be added to the provided users' directories. Each entry in this list can contain the key(s) `['user', 'group', 'key', 'dest', 'overwrite']`. Further information on the use of each key is provided in [defaults/main.yml](defaults/main.yml) | `[]` |

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: dudefellah.system_users
           system_users_groups:
             - name: bob
               gid: 1001
           system_users_users:
             - name: bob
               group: bob
           system_users_authorized_keys:
             - name: bob
               key: "{{ lookup('file', 'my-key-path') }}"

License
-------

GPLv2+

Author Information
------------------

Dan - github.com/dudefellah
