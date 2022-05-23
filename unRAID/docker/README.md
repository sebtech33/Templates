# Ultimate Guide for unRAID docker templates (WIP)

Simple explanation for all the options when creating a xml template for Community Applications (the manual way).

&nbsp;

<details>
<summary> Great to remember Markdown things </summary>

Anchor  
`[Create a docker network](#create-a-docker-network)`  
Like this: [Create a docker network](#create-a-docker-network)

New line  
`&nbsp;`

Dropdown

<details>
<summary> This is the name for the dropdown </summary>
What should be inside the dropdown goes here.

```dropdown
<details>
<summary> This is the name for the dropdown </summary>
What should be inside the dropdown goes here.
</details>
```

</details>

Block  
Use one \` at either side of what you want inside the block  
Like this:  
\`This is a block\`  
`This is a block`

Code block

```xml code language
This wil have code highligt and language support.
Like this:

<Code Block="true"/>
```

Table with Code block + highligh and language support.
Has to be written in RAW HTML sadly.

<table>
<tr>
  <td> Code </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  ```xml
  <Xml Code_highligh="true"/>
  ```

  </td>
  <td> A description of the code. </td>
</tr>

</table>

</details>

&nbsp;

&nbsp;

&nbsp;
This is not needed but recommended.

> [How to get the unRAID Community Applications plugin](https://forums.unraid.net/topic/38582-plug-in-community-applications/)  
> [Check out the unRAID Community Apps web version](https://unraid.net/community/apps)

&nbsp;

## Template example

All examples you see is based on this template:

<details><summary>Complete template for Gluetun minimal</summary>

```xml
<?xml version="1.0"?>
<Container version="2">
  <Name>gluetun-min</Name>
  <Repository>qmcgaw/gluetun</Repository>
  <Registry>https://hub.docker.com/r/qmcgaw/gluetun</Registry>
  <Network>bridge</Network>
  <Shell>sh</Shell>
  <Privileged>false</Privileged>
  <Project>https://github.com/qdm12/gluetun</Project>
  <Overview>Lightweight swiss-knife-like VPN client to tunnel many different VPN Providers to VPN servers using Go, OpenVPN or Wireguard, iptables, DNS over TLS, ShadowSocks and an HTTP proxy&#xD;
&#xD;
Usage:&#xD;
Add "--network=container:gluetun-vpn-client" to a docker container's "Extra Parameters"&#xD;
(If you change the container name be sure to change "--network=container:GLUETUN-CONTAINER-NAME")&#xD;
And set "Network Type" to "None&#xD;
Then add that container WebUI port to gluetun
&#xD;
Features:&#xD;
- Based on Alpine 3.15 for a small Docker image of 29MB&#xD;
- Supports: Cyberghost, ExpressVPN, FastestVPN, HideMyAss, IPVanish, IVPN, Mullvad, NordVPN, Perfect Privacy, Privado, Private Internet Access, PrivateVPN, ProtonVPN, PureVPN, Surfshark, TorGuard, VPNUnlimited, Vyprvpn, WeVPN, Windscribe servers&#xD;
- Supports OpenVPN for all providers listed&#xD;
- Supports Wireguard both kernelspace and userspace&#xD;
    - For Mullvad, Ivpn and Windscribe&#xD;
    - For Torguard, VPN Unlimited and WeVPN using the custom provider&#xD;
    - For custom Wireguard configurations using the custom provider&#xD;
    - More in progress, see #134&#xD;
- DNS over TLS baked in with service provider(s) of your choice&#xD;
- DNS fine blocking of malicious/ads/surveillance hostnames and IP addresses, with live update every 24 hours&#xD;
- Choose the vpn network protocol, udp or tcp&#xD;
- Built in firewall kill switch to allow traffic only with needed the VPN servers and LAN devices&#xD;
- Built in Shadowsocks proxy (protocol based on SOCKS5 with an encryption layer, tunnels TCP+UDP)&#xD;
- Built in HTTP proxy (tunnels HTTP and HTTPS through TCP)&#xD;
- Connect other containers to it&#xD;
- Connect LAN devices to it&#xD;
- Compatible with amd64, i686 (32 bit), ARM 64 bit, ARM 32 bit v6 and v7, and even ppc64le &#x1F386;&#xD;
- Custom VPN server side port forwarding for Private Internet Access&#xD;
- Possibility of split horizon DNS by selecting multiple DNS over TLS providers&#xD;
- Unbound subprogram drops root privileges once launched&#xD;
- Can work as a Kubernetes sidecar container, thanks @rorph</Overview>
  <Category>Security: Network:VPN Network:Privacy Status:Stable</Category>
  <Icon>https://raw.githubusercontent.com/qdm12/gluetun/master/doc/logo_256.png</Icon>
  <ExtraParams>--cap-add=NET_ADMIN --restart unless-stopped</ExtraParams>
  <DateInstalled>1653012536</DateInstalled>
  <DonateText>Consider donating to qmcgaw!</DonateText>
  <DonateLink>https://www.paypal.me/qmcgaw</DonateLink>

  <!--[PORTS]-->
  <Config Name="HTTP_PROXY" Target="8888" Default="" Mode="tcp" Description="HTTP Proxy (Default:8888)" Type="Port" Display="always" Required="true" Mask="false">8888</Config>
  <Config Name="SHADOWSOCKS_TCP" Target="8388" Default="" Mode="tcp" Description="Shadowsocks TCP (Default:8388)" Type="Port" Display="always" Required="true" Mask="false">8388</Config>
  <Config Name="SHADOWSOCKS_UDP" Target="8388" Default="" Mode="udp" Description="Shadowsocks UDP (Default:8388)" Type="Port" Display="always" Required="true" Mask="false">8388</Config>

  <!--[ALWAYS MENU]-->
  <Config Name="VPN_SERVICE_PROVIDER" Target="VPN_SERVICE_PROVIDER" Default="private internet access|cyberghost|expressvpn|fastestvpn|hidemyass|ipvanish|ivpn|mullvad|nordvpn|perfect privacy|privado|privatevpn|protonvpn|purevpn|surfshark|torguard|vpn unlimited|vyprvpn|wevpn|windscribe|custom" Mode="" Description="VPN Service Provider" Type="Variable" Display="always" Required="true" Mask="false"></Config>
  <Config Name="VPN_TYPE" Target="VPN_TYPE" Default="openvpn|wireguard" Mode="" Description="Choose OpenVPN or Wireguard" Type="Variable" Display="always" Required="true" Mask="false">openvpn</Config>
  <Config Name="OPENVPN_USER" Target="OPENVPN_USER" Default="" Mode="" Description="OpenVPN Username" Type="Variable" Display="always" Required="true" Mask="false"></Config>
  <Config Name="OPENVPN_PASSWORD" Target="OPENVPN_PASSWORD" Default="" Mode="" Description="OpenVPN Password" Type="Variable" Display="always" Required="true" Mask="true"></Config>

<!--[ADVANCED MENU]-->
  <Config Name="APPDATA" Target="/gluetun" Default="/mnt/user/appdata/gluetun-min" Mode="rw" Description="Appdata to store persistent data" Type="Path" Display="advanced" Required="true" Mask="false">/mnt/user/appdata/gluetun-min</Config>
  <Config Name="TIMEZONE" Target="TZ" Default="" Mode="" Description="Timezone for correct time in logs" Type="Variable" Display="advanced" Required="false" Mask="false"></Config>

</Container>
```

</details>

&nbsp;

## Base XML

XML base code you can copy and edit for your usecase, but you will need to manually add Ports/Variables/Paths to your xml file.

<details open>
<summary> Base XML code </summary>

```xml
<?xml version="1.0"?>
<Container version="2">
  <Name>        CONTAINER NAME </Name>
  <Repository>  REPOSITORY/CONTAINER </Repository>
  <Registry>    https://hub.docker.com/r/REPOSITORY/CONTAINER/ </Registry>
  <Network>     NETWORK_MODE </Network>
  <Privileged>  PRIVILEGED </Privileged>
  <Support>     SUPPORT_PAGE </Support>
  <Project>     PROJECT_PAGE </Project>
  <Overview>    OVERVIEW_TEXT </Overview>
  <WebUI>       WEBUI </WebUI>
  <TemplateURL> TEMPLATE_URL </TemplateURL>
  <Icon>        ICON </Icon>
  <ExtraParams> EXTRA_PARAMS </ExtraParams>
  <PostArgs>    POST_ARGS </PostArgs>
  <DonateText>  DONATE_TEXT </DonateText>
  <DonateLink>  DONATE_LINK </DonateLink>
  <DonateImg>   DONATE_IMG </DonateImg>
</Container>
```

</details>

Explaination for everything in UPPERCASE can be found [here](#adding-config).

&nbsp;

## Template Options

### Template Name

The name for the container that will show in the docker tab in unRAID (or in CA if you decide to publish), preferably in lowercase and the original name of the container.

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `CONTAINER_NAME`

  </td>
  <td>
  
  ```xml
  <Name>CONTAINER_NAME<Name/>
  ```

  </td>
  <td> Where CONTAINER_NAME is changed with the name that you want for the container. </td>
</tr>
</table>

<details>
<summary>Example</summary>

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
</tr>
<tr>
  <td>
  
  `gluetun`

  </td>
  <td>
  
  ```xml
  <Name>gluetun<Name/>
  ```

  </td>
</tr>
</table>

</details>

&nbsp;

### Template Repository

The repository the image to pull from dockerHub (or other repositories).

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `REPOSITORY/CONTAINER`

  </td>
  <td>
  
  ```xml
  <Repository>REPOSITORY/CONTAINER<Repository/>
  ```

  </td>
  <td> Where REPOSITORY/CONTAINER is changed with the repository for the container. </td>
</tr>
</table>

<details>
<summary>Example</summary>

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
</tr>
<tr>
  <td>
  
  `qmcgaw/gluetun`

  </td>
  <td>
  
  ```xml
  <Repository>qmcgaw/gluetun<Repository/>
  ```

  </td>
</tr>
</table>

</details>

&nbsp;

### Template Registry

Link to the dockerHub page for this container. e.g.

- `https://hub.docker.com/r/domistyle/idrac6/`

&nbsp;

### Template Network

Usually bridge if not specified by the image maintainer

- Options
  - `bridge`
  - `host`
  - `none`

- Custom networks can also be created.
  - `custom: br0`
  - `custom: wg0`
  - `custom: DOCKER_NETWORK`

For custom docker networks go to [Create a docker network](#create-a-docker-network).

&nbsp;

### Template Privileged

> :warning: Be carefull with this!

Usally false if not specified otherwise by image maintainer or you know what your doing.

Options [default = `false`]

- `true`
- `false`

&nbsp;

### Template Support

A link to a support thread on the unraid forums for the container

&nbsp;

### Template Project

Link to the GitHub page (or the homepage of the project) e.g.

- `https://github.com/DomiStyle/docker-idrac6`

&nbsp;

### Template Overview

Basic description of the project. e.g.

- `Allows access to the iDRAC 6 console without installing Java or messing with Java Web Start. Java is only run inside of the container and access is provided via web interface or directly with VNC.`

&nbsp;

### Template WebUI

Which container-port a webui might be on. e.g.

- `http://[IP]:[PORT:80]`
- `https://[IP]:[PORT:443]`
  - Unraid will translate this string to the IP of the server, and the host-port set for container-port 5800

&nbsp;

### Template TemplateURL

Url to the template e.g.

- `https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/"container-name".xml`
  - and replace "container-name" with the actual name of the container (again, in lowercase).

&nbsp;

### Template Icon

URL to an icon, personally I prefer them in png. It has to be loaded over https. e.g.

- Default Icon - `/plugins/dynamix.docker.manager/images/question.png`
  - Chevereto icon

&nbsp;

### Template ExtraParams Parameters

Set with the docker run command. e.g.

- `--restart unless-stopped`

&nbsp;

### Template PostArgs

Command to run inside the container after start. e.g.

- `/bin/sh -c 'apk update && apk add ipmitool && telegraf'`

&nbsp;

### Template DonateText Text

To show with the donate button.

&nbsp;

### Template DonateLink

URL for donations.

&nbsp;

### Template DonateImg

URL to donation image.

&nbsp;

&nbsp;

## Adding Config

Here are more information regarding the configuration of Ports/Variables/Paths.

### Full Config syntax

This line can be copied to the bottom of your base xml. This has all the options you will need. No need to remove those you dont use, just leave them empty.

```xml
<Config Name="" Target="" Default="" Mode="" Description="" Type="" Display="" Required="" Mask=""><Config/>
```

Be sure to place it before the

```xml
</Container>
```

<details>
<summary> Example </summary>

```xml
<Config Name="" Target="" Default="" Mode="" Description="" Type="" Display="" Required="" Mask=""><Config/>
</Container>
```

</details>

## Add a Volume

### Volume Target

The internal container path for the volume. Where the data you want to be persistent is located inside the container. This is usually recommended by the maintainer.

<details>
<summary>XML Example</summary>

XML

```xml
<Config Target="/INTERNAL_PATH"/>
```

</details>

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
<Config Mode="OPTIONS"/>
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

### Variable Target

The container variable specified by the container maintainer.

```xml
<Config Target="CONTAINER_VARIABLE"/>
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

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Shared Config attributes

### Name

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `NAME`

  </td>
  <td>
  
  ```xml
  <Config Name="NAME"/>
  ```

  </td>
  <td> Where NAME is change with a short explaination of what that Port/Volume/Variable does (Examples of this under). This is the name that shows on the left side in the unRAID template manager. </td>
</tr>
</table>

<details>
<summary> Examples for NAME </summary>
TODO add image of unRAID to show this

- WEBUI
- APPDATA
- PUID

</details>

### Description

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `DESCRIPTION`

  </td>
  <td>
  
  ```xml
  <Config Description="DESCRIPTION"/>
  ```

  </td>
  <td> Where DESCRIPTION is changed with a longer explaination of what that Port/Volume/Variable does (Examples of this under). This is the name that shows under the Port/Volume/Variable in the unRAID template manager. </td>
</tr>
</table>

<details>
<summary> Examples for NAME </summary>
TODO add image of unRAID to show this

- WebUI is the port to access the website
- Appdata to store persistent data
- PUID

</details>

Description - A more detailed description on this Config. e.g
    Appdata location, PUID, WebUI

Default - Suggested value for the Config. e.g.
    /mnt/user/appdata/idrac, 99, 8080

&nbsp;

&nbsp;

&nbsp;

&nbsp;

### Display

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>

<tr>
  <td>

  `always`

  </td>

  <td>

  ```xml
  <Config Display="always"/>
  ```

  </td>
  <td> Always show the Volume/Port/Variable, can be edited or deleted in basic view. </td>
<tr>
  <td>
  
  `always-hide`

  </td>
  <td>

  ```xml
  <Config Display="always-hide"/>
  ```

  </td>
  <td> Always show the Volume/Port/Variable, can't be edited or deleted in basic view. </td>
</tr>

<tr>
  <td>

   `advanced`

  </td>
  <td>

  ```xml
  <Config Display="advanced"/>
  ```

  </td>
  <td> Shows when the user presses "Show more settings ...", can be edited or deleted in basic view. </td>
</tr>

<tr>
  <td>

   `advanced-hide`

  </td>
  <td>

  ```xml
  <Config Display="advanced-hide"/>
  ```

  </td>
  <td> Shows when the user presses "Show more settings ...", can't be edited or deleted in basic view. </td>
</tr>
</table>

### Required

This just uses a bool to make the field required or not.

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `true`

  </td>
  <td>
  
  ```xml
  <Config Required="true"/>
  ```

  </td>
  <td> Value is required to be entered. </td>
</tr>
<tr>
  <td>
  
  `false`

  </td>
  <td>
  
  ```xml
  <Config Required="false"/>
  ```

  </td>
  <td> Value is not required to be entered. </td>
</tr>
</table>

### Mask

Allows value that is entered in the template to be hidden. This is usefull to hide sensitive information like passwords.

TODO picture of unirad showing this

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `true`

  </td>
  <td>
  
  ```xml
  <Config Mask="true"/>
  ```

  </td>
  <td> Shows what is entered as a vaule. </td>
</tr>
<tr>
  <td>
  
  `false`

  </td>
  <td>
  
  ```xml
  <Config Mask="false"/>
  ```

  </td>
  <td> Hides what is entered as a vaule. </td>
</tr>
</table>

### Additional tags

Need more info on these.

Beta - Gives the application a warning in CA with the following text. "This application has been marked as being Beta. This does NOT neccessarily mean that there will be issues.." e.g.

```xml
<Beta>true</Beta> or <Beta>false</Beta>
```

Branch - Prompts the user to choose a dockerHub tag e.g.
    linuxserver/emby.xml

&nbsp;

&nbsp;

&nbsp;

## Neat tricks and tips

### Template predefined values aka dropdowns

The template manager support setting a set of predefined values, often uses in conjunction with variables that expect bools.
Defined by separating the values with "|".  
This is often used when there are multiple options for a variable.

> :information_source: Has to be set in the `Default=""` attribute.  

<details open>
<summary> Example </summary>

```xml
<Config Default="true|false"/>
```

```xml  
<Config Default="on|off"/>  
```

</details>

&nbsp;

### Add Tags to containers

If you wish to be on one spesific version or LTS of a container (assuming the container has tags).  

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
  <td> Description </td>
</tr>
<tr>
  <td>
  
  `TAG`

  </td>
  <td>
  
  ```xml
  <Name>CONTAINER_NAME<Name/>
  ```

  </td>
  <td> Where CONTAINER_NAME is changed with the name that you want for the container. </td>
</tr>
</table>

<details>
<summary>Example</summary>

<table>
<tr>
  <td> Option </td>
  <td> XML Code Example </td>
</tr>
<tr>
  <td>
  
  `gluetun`

  </td>
  <td>
  
  ```xml
  <Name>gluetun<Name/>
  ```

  </td>
</tr>
</table>

</details>

&nbsp;

### Comment in the XML

It is possible to add comments in the xml file.

You can do this  

```xml
<!-- YOUR COMMENT HERE -->
```

You can also do multiline comments like this

```xml
<!--  
YOUR COMMENT HERE
-->
```

&nbsp;

### Create a docker network

For unRAID you will need to use the terminal

```docker
docker network create DOCKER_NETWORK_NAME
```

Where DOCKER_NETWORK_NAME is the name you want for the network

> :information_source: DOCKER_NETWORK_NAME has to be in one name. It can't be separated by space, but you can user "-" or "_" to seperate words.

<details>
<summary>Examle code</summary>

> :information_source: This is just some examples for networks you can create.

Proxy Network:

```docker
docker network create proxy
```

Secure Network:

```docker
docker network create secure
```

Database Network:

```docker
docker network create database
```

</details>

So you can name one `proxy` for all your reverse proxied apps so you can use the container name as a hostname instead of using an IP.

> :information_source: By doing it this way you have to use the internal port and not the external port

&nbsp;

### Docker PUID, PGID and UMASK

#### PUID and PGID

> :information_source: Setting these settings wrong on unRAID will cause wrong permissions.

If you run `cat /etc/passwd | grep "nobody"` in your unRAID terminal you will get information like:

```bash
tower:~# cat /etc/passwd | grep "nobody"
nobody:x:99:100:nobody:/:/bin/false
```

This is the recommended user to use for docker on unraid

<details open>
<summary>So for the user nobody</summary>

- `PUID` is `99`
- `PGID` is `100`

</details>

See [PUID and PGID](#unraid-docker-puid-and-pgid)

#### UMASK

> :warning: Setting this incorrectly can be a security risk!

UMASK is used to set permissions for folders and files created by the container. 

Recommended:

- `UMASK` = `0022`  
This will

What is usually used:

- `UMASK` = `0000`

See [UMASK](#unraid-docker-umask) for more information

&nbsp;

## Referencess

### unRAID templating

> [unRAID Forums - Templating](https://forums.unraid.net/topic/101424-how-to-publish-docker-templates-to-community-applications-on-unraid/)
>
> [Selfhosters - unRAID templating guide](https://selfhosters.net/docker/templating/templating/)

### unRAID docker PUID and PGID

> [Linuxserver - Understanding PUID and PGID](https://docs.linuxserver.io/general/understanding-puid-and-pgid)
>
> [Reddit - unRAID PUID, PGID and UMASK](https://www.reddit.com/r/unRAID/comments/hl3s68/folder_permissions_docker_pgid_puid_umask/fxqq33h/)

### unRAID docker UMASK

> [unRAID Forums - UMASK](https://forums.unraid.net/topic/32278-docker-container-developer-best-practice-guidelines-for-unraid/page/3/)