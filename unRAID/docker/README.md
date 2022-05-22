# Ultimate Guide for unRAID templates (WIP)

Simple explanation for all the options when creating a xml template for Community Applications (the manual way).

&nbsp;

Get the Community Applications Plugin:
>[unRAID Community Applications](https://forums.unraid.net/topic/38582-plug-in-community-applications/)

References:

>[selfhosters unRAID templating guide](https://selfhosters.net/docker/templating/templating/)

&nbsp;

&nbsp;

## Base XML

---

```xml
<?xml version="1.0"?>
<Container version="2">
    <Name>CONTAINER NAME</Name>
    <Repository>REPOSITORY</Repository>
    <Registry>https://hub.docker.com/r/USER/CONTAINER/</Registry>
    <Network>bridge</Network>
    <Privileged>false</Privileged>
    <Support></Support>
    <Project></Project>
    <Overview></Overview>
    <WebUI></WebUI>
    <TemplateURL/>
    <Icon>/plugins/dynamix.docker.manager/images/question.png</Icon>
    <ExtraParams/>
    <PostArgs/>
    <DonateText/>
    <DonateLink/>
</Container>
```

&nbsp;

## Options

---

&nbsp;

&nbsp;

| Name | Example | Description |
| - | - | - |
| Name | idrac6 | The name for the container, preferably in lowercase.
| Repository | domistyle/idrac6 | The name of the image to pull from dockerHub (other repositories work. e.g. ghcr.io/CONTAINER_NAME lscr.io/CONTAINER_NAME)

&nbsp;

### Name

The name for the container, preferably in lowercase. e.g.

- `idrac6`

&nbsp;

### Repository

The name of the image to pull from dockerHub (other repositories work). e.g.

- `domistyle/idrac6`

&nbsp;

### Registry

Link to the dockerHub page for this container. e.g.

- `https://hub.docker.com/r/domistyle/idrac6/`

&nbsp;

### Network

Usually bridge if not specified by the image maintainer

- Options
  - `bridge`
  - `host`
  - `none`
  - `custom: br0`
  - `custom: DOCKER_NETWORK`

For custom docker networks look under neat tricks.

&nbsp;

### Privileged

Usally false if not specified by image maintainer or you know what your doing.

- `false`

&nbsp;

### Support

A link to a support thread on the unraid forums for the container

&nbsp;

### Project

Link to the GitHub page (or the homepage of the project) e.g.

- `https://github.com/DomiStyle/docker-idrac6`

&nbsp;

### Overview

Basic description of the project. e.g.

- `Allows access to the iDRAC 6 console without installing Java or messing with Java Web Start. Java is only run inside of the container and access is provided via web interface or directly with VNC.`

&nbsp;

### WebUI

Which container-port a webui might be on. e.g.

- `http://[IP]:[PORT:5800]`
  - Unraid will translate this string to the IP of the server, and the host-port set for container-port 5800

&nbsp;

### TemplateURL

Url to the template e.g.

- `https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/"container-name".xml`
  - and replace "container-name" with the actual name of the container (again, in lowercase).

&nbsp;

### Icon

URL to an icon, personally I prefer them in png. It has to be loaded over https. e.g.

- `https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/chevereto.png`
  - Chevereto icon

&nbsp;

### ExtraParams Parameters

Set with the docker run command. e.g.

- `--restart unless-stopped`

&nbsp;

### PostArgs

Command to run inside the container after start. e.g.

- `/bin/sh -c 'apk update && apk add ipmitool && telegraf'`

&nbsp;

### DonateText Text

To show with the donate button.

&nbsp;

### DonateLink

URL for donations.

&nbsp;

### DonateImg

URL to donation image.

&nbsp;

>:warning: GitHub link it has to be a raw link! e.g.  
<https://raw.githubusercontent.com/>

&nbsp;

## Full config syntax

---

```xml
<Config Name="" Target="" Default="" Mode="" Description="" Type="" Display="" Required="" Mask=""><Config/>
```

## Add a Volume

---

### Volume Target

The internal container path for the volume. Where the data you want to be persistent is located inside the container. This is usually recommended by the maintainer.

```xml
<Config Target="/INTERNAL_PATH"/>
```

- `/INTERNAL_PATH`
  - Replace with the internal path in the container. e.g.
    - `/config`
    - `/data`
    - `/var/log`
    - etc

&nbsp;

### Volume Mode

What permissions to use for the volume.

```xml
<Config Mode="OPTIONS">
```

- `OPTIONS`
  - `rw` - Read-Write
  - `ro` - Read-Only
  - Slave options also supported

&nbsp;

### Volume Type

The only option for "Type" is always `Path` for a volume.

```xml
<Config Type="Path"/>
```

&nbsp;

## Add a Variable

---

### Variable Target

The container variable specified by the container maintainer.

```xml
<Config Target="CONTAINER_VARIABLE">
```

- `CONTAINER_VARIABLE`
  - IDRAC_HOST

&nbsp;

### Variable Type

The only option for "Type" is always `Variable` for a enviroment variable.

```xml
<Config Type="Variable">
```

&nbsp;

## Add a Port

---

### Port Target

The container port.

```xml
<Config Target="PORT">
```

- `PORT`
  - Replace with internal port. e.g.
    - `80`
    - `443`
    - `8080`
    - etc

&nbsp;

### Port Type

The only option for "Type" is always `Port` for a port.

```xml
<Config Type="Port">
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Shared attributes

---

Name - The name that shows in the Unraid template manager. e.g.
    Appdata, PUID, WebUI

Description - A more detailed description on this Config. e.g
    Appdata location, PUID, WebUI

Default - Suggested value for the Config. e.g.
    /mnt/user/appdata/idrac, 99, 8080

Display - How the volume is shown to the user.
    always - Always show the volume, can be edited and deleted in basic view.
    always-hide - Always show the volume, can't be edited and deleted in basic view.
    advanced - Shows when the user presses "Show more settings ...", can be edited and deleted in basic view.
    advanced-hide - Shows when the user presses "Show more settings ...", can't be edited and deleted in basic view.

Required - If the user is able to continue without specifying the value.
    true - If the value is required to be entered.
    false - If the value is not rewuired to be entered.

Mask - If the value should be masked behind asterisks, only really usefull on variables.
    true - Hides what is entered.
    false - Shows what is entered in clear text.

Additional tags
Beta - Gives the application a warning in CA with the following text. "This application has been marked as being Beta. This does NOT neccessarily mean that there will be issues.." e.g.

```xml
<Beta>true</Beta> or <Beta>false</Beta>
```

Branch - Prompts the user to choose a dockerHub tag e.g.
    linuxserver/emby.xml

&nbsp;

## Neat tricks

---

### Template predefined values aka dropdowns

The template manager support setting a set of predefined values, often uses in conjunction with variables that expect bools.
Defined by separating the values with |.

1. Example

    ```xml
    <Config Default="true|false"/>
    ```

2. Example

    ```xml  
    <Config Default="on|off"/>  
    ```

>Has to be set in the `Default=""` attribute.  

&nbsp;

### Create a docker network

For unRAID you will need to use the terminal

```docker
docker network create DOCKER_NETWORK_NAME
```

Where DOCKER_NETWORK_NAME is the name you want for the network

- So you can name one `proxy` for all your reverse proxied apps so you can use the container name as a hostname instead of using an IP.
