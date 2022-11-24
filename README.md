# My DN42 config and scripts (WIP)

I use github workflow to generate wireguard and bird config files, and set cron to run bash scripts on nodes to fetch artifacts once in a while. This repo is inspired by DN42 official registry repo.

## Peer with me

The peer config file is designed to be as simple as possible. Below is a valid config file which defines two peers with my Los Angeles and Montreal node:

```ini
# config/peers/amemiya.conf

[shared]
asn = 4242422464

[lax]
address = las.dneo.moeternet.com
pubkey = viR4CoaJTBHROo/Bgbb27hQ2ttr8AbByGY/yOz3D3GY=

[mtl]
address = nyc.dneo.moeternet.com
pubkey = KLF+h2/4FG5IjmVrywonwLP30slUzuoeRIb3e+awklY=
```

Begin with a common practice. To peer with a node with link-local address (then you'd better have `extended next hop on`), you can set:

```ini
# config/peers/<your nickname>.conf

[lax]
asn = <your asn number, dn42 or neonetwork>
link_local = <your link local address>
address = <domain or ip of your node on internet>
port = <port you set for wireguard connection with me>
pubkey = <your wireguard public key>
pskey = <your wireguard preshared key>
```

And the rules:

1. do not write any abbreviation of subnet mask like `/32` or `/64` in config file. And do not cover your value by single or double quotes. Please also check `config/nodes` for node names, address (endpoint) and ipv4/ipv6 availability in advance.

2. Nickname is the filename of the config file, and it should be no less than 4 characters and no more than 10 characters, containing only numbers, English letters, half-width underscore and hypen. It's better to pick your popular nickname which is used in such as Telegram, Github or Discord account so the name of interface and bird protocols are more recognizable.

3. The name of the section should be the name of the node I have.

4. If the link-local address is `fe80::<your asn last 4 digits>`, you can skip it.

5. Since both of us can take the initiative to establish the wireguard tunnel, the address is not necessary if my node has one. Do not provide ipv6 address, or domain that only have AAAA record to my nodes that do not have ipv6 access, and same for ipv4.

6. If the port you provide is `2<my asn last 4 digits>`, in this case `20365`, you can skip it as well. If you do not want to provide an address, the port can also be ignored.

7. The preshared key is not necessary. Write it if you want to set one.

This is another example, which peers with DN42 address instead of link-local one.

```ini
# config/peers/<your nickname>.conf

[lax]
asn = <your asn number, dn42 or neonetwork>
ipv4 = <dn42 ipv4 address of your node>
ipv6 = <dn42 ipv6 address of your node>
address = <domain or ip of your node on internet>
port = <port you set for wireguard connection with me>
pubkey = <your wireguard public key>
pskey = <your wireguard preshared key>
```

The ipv4 and ipv6 addresses are also not both necessary. If you only set one address, the script will set up peer with that address only, thus only ipv4 or ipv6 BGP tables are given.

If you want to peer with multiple nodes of mine, you can add a section named `[shared]`, and place anything in common there. Note that `[shared]` has lower priority than other node sections. If you want to undefine the keys defined in `[shared]`, you can write `<key> = none`.

Play with it!