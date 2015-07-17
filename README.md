# Running docker through an setuid wrapper

## Preamble

I've never been a big fan of `sudo`. I find it doing too much and hard
to configure. I prefer simple solutions. When I started using Debian,
almost 16 years ago I used
[super](http://www.ucolick.org/~will/RUE/super/README). I still like
its simplicity and control. Therefore usually when I need to run a
**privileged** command as a regular user I use `super`.

Therefore to run the docker CLI a bunch of aliases are created that
allow to use docker without `sudo` or as root.

This is what I propose everywhere I go and whenever I need to run
docker commands.

## Usage

 1. Install
    [`super`](http://www.ucolick.org/~will/RUE/super/README). In
    Debian is just:
    
        aptitude install super
 2. Define allowed users. Edit `/etc/super.tab` default it's assumed
    that all users that are allowed ro run docker commands are in the
    group `$docker_users`. For example:

        ## Users that have permission to use the commands
        :define docker_users perusio, luser, another_user

 3. Customize it to your liking, e.g., by defining special groups for
    specific commands. For example `$docker_cleaners` is the group of
    users that can remove running containers.

        ## Users that can remove running containers.
        :define docker_cleaners perusio, fooluser, another_foo_user

  4. Clone this repository:
     
        git clone https://github.com/perusio/docker-super.git

  5. Make the `docker.supertab` file be owned by **root**:
     
         chown root docker.supertab
      
  6. Include the `docker.supertab` in your `/etc/super.tab`:

        :include /path/to/docker.supertab
  7. Done.

To get information about all available commands **that you're allowed
to run** do `super -H`.

More information is available in
[`super(1)`](http://www.ucolick.org/~will/RUE/super/super.1.html) man
page. The `super.tab` format is in [`super.tab(5)`](http://www.ucolick.org/~will/RUE/super/super.5.html).

## TODO

Keep adding new commands: `rmi`, etc.
