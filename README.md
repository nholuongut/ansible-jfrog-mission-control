# Ansible role to install Jfrog Mission Control 2.0 to a Redhat 7.x linux host.
![](https://i.imgur.com/waxVImv.png)
### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
<br/>

## Usage

### Quickstart

If you are not using SELinux or using it in permissive mode there's absolute no variable needed to setup a basic, standalone setup. Just include it in a play:

```yaml
- name: install jfmc
  hosts: all
  become: true
  roles: stone-payments.jfrog-mission-control
```

### SELinux enforce mode

To run with SELinux enforcing mode add the `jfmc_selinux_tweaks` var as `true`.  
This role handle the necessary SELinux tweaks to run jfmc micro-services, but not for the mongodb.  
We recommend you to use an external mongodb installation that handles it. The stone-payments.mongodb does work well with SELinux enforcing mode

```yaml
- name: install jfmc
  hosts: all
  become: true
  vars:
    jfmc_selinux_tweaks: true
    jfmc_externalize_mongodb: true
    mongodb_admin_user: jfrog_insight
    mongodb_admin_password: password # Use ansible vault or other safe way to store the credentials.
    jfmc_mongodb_username: "{{mongodb_admin_user}}"
    jfmc_mongodb_password: "{{mongodb_admin_password}}"
  roles:
    - nholuong.mongodb
    - nholuong.jfrog-mission-control
```

### Other configs

I believe almost every other config is self-explanatory or directly related to a dependent service/feature. Simply override the configs on defaults/main.yml and they will be (hopefully) applied to your system.

#### Sensitive vars and credentials

I strongly recommend you to change the default value for these vars and keep it as secret with ansible-vault:

```yaml
jfmc_elastic_search_password: changeme
jfmc_mongodb_password: password
jfmc_postgres_root_user_pwd: postgres
jfmc_postgres_pwd: password
```

## Know issues

The postgres installation creates and try execute shell scripts at /tmp directory, if /tmp is mounted with no-exec you must remount it with exec permission.

Example play to run before jfmc installtion:

```yaml
- hosts: all
  become: true
  tasks:
  - mount:
      path: /tmp
      src: /dev/sr0
      fstype: xfs
      opts: exec
      state: mounted
```


# 🚀 I'm are always open to your feedback.  Please contact as bellow information:
### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com)

![](https://i.imgur.com/waxVImv.png)
![](Donate.png)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.🌟