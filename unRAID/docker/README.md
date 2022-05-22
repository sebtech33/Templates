# Ultimate Guide for unRAID docker templates (WIP)

Simple explanation for all the options when creating a xml template for Community Applications (the manual way).

&nbsp;

Get the Community Applications Plugin:
>[unRAID Community Applications](https://forums.unraid.net/topic/38582-plug-in-community-applications/)

&nbsp;

[Create a docker network](#create-a-docker-network)

## Template example

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

- Custom networks can also be created.
  - `custom: br0`
  - `custom: wg0`
  - `custom: DOCKER_NETWORK`

For custom docker networks go to [Create a docker network](#create-a-docker-network).

&nbsp;

### Privileged

> :warning: Be carefull with this!

Usally false if not specified otherwise by image maintainer or you know what your doing.

Options [default = `false`]

- `true`
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

### **Template predefined values aka dropdowns**

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

### **Create a docker network**

For unRAID you will need to use the terminal

```docker
docker network create DOCKER_NETWORK_NAME
```

Where DOCKER_NETWORK_NAME is the name you want for the network

- So you can name one `proxy` for all your reverse proxied apps so you can use the container name as a hostname instead of using an IP.

&nbsp;

&nbsp;

&nbsp;

## References

>[selfhosters unRAID templating guide](https://selfhosters.net/docker/templating/templating/)  
>[unRAID Forums](https://forums.unraid.net/topic/101424-how-to-publish-docker-templates-to-community-applications-on-unraid/)  
