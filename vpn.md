# VPNs - Virtual Private Networks

Encrypt traffic between 2 locations.

<!-- INDEX_START -->

- [SSL vs IPSec VPNs](#ssl-vs-ipsec-vpns)
- [OpenVPN](#openvpn)
    - [OpenVPN Client](#openvpn-client)
  - [Tunnelblick](#tunnelblick)
- [Client VPNs](#client-vpns)
- [Consumer VPNs](#consumer-vpns)
- [Browser Fingerprinting](#browser-fingerprinting)

<!-- INDEX_END -->

## SSL vs IPSec VPNs

- site-to-site VPNs - usually between two datacenters or an office and a datacenter

<!-- -->

- client-to-site VPNs - usually between your desktop / laptop and the office or datacenter
  - consumer VPNs - these are client-to-site VPNs that are used to encrypt traffic so your ISP can't snoop on you, or to change your geographic location to watch Netflix or other streaming services that may not be available where you are physically located or may have restricted shows by country

## OpenVPN

[OpenVPN](https://openvpn.net/) is the open source standard for VPNs.

Several products are build on this open source base software and use it under the hood, eg. Tunnelblick.

#### OpenVPN Client

<https://openvpn.net/client/>

### Tunnelblick

Standard open source GUI client on Mac that can connect to OpenVPN.

## Client VPNs

- [OpenVPN Client](https://openvpn.net/client/)
- [Perimeter 81](https://www.perimeter81.com/) - user friendly VPN

## Consumer VPNs

Free VPNs generally have fewer server locations to routing your traffic from.

- [NordVPN](https://nordvpn.com/) - commercial well-established with a kill-switch to reduce risk of leakage
- [ExpressVPN](https://www.expressvpn.com/) - simple but more expensive than Nord
- [Surfshark](https://surfshark.com) - best cheap VPN
- [Private Internet Access](https://privateinternetaccess.com) - best for Linux
- [TunnelBear](https://www.tunnelbear.com/) - consumer VPN with free tier but limited to 500MB of data, use
  [ProtonVPN](https://protonvpn.com/) instead
- [PrivadoVPN](https://privadovpn.com) - free, perhaps ok for light traffic
- [Proton VPN](https://protonvpn.com/) - free to use privacy from your internet / wifi hotspot provider.
  Pay for more features or server locations.
  This often breaks DNS resolution when connecting/disconnecting on [Mac](mac.md).
  Workaround:

```shell
dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

If you are sourcing [DevOps-Bash-tools](devops-bash-tools.md) repo in your `.bashrc` there is a shell function
shortcut so you can just run: `flushdns`.

## Browser Fingerprinting

[fingerprint.com](https://fingerprint.com/) can still sort of identify you using a hash of common characteristics.
Click the link from Incognito/Private Browsing and on/off VPN to see

Documentation:

<https://dev.fingerprint.com/>

Open source library (TODO read this code):

<https://github.com/fingerprintjs/fingerprintjs>