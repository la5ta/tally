# Tally

This is the whole "tally" project with batteries included. This is the repo that contains all services(and repos) needed for the project.

## Pre-requisites

* [git](https://git-scm.com/downloads) >= 1.8.2
* [dockers](https://www.docker.com/community-edition)

## Installation

To install, you need to clone the repo with `recurse-submodules` to pull the submodules as well. Check each submodule

```bash
# git clone --recurse-submodules -j8 github.com/la5ta/tally.git
git clone --recursive https://github.com/la5ta/tally
cd tally
docker-compose up --build
```

You will end up with a directory structure like this:

```
┣ tally
┣━ docker-compose.yml
┗┳ front <= this is a pointer to tally-web
⬚┗━ Dockerfile
```

We now have the repo with submodules and a docker-compose with all(services) your app needs to run.

`docker-compose` will call each submodules' Dockerfile and build its service. On this project, it builds a container with a cady server(for https), web server(for the front app), postgres (the DB engine) and a hasura container(for graphql).

We are using custom domains so we need to update the hosts file to make use of this. Edit the `/etc/hosts` on Mac and `c:\windows\system32\drivers\etc\hosts` on Windows:

``` bash
127.0.0.1 tally-web.com tally-hasura.com tally-graphql.com
```

## Usage

`docker-compose up --build` builds the containers' images, if you don\'t have them. to avoid this -and everytime you work on the project- just run:

```bash
docker-compose up
```

We now have the site running with a front(`https://localhost:4000`) and the hasura service(`https://localhost:8080`) container.

## How to achieve this

To get submodules the way they are here, you need to follow these steps:

```bash
# Create the new folder
  mkdir -p ~/sites/tally
  cd ~/sites/tally


# Add the given repositories as a submodule at the given path
  git submodule add github.com/la5ta/tally-web.git tally-web
  # git submodule [--quiet] add [-b <branch>] [-f|--force] [--name <name>] [--reference <repository>] [--] <repository> [<path>]

# To update submodules to latest sub-repo's branch tip
  git submodule update --recursive --remote
```

------------

The correct mental model to have is that the parent project "tally" points to a very specific revision of the submodules. Thus, if you make changes to the submodules' repo, these changes will not be visible in "tally" unless you update the pointer accordingly.

Another key step here is that you must go into the submodule's directory and perform the git pull there, rather than in the root of the parent repo. Then you must return to the parent repo (i.e. be outside of the submodule) when you perform the commit and push.

------------

To remove a submodule, do the following:

```bash
git submodule rm [submodule's name]
```

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D
