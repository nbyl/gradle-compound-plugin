---
version: ${version}
layout: guide
title: gradle-compound-plugin - User Guide - Version ${version}
---
## Introduction

This is a plugin for the popular [Gradle](https://gradle.org/) build tool. It enables you to integrate the orchestration management tool [docker-compose](https://docs.docker.com/compose/) into your build.gradle. Unlike other docker plugins, gradle-compound-plugin has some unique features:

* Rich domain language to integration docker-compose into your build
* Select docker environment using [docker-machine](https://docs.docker.com/machine)
* Download docker-compose if not installed

## Usage

### Plugin Activation

To activate the plugin, insert the following code into your `build.gradle`:

```
plugins {
  id "com.github.nbyl.compound" version "${version}"
}
```

Otherwise you can include plugin also tradionally:

```
buildscript {
    repositories {
        maven {
            url  "http://dl.bintray.com/nbyl/maven"
        }
    }

    dependencies {
        classpath("com.github.nbyl.gradle:gradle-compound-plugin:${version}")
    }
}

apply plugin: 'com.github.nbyl.compound'
```
Afterwards you can use the provided [tasks](#tasks) to activate `docker-compose`.

### Configuration

#### Download of docker-compose 

The plugin can optionally download docker-compose and use. Simple turn the download property on:
```
dockerCompose {
    download = true
}
```

### <a name="tasks"></a>Tasks

#### com.github.nbyl.gradle.compound.tasks.compose.Build

This task calls the <a href="https://docs.docker.com/compose/reference/build/">build command</a> to build all missing containers.

| Property | Default Value | Description |
| --- | --- | ---: |
| `composeFile` | | The location of the docker-compose file to use. If not set, it will use the default of docker-compose (`docker-compose.yml`). |
| `dockerMachineEnvironment` | | The docker-machine environment to select. See [Docker Machine Support](#docker-machine-support) for furhter details.|

#### com.github.nbyl.gradle.compound.tasks.compose.Down

This task calls the <a href="https://docs.docker.com/compose/reference/down/">build command</a> to remove all containers and networks.

| Property | Default Value | Description |
| --- | --- | ---: |
| `composeFile` | | The location of the docker-compose file to use. If not set, it will use the default of docker-compose (`docker-compose.yml`). |
| `dockerMachineEnvironment` | | The docker-machine environment to select. See [Docker Machine Support](#docker-machine-support) for furhter details.|

#### com.github.nbyl.gradle.compound.tasks.compose.Up

This task calls the <a href="https://docs.docker.com/compose/reference/up/">up command</a> to start all containers and networks.

| Property | Default Value | Description |
| --- | --- | ---: |
| `composeFile` | | The location of the docker-compose file to use. If not set, it will use the default of docker-compose (`docker-compose.yml`). |
| `dockerMachineEnvironment` | | The docker-machine environment to select. See [Docker Machine Support](#docker-machine-support) for furhter details.|

### <a name="docker-machine-support"></a>Docker-Machine Support

The plugin supports selection of the docker host using the provisioning tool docker-machine. If you define a task `start` using the follwing configuration:

```
task start(type: DockerComposeUp) {
    dockerMachineEnvironment 'dev'
}
```

The plugin will read the settings from the docker-machine configuration `dev` first and use it to connect to the given docker daemon. This way, you can keep the configuration of your docker hosts from the build description.

## Development

The code of the gradle-compound-plugin is avaible at [GitHub](https://github.com/nbyl/gradle-compound-plugin). The current state of the master branch can be visited at [Travis CI](https://travis-ci.org/nbyl/gradle-compound-plugin).

If you see any errors feel free to provide a patch. To do so, simply open a pull request at https://github.com/nbyl/gradle-compound-plugin/pulls. Afterwards Travis will build the code and if the build succeeds your pull request will be merged by the development team.

To create a release the following procedurs needs to be executed:

* Create a Tag and push it to github: `git tag '<version>' && git push --tags`
* Travis will build the release and deploy the artifacts to [bintray](https://bintray.com/nbyl/maven/gradle-compound-plugin)

## License

The plugin is provided under the [Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)
