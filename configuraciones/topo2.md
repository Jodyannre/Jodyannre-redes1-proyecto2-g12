# Topología 2 (Oficina Central)

![topo2](images\topo2.PNG)

### Configuración de enlaces troncales para ESW2, ESW3, ESW4 y ESW5

como primer paso se configura la redundancia en los switch de capa 3 para que la comunicación sea efectiva, de todos los switch se establece uno como un switch server en el que se crean las vlans y luego se replican hacia las demás.

### ESW4 (Switch Server) 

###  ![esw4](images\esw4.PNG)

#### Configuración de VTP

conf ter

vtp domain redes1gp12

vtp password redes1gp12

vtp mode server

#### Configuración de PortChannel (Agrupar todas las interfaces para redundancia)

conf ter

interface range f1/1 - 2

channel-group 1 mode on

end

//validar configuración 

show etherchannel port-channel

conf ter

interface range f1/3 - 4

channel-group 2 mode on

end

show etherchannel summary

-- mode trunk de portchanels

#conf ter

#interface port-channel 1

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

#conf ter

#interface port-channel 2

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

-- conexion a router

#conf ter

#interface f1/0

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

-- configurar vlans

#conf ter

#vlan 10

#name RHUMANOS

#exit

#vlan 20

#name CONTABILIDAD

#exit

#vlan 30

#name VENTAS

#exit

#vlan 40

#name INFORMATICA

#exit

#shown vlan-sw

//configuracion de stp

#conf ter

#vtp pruning

#conf ter

#spanning-tree vlan 10 root primary

#spanning-tree vlan 20 root primary



#spanning-tree vlan 30 root primary

#spanning-tree vlan 40 root primary

//verificar stp

#sh spanning-tree brief

//ver puertos

#show spanning-tree blockedports

### ESW2 (Cliente)

### ![esw2](images/esw2.png)



-- CONFIGURACION DE PORTCHANNEL (AGRUPAR TODAS LAS INTERFACES PARA REDUNDANCIA)

#conf ter

#interface range f1/8 - 9

#channel-group 6 mode on

#end

//validar configuración 

#show etherchannel port-channel

#conf ter

#interface range f1/1 - 2

#channel-group 3 mode on

#end

//validar configuración 

#show etherchannel port-channel

#show etherchannel summary

#conf ter

#interface range f1/5 - 7

#channel-group 5 mode on

#end

-- mode trunk de portchanels

#conf ter

#interface port-channel 2

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

#conf ter

#interface port-channel 3

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

#interface port-channel 5

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

-- agregar vtp

#conf ter

#vtp domain redes1gp12

#vtp password redes1gp12

#vtp mode client

#end

#vtp status

#show vlans 

### ESW5 (Cliente) 

### ![esw2](images/esw5.png)

-- CONFIGURACION DE PORTCHANNEL (AGRUPAR TODAS LAS INTERFACES PARA REDUNDANCIA)

#conf ter

#interface range f1/1 - 2

#channel-group 3 mode on

#end

//validar configuración 

#show etherchannel port-channel

#conf ter

#interface range f1/3 - 4

#channel-group 4 mode on

#end

//validar configuración 

#show etherchannel port-channel

#show etherchannel summary

-- mode trunk de portchanels

#conf ter

#interface port-channel 3

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

#conf ter

#interface port-channel 4

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

-- agregar vtp

#conf ter

#vtp domain redes1gp12

#vtp password redes1gp12

#vtp mode client

#end

#vtp status

#show vlan-sw



### ESW3 (Cliente)

### ![esw3](images/esw3.png)



-- CONFIGURACION DE PORTCHANNEL (AGRUPAR TODAS LAS INTERFACES PARA REDUNDANCIA)

#conf ter

#interface range f1/1 - 2

#channel-group 1 mode on

#end

//validar configuración 

#show etherchannel port-channel

#conf ter

#interface range f1/3 - 4

#channel-group 4 mode on

#end

//validar configuración 

#show etherchannel port-channel

#show etherchannel summary

#conf ter

#interface range f1/5 - 7

#channel-group 5 mode on

#end

//validar configuración 

#show etherchannel port-channel

-- mode trunk de portchanels

#conf ter

#interface port-channel 1

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

#conf ter

#interface port-channel 4

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

#conf ter

#interface port-channel 5

#switchport mode trunk

#switchport trunk allowed vlan 1,10,20,30,40,1002-1005

#end

#show int trunk

-- agregar vtp

#conf ter

#vtp domain redes1gp12

#vtp password redes1gp12

#vtp mode client

#end

#vtp status

#show vlan-sw 

### R2 (Router de topologia)

###  ![R2](images/r2.png)

Se crean vlans y se establece el modo trunk a traves de Dot1Q

#conf ter

#interface f1/0

#no shutdown

#int f1/0.10

#int f1/0.20

#int f1/0.30

#int f1/0.40

#do sh run

int f1/0.10

encapsulation dot1Q 10

int f1/0.20

encapsulation dot1Q 20

int f1/0.30

encapsulation dot1Q 30

int f1/0.40

encapsulation dot1Q 40

#interface f1/0

#no shutdown

#int f1/0.10

#encapsulation dot1Q 10

#ip address 192.168.43.222 255.255.255.224

#int f1/0.20

#encapsulation dot1Q 20

#ip address 192.168.43.238 255.255.255.240

#int f1/0.30

#encapsulation dot1Q 30

#ip address 192.168.43.126 255.255.255.128

#int f1/0.40

#encapsulation dot1Q 40

#ip address 192.168.43.190 255.255.255.192

### Configuracion de VPC'S