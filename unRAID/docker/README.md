# Ultimate Guide for unRAID docker templates (WIP)

Simple explanation for all the options when creating a xml template for Community Applications (the manual way) pluss some tips and tricks.

I don't claim to know everything so if I have misunderstood or said anything wrong then please do enlighten me. I have done this as a documentation project for myself, but if this helps anybody im glad. I'll also try to leave references to stuff i find at the bottom to help those who want more information.

&nbsp;

This is not needed, but for reference on how I make this markdown file. It will be removed when I feel like im done with this file.

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

This is not needed but recommended.

> [How to get the unRAID Community Applications plugin](https://forums.unraid.net/topic/38582-plug-in-community-applications/)  
> [Check out the unRAID Community Apps web version](https://unraid.net/community/apps)

&nbsp;

## Template example

All examples you see is based on this template:

<details><summary> View the complete template for Gluetun minimal XML </summary>

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
  <!-- This is information for the container -->
  <Name> CONTAINER_NAME </Name>
  <Repository> REPOSITORY/CONTAINER </Repository>
  <Registry> https://hub.docker.com/r/REGISTRY/CONTAINER/ </Registry>
  <Network> NETWORK_MODE </Network>
  <Privileged> PRIVILEGED </Privileged>
  <Support> SUPPORT_PAGE </Support>
  <Project> PROJECT_PAGE </Project>
  <Overview> OVERVIEW_TEXT </Overview>
  <WebUI> WEBUI </WebUI>
  <TemplateURL> TEMPLATE_URL </TemplateURL>
  <Icon> ICON </Icon>
  <ExtraParams> EXTRA_PARAMS </ExtraParams>
  <PostArgs> POST_ARGS </PostArgs>
  <DonateText> DONATE_TEXT </DonateText>
  <DonateLink> DONATE_LINK </DonateLink>
  <DonateImg> DONATE_IMG </DonateImg>

  <!-- This is for adding ports/variables/paths -->
  <Config Name="" Target="" Default="" Mode="" Description="" Type="" Display="" Required="" Mask=""><Config/>
</Container>
```

</details>

Explaination for everything in UPPERCASE can be found [here](#template-options).

&nbsp;

## Template Options

> :information_source: All GitHub links need to be in a raw format.

### Template Container Name

The name for the container that will show in the docker tab in unRAID (or in CA if you decide to publish), preferably in lowercase and the original name of the container.

<table>
<tr>
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  ```xml
  <Name> CONTAINER_NAME <Name/>
  ```

  <td> Where CONTAINER_NAME is changed with the name that you want for the container.
</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> XML Code Example
<tr>
  <td>
  
  ```xml
  <Name> gluetun <Name/>
  ```

</table>

</details>

&nbsp;

### Template Repository/Container

The repository the image to pull from dockerHub (or other repositories).

<table>
<tr>
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  ```xml
  <Repository> REPOSITORY/CONTAINER <Repository/>
  ```

  <td> Where REPOSITORY/CONTAINER is changed with the repository for the container.
</table>

<details>
<summary> View example</summary>

<table>
<tr>
  <td> XML Code Example
<tr>
  <td>
  
  ```xml
  <Repository> qmcgaw/gluetun <Repository/>
  ```

</table>

</details>

&nbsp;

### Template Registry/Container

Link to the dockerHub page for this container.

<table>
<tr>
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  ```xml
  <Registry> REGISTRY <Registry/>
  ```

  <td> Where REGISTRY is changed with the url for the docker hub for the container.
</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> XML Code Example
<tr>
  <td>
  
  ```xml
  <Registry> https://hub.docker.com/r/qmcgaw/gluetun <Registry/>
  ```

</table>

</details>

&nbsp;

### Template Network Mode

Usually bridge if not specified by the image maintainer

For custom docker networks go to [Create a docker network](#create-a-docker-network).

<table>
<tr>
  <td> Option
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  `bridge`

  <td>
  
  ```xml
  <Network> bridge <Network/>
  ```

  <td> This is the default network mode, you usually don't need to change this unless maintainer recommends otherwise. Normal usage <br>
  http://[SERVER-IP]:[PORT] or <br>
  https://[SERVER-IP]:[PORT]
<tr>
  <td>
  
  `host`

  <td>
  
  ```xml
  <Network> host <Network/>
  ```

  <td> Personally i don't know what this does as of now.
<tr>
  <td>
  
  `none`

  <td>
  
  ```xml
  <Network> none <Network/>
  ```

  <td> Disable network for a container. Often used toghter with

  `--network=container:CONTAINER` in the [Extra Perameters](#template-extraparams). Where CONTAINER is the name of the container you want all the traffic going through. Often used for a container going through a vpn container.
<tr>
  <td>
  
  `custom: TYPE`

  <td>
  
  ```xml
  <Network> custom: TYPE <Network/>
  ```

  <td>
  More information

  [Create a Docker Network](#create-a-docker-network).
</table>

Custom networks can also be created.

- `custom: br0`
- `custom: wg0`
- `custom: DOCKER_NETWORK`

&nbsp;

### Template Privileged

> :warning: Be carefull with this!

Usally false if not specified otherwise by image maintainer or you know what your doing.

The `--privileged` flag gives all capabilities to the container, and it also lifts all the limitations enforced by the device cgroup controller. In other words, the container can then do almost everything that the host can do. This flag exists to allow special use-cases, like running Docker within Docker.

<table>
<tr>
  <td> Boolean
  <td> XML Code Example
  <td> Description
<tr>
  <td>

  `true` or `false`

  <td>
  
  ```xml
  <Privileged> false <Privileged/>
  ```

  <td>
  
  Default =
  `false`. <br>
  I don't know what this acually does as of now.
</table>

&nbsp;

### Template Support

A link to a support thread on the unraid forums for the container

<table>
<tr>
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  ```xml
  <Support> SUPPORT_LINK <Support/>
  ```

  <td> Where SUPPORT_LINK is a unRAID Forum post or GitHub issues page (or anywhere else where people can get help) for the template.
</table>

<details>
<summary> View example </summary>
<table>
<tr>
  <td> XML Code Example
<tr>
  <td>
  
  ```xml
  <Support> https://github.com/qdm12/gluetun/issues <Support/>
  ```

</table>
</details>

&nbsp;

### Template Project

Link to the GitHub page (or the homepage of the project).

<table>
<tr>
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  ```xml
  <Project> PROJECT_LINK <Project/>
  ```

  <td> Where SUPPORT_LINK is a unRAID Forum post or GitHub issues page (or anywhere else where people can get help) for the template.
</table>

<details>
<summary> View example </summary>
<table>
<tr>
  <td> XML Code Example
<tr>
  <td>
  
  ```xml
  <Project> https://github.com/qdm12/gluetun/ <Project/>
  ```

</table>
</details>

&nbsp;

### Template Overview

Description of the project.

> :information_source: Tip: Add `&#xD;` or `[br]` at the end to go to a new line.

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <Overview>
  Describe what&#xD;
  the container[br]
  is here
  </Overview>
  ```

  <td>
  Have a good explaination of the container here so others have an easy understanding of what this container does or is.

</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> RAW Overview
</tr>
<tr>
  <td>
  Lightweight swiss-knife-like VPN client to <br>
  tunnel to many different VPN Providers <br>
  or Custom VPN servers. <br>
  Using Go, <br>
  OpenVPN or Wireguard, <br>
  iptables, <br>
  DNS over TLS, <br>
  ShadowSocks, <br>
  an HTTP proxy with a Killswitch <br>
  <br>
  Supported VPN providers: <br>
  CyberGhost, ExpressVPN, FastestVPN, HideMyAss, IPVanish, <br>
  IVPN, Mullvad, NordVPN, Perfect Privacy, PrivadoVPN, <br>
  Private Internet Access, PrivateVPN, ProtonVPN, PureVPN, Surfshark, <br>
  TorGuard, VPN Unlimited, VyprVPN, WeVPN, Windscribe
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Overview>
  Lightweight swiss-knife-like VPN client to&#xD;
  tunnel to many different VPN Providers&#xD;
  or Custom VPN servers.&#xD;
  Using Go,&#xD;
  OpenVPN or Wireguard,&#xD;
  iptables,&#xD;
  DNS over TLS,&#xD;
  ShadowSocks,&#xD;
  an HTTP proxy with a Killswitch&#xD;
  &#xD;
  Supported VPN providers:&#xD;
  CyberGhost, ExpressVPN, FastestVPN, HideMyAss, IPVanish,&#xD;
  IVPN, Mullvad, NordVPN, Perfect Privacy, PrivadoVPN,&#xD;
  Private Internet Access, PrivateVPN, ProtonVPN, PureVPN, Surfshark,&#xD;
  TorGuard, VPN Unlimited, VyprVPN, WeVPN, Windscribe&#xD;
  </Overview>
  ```

</table>

</details>

&nbsp;

### Template WebUI

Which container-port a webui might be on. So when you left-click on the docker image in unRAID UI it will take you to the WebUI of your container.

> :information_source: Unraid will translate this string to the IP of the server, and the host-port set for container-port `8080` or `8443` from the example

<table>
<tr>
  <td> Option
  <td> XML Code
  <td> Description
<tr>
  <td>

  `http`

  <td>

  ```xml
  <Webui> http://[IP]:[PORT:8080] </Webui>
  ```

  <td>
  
  In this example I used port `8080`. You can use the whole line and just change the port number to your desired port for the WebUI.

<tr>
  <td>
  
  `https`

  <td>
  
  ```xml
  <Webui> https://[IP]:[PORT:8443] </Webui>
  ```

  <td>
  
  This is not that often used, but the maintainer for the container will usually say it is https://. In this example I used port `8443`. You can use the whole line and just change the port number to your desired port for the WebUI.
</table>

&nbsp;

### Template TemplateURL

URL to the template.

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <TemplateUrl> TEMPLATE_URL </TemplateUrl>
  ```

  <td>
  
  Where TEMPLATE_URL is the URL to the xml file.

</table>

<details>
<summary> View example </summary>
<table>
<tr>
  <td> XML Code
</tr>
  <td>

  ```xml
  <TemplateUrl> https://raw.githubusercontent.com/sebtech33/Templates/main/unRAID/docker/gluetun/my-gluetun-min.xml </TemplateUrl>
  ```

</table>
</details>

&nbsp;

### Template Icon

URL to an icon, personally I prefer them in png. It has to be loaded over https.  
Default Icon - `/plugins/dynamix.docker.manager/images/question.png`

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <Icon> ICON_URL </Icon>
  ```

  <td> Where ICON_URL is where the icon is stored. Often GitHub is used for this and many maintainers have icons in their repositories.
</table>

<details>
<summary> View example </summary>
<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Icon> https://raw.githubusercontent.com/qdm12/gluetun/master/doc/logo_256.png </Icon>
  ```

</table>
</details>

&nbsp;

### Template ExtraParams

Set with the docker run command. Some options are:

<details>
<summary> Network Admin </summary>
<table>
<tr>
  <td> Option
  <td> XML Code
  <td> Description
<tr>
  <td>

  `--cap-add=NET_ADMIN`

  <td>
  
  ```xml
  <ExtraParams> --cap-add=NET_ADMIN </ExtraParams>
  ```

  <td> Needed to create new networks.
</table>

</details>

<details>
<summary> Restart Policy </summary>

<table>
<tr>
  <td> Option
  <td> XML Code
  <td> Description
<tr>
  <td>

  `--restart no`

  <td>

  ```xml
  <ExtraParams> --restart no </ExtraParams>
  ```

  <td> Do not automatically restart the container. (the default)
<tr>
  <td>

  `--restart on-failure[:max-retries]`

  <td>

  ```xml
  <ExtraParams> --restart on-failure[:max-retires] </ExtraParams>
  ```

  <td> Restart the container if it exits due to an error, which manifests as a non-zero exit code. Optionally, limit the number of times the Docker daemon attempts to restart the container using the :max-retries option.
<tr>
  <td>

  `--restart always`

  <td>

  ```xml
  <ExtraParams> --restart always </ExtraParams>
  ```

  <td> Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted.
<tr>
  <td>

  `--restart unless-stopped`

  <td>

  ```xml
  <ExtraParams> --restart unless-stopped </ExtraParams>
  ```

  <td> Similar to always, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts.
<tr>

</table>

</details>

> :information_source: For more information look at the [Docker documentation](https://docs.docker.com/engine/reference/commandline/run/#options)

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

You will see something like this. Usually in a single line, but I have made it into a multiline so its easier to read here.
> :question: I think you can make the templates like this too, but I have not tested this.

```xml
<Config
  Name="SHARED_NAME"
  Target="/VOLUME_TARGET"
  Default="SHARED_DEFAULT"
  Mode="VOLUME_MODE"
  Description="SHARED_DESCRIPTION"
  Type="VOLUME_TYPE"
  Display="SHARED_DISPLAY"
  Required="SHARED_REQUIRED"
  Mask="SHARED_MASK">
</Config>
```

> :information_source: See everything starting with SHARED [here](#shared-config-attributes).

### Volume Target

The internal container path for the volume. Where the data you want to be persistent is located inside the container. This is usually recommended by the maintainer.

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <Config Target="/INTERNAL_PATH"/>
  ```

  <td>

  Where `/INTERNAL_PATH` is changed with the internal path to where the the persistent data is stored.
</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Config Target="/config"/>
  ```

<tr>
  <td>

  ```xml
  <Config Target="/data"/>
  ```

<tr>
  <td>
  
  ```xml
  <Config Target="/var/log"/>
  ```

</table>
</details>

&nbsp;

### Volume Mode

What permissions to use for the volume.

<table>
<tr>
  <td> Mode
  <td> XML Code
  <td> Description
<tr>
  <td>

  `rw` or `ro`
  <td>

  ```xml
  <Config Mode="MODE"/>
  ```

  <td>

  Where `MODE` is changed with the modes you want.  
  `rw` is Read-Write  
  `ro` is Read-Only  
  Slave options is also supported.
</table>

&nbsp;

### Volume Type

The only option for "Type" is always `Path` for a volume.

<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Config Type="Path"/>
  ```

</table>

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
<Config Type="Variable"/>
```

&nbsp;

## Add a Port

### Port Target

The container port.

```xml
<Config Target="PORT"/>
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
<Config Type="Port"/>
```

&nbsp;

&nbsp;

## Shared Config attributes

### Name

This is the name that shows on the left side in the unRAID template manager.

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>
  
  ```xml
  <Config Name="NAME"/>
  ```

  <td>
  
  Where `NAME` is change with a short explaination of what that Port/Volume/Variable does (Examples of this under).
</table>

<details>
<summary> View example </summary>
TODO add image of unRAID to show this

:information_source: The names don't need to be uppercase, but that is what I have usually seen in Community Applications.

<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Config Name="WEBUI"></Config>
  ```

<tr>
  <td>

  ```xml
  <Config Name="APPDATA"></Config>
  ```

<tr>
  <td>

  ```xml
  <Config Name="PUID"></Config>
  ```

</table>
</details>

### Description

This is a more extensive description of what the Port/Volume/Variable is and it shows under the input field in the unRAID template manager.

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <Config Description="DESCRIPTION"/>
  ```

  <td>
  
  Where `DESCRIPTION` is changed with a longer explaination of what that Port/Volume/Variable does (Examples of this under).
</table>

<details>
<summary> View example </summary>
TODO add image of unRAID to show this

- WebUI is the port to access the website
- Appdata to store persistent data
- PUID

</details>

### Default

This is what you think the default value should be. It allow the user to change this since your setup does not always work for everyone so this is just a suggestion.

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <Config Default="DEFAULT"></Config>
  ```

  <td>
  
  Where `DEFAULT` is changed with what you think should be default, but does not have to be.
</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Config Default="/mnt/user/appdata/gluetun-min"></Config>
  ```

<tr>
  <td>

  ```xml
  <Config Default="8080"></Config>
  ```

</table>
</details>

&nbsp;

### Display

Here is all the options for display. This is for customazation in how the template shows when someone uses it. For example the `-hide` option hides the edit and remove buttons from the template when in basic view.

<table>
<tr>
  <td> Option
  <td> XML Code Example
  <td> Description
<tr>
  <td>

  `always`

  <td>

  ```xml
  <Config Display="always"/>
  ```

  <td> Always show the Volume/Port/Variable, can be edited or deleted in basic view.
<tr>
  <td>
  
  `always-hide`

  <td>

  ```xml
  <Config Display="always-hide"/>
  ```

  <td> Always show the Volume/Port/Variable, can't be edited or deleted in basic view.
<tr>
  <td>

   `advanced`

  <td>

  ```xml
  <Config Display="advanced"/>
  ```

  <td> Shows when the user presses "Show more settings ...", can be edited or deleted in basic view.

<tr>
  <td>

   `advanced-hide`

  <td>

  ```xml
  <Config Display="advanced-hide"/>
  ```

  <td> Shows when the user presses "Show more settings ...", can't be edited or deleted in basic view.
</table>

### Required

Allows value that is entered in the template to be required. This wil insure that the field is not left empty if it has to have a value for it to work.

This just uses a boolean to make the field required or not.

<table>
<tr>
  <td> Boolean
  <td> XML Code Example
  <td> Description
<tr>
  <td>
  
  `true` or `false`

  <td>
  
  ```xml
  <Config Required="false"/>
  ```

  <td>

  Default = `false`  
  Set if the value is required to be entered.
</table>

### Mask

Allows value that is entered in the template to be hidden. This is usefull to hide sensitive information like passwords.

TODO picture of unirad showing this

<table>
<tr>
  <td> Boolean
  <td> XML Code Example
  <td> Description
<tr>
  <td>

  `true` or `false`
  <td>
  
  ```xml
  <Config Mask="false"/>
  ```

  <td>
  
  Default = `false`  
  Set if the value that is entered should be hidden.
</table>

### Additional tags

Beta

- Gives the application a warning in CA with the following text. "This application has been marked as being Beta. This does NOT neccessarily mean that there will be issues.." e.g.

<table>
<tr>
  <td> Boolean
  <td> XML Code
  <td> Description
<tr>
 <td>

 `true`

  <td>

  ```xml
  <Beta> true </Beta>
  ```

  <td> Desc
<tr>
 <td>

 `false`

  <td>

  ```xml
  <Beta> false </Beta>
  ```

  <td> Desc
</table>

Branch

Need more info on this.

- Prompts the user to choose a dockerHub tag e.g.
    linuxserver/emby.xml

&nbsp;

## Neat tricks and tips

&nbsp;

### Template predefined values aka dropdowns

The template manager support setting a set of predefined values, often uses in conjunction with variables that expect bools.
Defined by separating the values with " | ".  
This is often used when there are multiple options for a variable.

> :information_source: Has to be set in the `Default=""` attribute.  

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>
  <td>

  ```xml
  <Config Default="OPTIONS|SEPERATED|BY|PIPE"></Config>
  ```

  <td>
  
  Where you use ` | ` to seperate each variable option.
</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Config Default="true|false"/>
  ```

<tr>
  <td>

  ```xml
  <Config Default="on|off"/>  
  ```

</table>

</details>

&nbsp;

### Add Tags to containers

If you wish to be on one spesific version or LTS of a container (assuming the container has tags).  

<table>
<tr>
  <td> XML Code
  <td> Description
<tr>

  <td>
  
  ```xml
  <Repository> CONTAINER_NAME:TAG </Repository>
  ```

  <td>
  
  Where `CONTAINER_NAME` is changed with the container that you want. <br>
  <br>
  Where `:TAG` is changed with the tag you want as the default. Normally not needed since it wil pull the latest by default, but usefull if you want a data base to be on one spesific version.
</table>

<details>
<summary> View example </summary>

<table>
<tr>
  <td> XML Code
<tr>
  <td>

  ```xml
  <Repository> qmcgaw/gluetun:latest </Repository>
  ```

<tr>
  <td>

  ```xml
  <Repository> qmcgaw/gluetun:v3 </Repository>
  ```

</table>

</details>

&nbsp;

### Comment in the XML

It is possible to add comments in the xml file. It's not needed for for a detail freak like me its nice, helps remebering things too.

To add a inline comment you can do this:

```xml
<!-- YOUR COMMENT HERE -->
```

For multiline comments you can do it like this.

```xml
<!--  
YOUR COMMENT HERE
-->
```

&nbsp;

### Create a docker network

To create a docker network you will need to use the terminal.

<details open>
<summary> If you don't know how to do this, look here. </summary>

There are multiple ways to open the terminal on unRAID.

<details>
<summary> WebUI </summary>

WebUI explaination  

In unRAID's WebUI you will see this icon <img src="https://user-images.githubusercontent.com/27271446/170883559-51538954-4132-4bc8-bc89-70ae15f6610b.png" width="20" height="20"> in the top right.

![WebUI icons](https://user-images.githubusercontent.com/27271446/170881378-50a78a9d-a883-4c28-b1e6-86104caece3a.png)

> :information_source: The extra icons and search is from two plugins installed via the Community Applications.

<details>
<summary> Plugins </summary>

- Dynamix System Buttons from Dynamix Repository
- GUI Search from Andrew Zawadzki

</details>

Click that icon and a window like this will show up.

![unraid_webui_terminal](https://user-images.githubusercontent.com/27271446/170884580-0f206355-47b2-4723-8d30-c2a3f3a1271b.png)

Here you will type the docker command (command below).

&nbsp;

</details>

<details>
<summary> SSH </summary>

SSH explaination

You can use SSH to connect to the server too. This can be done by using Putty or other ssh programs. I personally use OpenSSH (on Windows with ssh-keys).

<table>
<tr>
  <td> SSH command
  <td> Description
<tr>
  <td>

  ```sh
  ssh root@<SERVER-IP>
  ```

  <td>

  Where `root` is the default ssh user on unRAID.

  Where `<SERVER-IP>` is the ip of your server.
</table>

You would then enter the root password (same as you use to log into the webui). Then enter the docker command below.

</details>

&nbsp;

</details>

<table>
<tr>
  <td> Docker command
  <td> Description
<tr>
  <td>

  ```docker
  docker network create DOCKER_NETWORK_NAME
  ```

  <td>

  Where `DOCKER_NETWORK_NAME` is the name you want for the network.  

  > :information_source: DOCKER_NETWORK_NAME has to be in one name. It can't be separated by space, but you can for example use "-" or "_" to seperate words.
</table>

<details>
<summary>Examle code</summary>

<table>

<tr>
  <td> Proxy Network
<tr>
  <td>

  ```docker
  docker network create proxy
  ```

<tr>
  <td> Secure Network
<tr>
  <td>

  ```docker
  docker network create secure_net
  ```

<tr>
  <td> Database Network
<tr>
  <td>

  ```docker
  docker network create secure-database
  ```

</table>

</details>

So you can name one `proxy` for all your reverse proxied apps so you can use the container name as a hostname instead of using an IP.

> :information_source: By doing it this way you have to use the internal port and not the external port.
For apps that has conflicting ports see if they have an enviroment variable to change the internal port.

&nbsp;

### Docker PUID, PGID and UMASK

#### PUID and PGID

> :information_source: Setting these settings wrong on unRAID will cause wrong permissions.

This sets user and group permissions.

If you run `cat /etc/passwd | grep "USER"` in your unRAID terminal you will get information like:

```sh
USER:x:ID:GROUP:USER:/:/bin/false
```

<details>
<summary> Example </summary>

```sh
tower:~# cat /etc/passwd | grep "nobody"
nobody:x:99:100:nobody:/:/bin/false

tower:~# cat /etc/passwd | grep "root"
root:x:0:0:Console and webGui login account:/root:/bin/bash

tower:~# cat /etc/passwd | grep "SMB_USER"
SMB_USER:x:1000:100:Admin User:/:/bin/false
```

</details>

<details open>
<summary>So for the User ID for nobody</summary>

:information_source: This is the recommended user for docker.

- `PUID` is `99`
- `PGID` is `100`

</details>

<details open>
<summary> For the default User ID for the default user </summary>

:information_source: This is the first user you create on unRAID. This is the SMB user.

- `PUID` is `1000`
- `PGUI` is `100`

</details>

<details open>
<summary> For the default User ID for root </summary>

> :warning: Not recommended to use this user for anything docker related

- `PUID` is `0`
- `PGUI` is `0`

</details>

See [PUID and PGID](#unraid-docker-puid-and-pgid) for more information.

&nbsp;

#### UMASK

> :warning: Setting this incorrectly can be a security risk!  
>
> :information_source: This is not always a Enviroment Variable.
If it's not added you can open an issue on the maintainer's github for the container, but remember it can be hard to add enviroment variables to container after the dockerfile is created, so it's not always easy to do from what I have understood (I can be wrong about this).

UMASK is used to set permissions for folders and files created by the container.

I don't know much about this subject, but this is what i have found on the internet.

<details open>
<summary> This is what I recommend. </summary>

> :information_source: This will keep smb working with docker files.

UMASK for `002` results in

- `775` for folders (`drwxrwxr-x`)
- `664` for files (`-rw-rw-r--`)

> :information_source: If someone has more knowlage about this and would like to help documenting this, please do.
</details>

<details open>
<summary> A more sane option would be </summary>

> :information_source: I have found this has a problem on smb if a docker creates a file.

UMASK for `022` which results in

- `755` for folders (`drwxr-xr-x`)
- `644` for files (`-rw-r--r--`)

</details>

<details open>
<summary> What root uses </summary>

> :warning: Not recommended as this is the root user and group.  
>
> :information_source: Some container uses this by default, but its not recommended.

UMASK for `000` results in

- `777` for folders (`drwxrwxrwx`)
- `666` for files (`-rw-rw-rw-`)

</details>

See [UMASK](#unraid-docker-umask) for more information

&nbsp;

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
>
> [Servarr - Docker Guide PUID and PGID](https://wiki.servarr.com/docker-guide#puid-and-pgid)

### unRAID docker UMASK

> [unRAID Forums - umask](https://forums.unraid.net/topic/32278-docker-container-developer-best-practice-guidelines-for-unraid/page/3/)
>
> [Servarr - Docker Guide umask](https://wiki.servarr.com/docker-guide#umask)
>
> [Cyberciti - Understanding linux unic umask value](https://www.cyberciti.biz/tips/understanding-linux-unix-umask-value-usage.html)
>
> [Linuxize - Understanding umask](https://linuxize.com/post/umask-command-in-linux/#understanding-umask)
