# inventaris-dev

## deployment of dev environment for the 'inventaris' app

### build instructions

#### assumes
- access to OE private repos
- osx/linux environment
- docker installed
- python installed
- git
- your wildcards-private.ini files
- pycharm enterprise edition

#### general setup
```
# assumes you have access to OE private repos
git clone --recursive https://github.com/cecemel/inventaris-dev.git
cd inventaris-dev
# now, you have to add the development-private.ini and wildcards files on the right place,
ask your colleagues for help if you don't know (this will include storageprovider, inventaris,...)
```

#### building the frontend
```
TODO :-/ (in docker)
# manually
cd ./inventaris/inventaris/static
npm install; bower install
```

### building, migrating & init elastic, dummy data etc...
```
# assumes you are in folder inventaris-dev
docker-compose stop; docker-compose rm -f; #not required, but cleans your working environment
# assumes you are in inventaris-dev
python build_images.py [GITHUB_USER] [GITHUB_PASS];
python migrate_dbs.py;
python init_data.py;
```

#### reset backend one liner
```
docker-compose stop; docker-compose rm -f; sudo rm -rf data/*; python build_images.py [GITHUB_USER] [GITHUB_PASS]; python migrate_dbs.py; python init_data.py;
```

### running in pycharm
see e.g.
https://blog.jetbrains.com/pycharm/2017/03/docker-compose-getting-flask-up-and-running/

### running in emacs
On image doesn't fire on boot ->  inventaris-app
(added docker-compose.override.yml ->  command: tail -f /dev/null)
directly debugging and running tests in terminal

### rebuilding and running a dependent service
```
# e.g.
python build_images.py [GITHUB_USER] [GITHUB_PASS] storageprovider
```

### some git submodule tricks
- tutorial: https://git-scm.com/book/en/v2/Git-Tools-Submodules
- some good submodule SO: https://stackoverflow.com/questions/1030169/easy-way-to-pull-latest-of-all-git-submodules
- bringing *-dev repo up to data: git pull; git submodule update --init --recursive

### caveats-todos
- on slow networks, you'll might have to build a couple of times (2,3) again, because some scripts are not robust. needs fix
- if you modify the production.ini, you will have to build the image again
- docs need to be build automatically
- archive_feed is not build currently
- no documentgenerator
- currently, only dummy data is imported, if you want data from the db.dump -> you will need to fiddle a little with the docker files...
- if you get error HTTPError similar to: "401 Client Error: Unauthorized for url": restart your docker daemon
- scripts contain some hard coded parameters and should be cleaned
- currently, if you change code or config in dependent services, you will have to build and run every time again
- if you add a new dependent service and it needs a database, you will have to remove the postgres folder, and start the build from scratch
- working with private pypi server is still a hack, needs a fix
- a generic base image should be extract to speed up image build
- clean-up scripts, docker-compose should be sufficient for all migrations
- (linux issue) the redis data folder seems to be unknown user. THis has to do with user name space not properly mapped


### wildcards-private.yaml
For inventaris, the following should work