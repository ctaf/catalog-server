# Linux Server Configuration

This describes the configuration of a linux-based server hosting the mountain 
sports catalog, a flask web-application with OAuth2 authentication.


## Server data

IP (URL) | Port
----|-----
52.27.49.81 | 2200


## Documentation


### User management

- Created user grader who is the only one allowed to login via SSH. Granted
  permission to sudo.

### Upgrades

- Installed and configured package unattended-upgrades to automatically install
  security updates. Non-security updates are regularly checked by Icinga and
  have to be installed manually.

### Security

- Restricted access to apache root directory, disallowed .htaccess.
- Configured permission settings of apache directories: App directory and all
  subdirectories belong to group www-data. Absolutely no rights for others.
- Disabled apache modules for security reasons: negotiation, setenvif, status,
  autoindex.
- Restricted request size for upload directory of catalog app.
- Changed ssh port to 2200 and disallowed root-login.

### Monitoring

- Installed and configured fail2ban.
- Installed and configured icinga.
- Added icinga user to group adm to be able to read auth.log.
- Added a nagios plugin to check for failed ssh logins using check_logfile
- Added icinga service to check for http content
- Configured icinga to accept commands from web console.

### Apache

- Installed apache2 package, configured timeout in config.
- Created the appropriate directory structure for the catalog app under
  /var/www.
- Added virtual host file for catalog app and enabled site.
- Installed mod_wsgi module.
- Created a .wsgi script and configured the python load path, so apache can
  find and get the application object.

### Database

- Installed package postgresql.
- Added user postgres to system and created database 'catdb'.
- Made a sql dump from the sqlite database and made appropriate changes to be
  read by psql.
- Added user catalog with encrypted password 'catalog'.
- Granted login to user catalog in pg_hba.conf.
- Granted user catalog limited access to catalog database:
  - Allowed connecting to database.
  - Allowed selecting on sequences.
  - Allowed selecting, updating, inserting and deleting on tables.

### Catalog App

- Installed git and cloned repository sports-catalog.
- Installed python-pip and necessary python modules for the catalog app
  (flask, sqlalchemy, werkzeug, oauth2client, Jinja2, flask-wtf, psycopg2).
- Made appropriate changes to flask application to connect to postgresql
  database.
- Changed IDs on item and image to serial types.
- Added server IP to urls allowed for OAuth2.

### Firewall

- Configured to policy default deny (for incoming traffic). Outgoing traffic
  only allowed on ports 80 (web), 2200 (ssh), 123 (ntp) and 5666 (icinga).
- Enabled logging of ufw.

### Miscellaneous

- Installed ntp and set timezone to UTC.

### Personal modifications

- Added personal vimrc.
- Changed standard shell of root user to zsh.
- Installed oh-my-zsh, included personal zshrc.
- Added personal tmux configuration.
- Added banner to sshd configuration.


## Creator

**Philip Taferner**

* [Google+](https://plus.google.com/u/0/+PhilipTaferner/posts)
* [Github](https://github.com/ctaf)

[1]: https://www.python.org/download/releases/2.7
[2]: https://developers.google.com/appengine
[3]: https://console.developers.google.com
