[![Build Status](https://travis-ci.org/SonarSource/sonar-scanner-cli-docker.svg?branch=master)](https://travis-ci.org/SonarSource/sonar-scanner-cli-docker)

# SonarScanner CLI

This is the Git repository that contains source for [SonarScanner CLI](https://github.com/SonarSource/sonar-scanner-cli) Docker images.
Images are available on [Docker Hub](https://hub.docker.com/r/sonarsource/sonar-scanner-cli).

## Beta

This image is currently in Beta testing and is made available to gather feedback.

## License

Copyright 2015-2019 SonarSource.

Licensed under the [GNU Lesser General Public License, Version 3.0](http://www.gnu.org/licenses/lgpl.txt)

# How to run the Docker image

## On Linux

You can then run the following command:

```
docker run -e SONAR_HOST_URL=http://foo.acme:9000 --user="$(id -u):$(id -g)" -it -v "/path/to/project:/usr/src" sonarsource/sonar-scanner-cli
```

To analysis the project in the current directory:

```
docker run -e SONAR_HOST_URL=http://foo.acme:9000 --user="$(id -u):$(id -g)" -it -v "$PWD:/usr/src" sonarsource/sonar-scanner-cli
```

### Write permissions

The scanner writes to the analysed project directory, in directory `/path/to/project/.scannerwork`.

The `--user` option (see sample commands above) is used on Linux to have the scanner write with the same user as the one calling the `docker run` command.

## On Mac

The command is quite similar to the one for Linux except that you don't need to specify `--user` option.

To analyse the project located in `/path/to/project`, execute:

```
docker run -e SONAR_HOST_URL=http://foo.acme:9000 -it -v "/path/to/project:/usr/src" sonarsource/sonar-scanner-cli
```

To analyse the project in the current directory, execute:

```
docker run -e SONAR_HOST_URL=http://foo.acme:9000 -it -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli
```

## `.sonar` directory

The scanner downloads data from the SonarQube server it connects to. Retrieving this data can take time and certainly takes bandwidth. For efficiency, the scanner caches this data in a `.sonar` directory.

When running the scanner with this image, this `.sonar` directory is created in the project's directory. This implies caching is not happening accross analysis of multiple projects.

Caching is actually shared between projects when running the scanner natively as the `.sonar` is created in the home directory of the current user (eg. `/home/my_user/.sonar`).

You can reproduce this behavior, by adding the following to the `docker run` command:

```
-e SONAR_USER_HOME=/usr/.sonar -v "/home/my_user/.sonar:/usr/.sonar"
```

# Developer documentation

Developer documentation is available in [DEVELOPER.md](DEVELOPER.md).
