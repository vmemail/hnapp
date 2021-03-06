hnapp
=====

hnapp is a search engine for Hacker News that lets you subscribe to new search results via RSS or JSON. You would use it to keep in the loop about your favourite technologies, keep an eye on mentions of your product, filter out politics, or follow what your friends are talking about. See [hnapp.com](http://hnapp.com) for more examples.

This repository contains the source code for all of hnapp, including the website [hnapp.com](http://hnapp.com).


What's in the box
-----------------

- Mobile and web interfaces
- RSS and JSON feeds
- Full text search with stemming, as well as search constraints (e.g. score>10). For search syntax, see [hnapp.com](http://hnapp.com)
- Hacker News data is retrieved from the [official Firebase API](https://github.com/HackerNews/API).


Dependencies
------------

- ```python``` 2.7/2.8, ```python-dev```, ```virtualenv```, ```setuptools```
- ```postgresql```, ```postgresql-common```, ```libpq-dev```
- ```nodejs```, ```npm```, ```bower```
- Also see ```requirements.txt``` for pip


Web Browser Support
-------------------

- Enabling Javascript provides a faster GUI, but is not required
- Mobile version tested in Chrome and Safari on iOS 6+ and Chrome on Android 4
- Desktop version tested in Chrome, Firefox, Safari
- IE 9-11 should work fine, but was not tested
- IE <= 8 has some compatibility issues. Those browsers are out of the scope for this project


Environment
-----------

hnapp was tested in the following environment:
- Ubuntu 12.04 and 14.04
- ```nginx``` + ```uwsgi``` combo for production, or Flask's built-in web server for development
- All server timezones set to UTC


Installation
------------

- Install all dependencies. Install python stuff into a virtualenv.
- Create postgresql user and database
```bash
sudo su - postgres
psql -U postgres -c "CREATE USER hnapp WITH PASSWORD 'new_password'"
psql -U postgres postgres -f /srv/www/hnapp/sql/initial_schema.sql
psql -U postgres postgres -f /srv/www/hnapp/sql/migration_001.sql
psql
```
- Grant permissions to the new user (run this in psql)
```sql
\c hnapp
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES in schema public TO hnapp
INSERT INTO status (name, number) values ('last_item_id', XXXXXXX)
```
Where XXXXXXX is the id of the first HN item you want hnapp to download. Don't set it too far in the past, you can always download more items later.
- Install dependencies from bower.json
- Install python dependencies using pip if you haven't done so yet:
```bash
pip install -r requirements.txt
```
- Create a config file and edit it as per instructions within
```bash
cp config.sample.py config.py
nano config.py
```
- Set up a cron job to run ```vpython /srv/www/hnapp/cron.py every_1_min``` every minute (replace ```vpython``` with the path to the python binary in your virtual environment). You should redirect all output to a log file to log errors.
- Connect hnapp to a web server. The directory ```static``` must be webroot. hnapp was tested with ```nginx``` and ```uwsgi```. The uwsgi application is ```app``` in ```run.py```. For development purposes, you can use Flask's built-in web server by running ```vpython run.py```


License
--------------------------------

MIT License, see LICENSE.md


Contact
-------

For bug reports and feature requests, please open an issue on github.

Nikita Gazarov

[nikita@raquo.com](mailto:nikita@raquo.com)

[@raquo](http://twitter.com/raquo) [@hnapp](http://twitter.com/hnapp)

