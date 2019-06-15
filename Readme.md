django-box
==========

Underlying ansible roles:
 `sa-include`,
 `sa-python`,
 `sa-python3`,
 `sa-nginx`,
 `sa-supervisord`,
 `sa-uwsgi`

Check roles documentation for whole set of parameters to override.


Supported tags:

`none`


Referencing as dependency using gilt (https://github.com/metacloud/gilt/,  `pip install python-gilt`)

```
# https://gilt.readthedocs.io/en/latest/
  - git: https://github.com/oops-to-devops/django-box.git
    version: master
    dst: deployment/provisioners/django-box/
    post_commands:
      - make
```


Supported parameters overrides:


```
django_database_user: "default_django_user"
django_database_password: "default_django_pass"
django_database_name: "{{ django_database_user }}"
database_host: "localhost"

# Parameters, that has no defaults and are omitted if not set
django_postgresql_listen_addresses: 127.0.0.1,
django_postgres_app_network: "192.168.0.1/32",
django_postgres_app_network_regex: "192\.168\.0\.1\/32",
django_postgres_dev_network: "192.168.0.1/32",
django_postgres_dev_network_regex: "192\.168\.0\.1\/32"
```


Using with vagrant boilerplate (https://github.com/Voronenko/devops-vagrant-ansible-boilerplate)

```ruby

    config.vm.provision "django-box", type: "ansible" do |ansible|
        ansible.playbook = "deployment/provisioners/django-box/box_django.yml"
        ansible.galaxy_role_file = "deployment/provisioners/django-box/requirements.yml"
        ansible.galaxy_roles_path = "deployment/provisioners/django-box/roles"
        ansible.verbose = true
        ansible.groups = {
            "django_box" => [vconfig['vagrant_machine_name']]
        }
        # ansible.tags = ["sa_django"]
        ansible.extra_vars = {
            box_provider: "vagrant",
            env: "vagrant"
        }
    end


```

Standalone play run can be configured by environment

```
export REMOTE_HOST=192.168.2.63
export REMOTE_USER_INITIAL=user
export REMOTE_PASSWORD_INITIAL=password
export BOX_DEPLOY_USER=jenkins
# if you use sudo
export BOX_DEPLOY_PASS=
```
