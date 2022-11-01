# Topologia 3 

## Cálculo de subnetting

Subredes
- RRHH
- CONTABILIDAD
- VENTAS
- INFORMATICA

Máscara de Subred

| Red                               | Host      |
| --------------------------------- | --------- |
| 1111 1111 . 1111 1111 . 1111 1111 | 0000 0000 |

Número de bits que hay que tomar para las 4 subredes:   
- 2^n => 4 
- 2^2 = 4
- 4 => 4
> **Nota:** Se necesitan tomar 2 bits de la parte del host (/26)Nueva mascara de subred: 255.255.255.192
>
Combinación de bits de nueva máscara de subred 192 = 1100 0000
Posibles combinaciones:
- 0000 0000 = 0
- 0100 0000 = 64 
- 1000 0000 = 128
- 1100 0000 = 192

Calcular número de hosts por subred:
- H = 2^n - 2 (n: numero de bits de la parte del host)
- H = 2^6 - 2 = 62

Cálculo del primer Host:
- 192.168.125.(0 + 1) = 192.168.125.1

Cálculo del último Host:
- 192.168.125.(0 + 62) = 192.168.125.62

Cálculo del Broadcast:
- 192.168.125.(62 + 1) = 192.168.125.63

| VLAN  #   |   SALTO   |       IP Red      |     Mascara     |   Primer Host  |  Ultimo Host   |    Broadcast   | HOST TOTALES  | CANTIDAD DE HOSTS |
| --------- | --------- | ----------------- | --------------- | -------------- | -------------- | -------------- | ------------- | ----------------- |
| 10-(RRHH)  |     64    |  192.168.125.0/26  | 255.255.255.192 |  192.168.125.1  | 192.168.125.62  | 192.168.125.63  |       62      |         1         |
| 20-(CONTA) |     64    |  192.168.125.64/26 | 255.255.255.192 |  192.168.125.65 | 192.168.125.126 | 192.168.125.127 |       62      |         1         |
| 30-(VENTAS)|     64    | 192.168.125.128/26 | 255.255.255.192 | 192.168.125.129 | 192.168.125.190 | 192.168.125.191 |       62      |         1         |
| 40-(INFOR) |     64    | 192.168.125.192/26 | 255.255.255.192 | 192.168.125.193 | 192.168.125.254 | 192.168.125.255 |       62      |         1         |


--

## Comandos utilizados en la topología 3

### Configuracion vtp - sw1

```console
configure terminal
vtp domain GRUPO12
vtp password GRUPO12
vtp version 2
vtp mode server
end
```

### INTERFACE TRUNK - ESW1

```console
configure terminal
interface f1/4
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
```

### VLANS EN - ESW1

```console
configure terminal
vlan 10
name RHUMANOS
exit
vlan 20
name CONTABILIDAD
exit
vlan 30
name VENTAS
exit
vlan 40
name INFORMATICA
exit
end
```

### ROUTER - ON A STICK - R1

```console
configure terminal
interface f0/0
no shutdown
exit

interface f0/0.10
encapsulation dot1Q 10
ip address 192.168.125.1 255.255.255.192
exit

interface f0/0.20
encapsulation dot1Q 20
ip address 192.168.125.65 255.255.255.192
exit

interface f0/0.30
encapsulation dot1Q 30
ip address 192.168.125.129 255.255.255.192
exit

interface f0/0.40
encapsulation dot1Q 40
ip address 192.168.125.193 255.255.255.192
exit
```

### INTERFACE MODE ACCESS - ESW1

```console
configure terminal
interface f1/0
switchport mode access
switchport access vlan 10
exit

configure terminal
interface f1/1
switchport mode access
switchport access vlan 20
exit

configure terminal
interface f1/2
switchport mode access
switchport access vlan 30
exit

configure terminal
interface f1/3
switchport mode access
switchport access vlan 40
exit
```

### IP VPCS - vpc

```console
ip 192.168.125.7 255.255.255.192 192.168.125.1
save

ip 192.168.125.77 255.255.255.192 192.168.125.65
save

ip 192.168.125.142 255.255.255.192 192.168.125.129
save

ip 192.168.125.202 255.255.255.192 192.168.125.193
save
```

### CONFIGURACION INTERFACES - R1

```console
configure terminal
interface f1/0
ip address 10.12.0.22 255.255.255.252
no shutdown
exit

configure terminal
interface f2/0
ip address 10.12.0.14 255.255.255.252
no shutdown
exit
```

### CONFIGURACION RIP - R1

```console
configure terminal
router rip
version 2
network 10.12.0.12
network 10.12.0.20
network 192.168.125.0
network 192.168.125.64
network 192.168.125.128
network 192.168.125.192
end
```

### Otras configuraciones

```console
GUARDAR EN SWITCH
copy running-config startup-config
```

### ver mac

```console
sh ip
```
