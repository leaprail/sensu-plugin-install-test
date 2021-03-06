# sensu-plugin-install-test

## Goal
The goal of this is to help making it easier to help find dependency issues.

## How it works and how to use
It uses the ruby slim docker image as a base. Basically there is a very simple shell script to extract a list of packages that you would like to install (such as `build-essential`) and installs those packages before installing any rubygems that would be required and defined in your Gemfile which always must be defined.

### Prep Me
Basically we need to have a Gemfile to determine what we need to install. Here is an example of one I made while creating this:
```
$ cat app/Gemfile
source 'https://rubygems.org'

gem 'sensu-plugins-supervisor'
```

### Build Me
Simply run: `docker build .` there should be an image id that we will use in the next step

### Run Me
Here is where you can affect runtime, we use a simple ENV var of PACKAGES which should be 1 or more os packages that needs to be installed before installing gems. An example might look like this: `PACKAGES='build-essential,libexpat1-dev'`.

You now have everything you need to know to run this. An example might look like this: `docker run -e PACKAGES='build-essential,libexpat1-dev' d03f294ac7b9 ./install_packages.sh`.

### Ad-hoc Me
You can use the `-it` docker options and specify a shell so you can mess with anything you want to your hearts content.

Example:
```
docker run -it -e PACKAGES='build-essential,libexpat1-dev' d03f294ac7b9 /bin/bash
```

## Ruby-slim packages
It does include some packages that are installed by default. If you are having issues locally but not in this image you can always try installing them one by one. These change regularly so please always refer to the master branch: https://github.com/docker-library/ruby/blob/master/2.3/slim/Dockerfile There is indeed a c compiler installed but later removed so it might be hard to validate the need for a c complier: https://github.com/docker-library/ruby/blob/09c6a1602c111cf0cc83bf1df3186a85314ce398/2.3/slim/Dockerfile#L35.
