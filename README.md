# Rendu projet reseau

Voici l'infra GNS :

![Screenshot_1](https://user-images.githubusercontent.com/34342829/57858497-77aa8b80-77f1-11e9-80d8-076469d5bcad.png)



fichier de conf client 1 :

![image](https://user-images.githubusercontent.com/34342829/58159461-db630780-7c7c-11e9-9fd8-3ddcc6d33db2.png)

fichier de conf client 3 :

![image](https://user-images.githubusercontent.com/34342829/58159571-17966800-7c7d-11e9-9bc5-e71734333107.png)

configuration du VLAN 10 :

-SUR IOU1
``````
# conf t
(config)# vlan 10
(config-vlan)# name client1-3_network
(config-vlan)# exit
(config)# interface Ethernet 0/1
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
``````
-SUR IOU2
``````
# conf t
(config)# vlan 10
(config-vlan)# name client1-3_network
(config-vlan)# exit
(config)# interface Ethernet 0/0
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
``````

Le client 1 ping le client 3 car ils sont dans le meme reseau et le meme VLAN, ils ne passent donc pas par le routeur pour se joindre : 

![Screenshot_2](https://user-images.githubusercontent.com/34342829/57858624-af193800-77f1-11e9-81cf-4843e014e4ef.png)

Fichier de conf du client 2 :

![image](https://user-images.githubusercontent.com/34342829/58159961-cfc41080-7c7d-11e9-8f15-79db8b01b747.png)


configuration du VLAN 20 sur IOU1 :

``````
# conf t
(config)# vlan 20
(config-vlan)# name client2_network
(config-vlan)# exit
(config)# interface Ethernet 0/2
(config-if)# switchport mode access
(config-if)# switchport access vlan 20
``````

Le client 2 n'est pas dans le meme reseau, il faut donc qu'il passe par le routeur pour joindre le client 1 et le client 3.  Pour cela on configure un "router on a stick" afin d'avoir deux sous interface sur l'interface 1/0 :

![image](https://user-images.githubusercontent.com/34342829/58160383-a0fa6a00-7c7e-11e9-9d08-0f8694067062.png)


Grace a ca et a la configuration des VLAN, les clients 1 et 3 ping le client 2 qui n'est pas dans le meme reseau en passant donc par le routeur :

![Screenshot_3](https://user-images.githubusercontent.com/34342829/57858862-1afba080-77f2-11e9-9090-c85236082d77.png)


Configuration du nat sur le routeur :

``````
(config)# interface fastEthernet 0/0
(config-if)# ip nat outside
(config-if)# exit

(config)# interface fastEthernet 1/0
(config-if)# ip nat inside
(config-if)# exit
``````

Tout les clients ont accés a internet car le nat est configuré sur le router :

![Screenshot_4](https://user-images.githubusercontent.com/34342829/57858964-5302e380-77f2-11e9-8d25-20c03fa261cf.png)



