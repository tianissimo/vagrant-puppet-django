# Vagrant-Puppet-Django

This is a Vagrant Ubuntu (Precise32) box with a single Puppet manifests file (no modules) for Django development.

## What's included

Puppet provisions:

- Nginx
- uWSGI
- MySQL
- Python
- Virtualenv
- Django
- PIL dependencies to support jpg, zlib, freetype
- Git
- Vim

## Usage

- Edit configuration variables in the `config.yaml` file
- Run `vagrant up --provision`
- Follow the [First-Time Setup](#first-time-setup)

That's it! You are all set. Now, your Django app should be accessible from your host machine using [http://localhost:8080](http://localhost:8080) by default. Feel free to modify `Vagrantfile` to suit your needs.

## First-Time Setup

*Replace `<user>`, `<domain_name>` and `<project>` indicated below with the values configured in `config.yaml`*

### Install Django and Required Dependencies

After SSH into the VM, you can install Django and necessary dependencies for your app as follows:

1. Go to your project's virtualenv directory: `cd /home/<user>/virtualenvs/<domain_name>`
2. Run `source bin/activate` to activate virtualenv
3. Review and edit your `requirements.txt`
4. Run `pip install -r requirements.txt`

### Creating a New Django Project

1. Go to your project's virtualenv directory: `cd /home/<user>/virtualenvs/<domain_name>`
2. Run `source bin/activate` to activate virtualenv
3. Run `django-admin.py startproject <project>`

### Using an Existing Django Project

On the host machine, put your Django project folder in the same directory as `Vagrantfile`. Your Django project folder will be synced to `/home/<user>/virtualenvs/<domain_name>/<project>` on the VM.

## Configuring Django

### uWSGI

uWSGI is configured to load Django wsgi application from `/home/<user>/virtualenvs/<domain_name>/<project>/<project>/wsgi.py`

If you follow the [First-Time Setup](#first-time-setup), your Django app `wsgi.py` file is already at the correct path.

### Static and media files

Static and media files are served by nginx and you need to configure the paths in your Django `settings.py` to the correct directory.

```python
STATIC_ROOT = '/home/<user>/public_html/<domain_name>/static/'
MEDIA_ROOT = '/home/<user>/public_html/<domain_name>/media/'
```

## Note

You may want to:

- Run `git config --global` to set your `user.name` and `user.email`
- Edit `my.cnf` file to configure charset and collation to UTF8 as follows:

```ini
[client]
default-character-set = utf8

[mysqld]
character-set-server = utf8
collation-server = utf8_general_ci
```
