
## Filtering Operators

|Operator | Definition|
|---------|-----------|
|`and` or `&&`|and|
|`or` or `\|\|`|or|
|`eq` or `==`|equal|
|`ne` or `!=`|not equal|
|`gt` or `>`|greater than|
|`lt` or `<`|less than|

## Basic Filtering

- `ip.addr == <IP Address>` will show all the packets with the defined IP Address
- `ip.src == <SRC IP Address> and ip.dst == <DST IP Address> ` will show the captured traffic between ip.src and ip.dst
- `tcp.port eq <Port #> or <Protocol Name>` filters by TCP-protocols and shows all the traffic on defined port
- `udp.port eq <Port #> or <Protocol Name>` does the same as above, but for UDP-protocols

