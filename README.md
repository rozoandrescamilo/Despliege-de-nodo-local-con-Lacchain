[![1](https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order/blob/main/img/1.jpg?raw=true "1")](https://github.com/Smart-Contract-para-consultar-estados-de-una-Purchase-Order/blob/main/img/1.jpg?raw=true "1")

# Investigación para despliege de nodo local con Lacchain y servicio de nodo Firefly

- [Despliege de nodo local con Lacchain](#despliege-de-nodo-local-con-lacchain)
  - [Requisitos para despliege](#requisitos-para-despliege)
    - [Requisitos mínimos del sistema](#requisitos-mínimos-del-sistema)
    - [Requisitos previos](#requisitos-previos)
  - [Instalación de nodo](#instalación-de-nodo)
    - [Preparación de la instalación de un nuevo nodo](#preparación-de-la-instalación-de-un-nuevo-nodo)
  - [Implementar el nuevo nodo](#implementar-el-nuevo-nodo)
  - [Configuración de nodo](#configuración-de-nodo)
    - [Iniciar nodo](#iniciar-nodo) 
    - [Comprobando conexión](#comprobando-conexión)
- [Implementar contratos inteligentes](#implementar-contratos-inteligentes)
- [Desarrollo con herramienta Firefly](#desarrollo-con-herramienta-firefly)


# Despliege de nodo local con Lacchain

## Requisitos para despliege

### Requisitos mínimos del sistema

[![1](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/1.png?raw=true "1")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/1.png?raw=true "1")

Las caracteristicas de la máquina asiganda en DigitalOcean para el despliegue son una CPU Intel de 8 GB de Memoria, Disco en Estado Sólido de 160 GB con 5 TB de transferencia de datos y funciona con una Sistema Operativo Ubuntu 20.04 (LTS) x64.

[![2](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/2.png?raw=true "2")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/2.png?raw=true "2")

### Requisitos previos

Para este proceso se tiene en cuenta la Guía oficial de LACNet para Instalar un nodo local usando Ansible: 
[https://lacnet.lacchain.net/using-ansible/](https://lacnet.lacchain.net/using-ansible/ "https://lacnet.lacchain.net/using-ansible/")

#### Instalar Ansible

Para esta instalación se requiere Ansible. Ansible es una herramienta open source que automatiza los procesos informáticos para preparar la infraestructura, gestionar la configuración, implementar las aplicaciones y organizar los sistemas, entre otros procedimientos manuales de TI. Es necesario instalar Ansible en un máquina local que realizará la instalación del nodo en una máquina remota.

`$ sudo apt-get update`
`$ sudo apt-get install software-properties-common`
`$ sudo apt-add-repository ppa:ansible/ansible`
`$ sudo apt-get update`
`$ sudo apt-get install ansible`

[![3](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/3.png?raw=true "3")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/3.png?raw=true "3")

[![4](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/4.png?raw=true "4")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/4.png?raw=true "4")

#### Clonar Repositorio

Para configurar e instalar un nodo LACChain, se debe clonar este repositorio git en la máquina local:

`$ git clone https://github.com/LACNet-Networks/besu-networks`
`$ cd besu-networks/`

[![5](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/5.png?raw=true "5")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/5.png?raw=true "5")

#### Acceso SSH a máquina remota

Se requiere tener acceso SSH a la máquina remota donde va a implementar el nodo LACChain.Este documento asume que puede iniciar sesión en su máquina remota utilizando el siguiente comando: 

`$ ssh remote_user@remote_host`

[![6](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/6.png?raw=true "6")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/6.png?raw=true "6")

## Instalación de nodo

### Preparación de la instalación de un nuevo nodo

Hay tres tipos de nodos LACChain ( validator, bootnode y escritor ) que se pueden crear en las redes LACChain orquestadas por LACNet en este momento. Después de clonar el repositorio en la máquina local, crear una copia de “inventory.example" archivar y guardarlo como “inventory". Editar este archivo para agregar una línea para el servidor remoto donde está creando el nuevo nodo. Puede hacerlo con una herramienta gráfica o dentro del shell.

`$ cd besu-networks/`
`$ cp inventory.example inventory`
`$ vi inventory`
`[node]`
`192.168.10.72 node_ip=your.public.node.ip node_name=my_node_name node_email=your@email`

[![7](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/7.png?raw=true "7")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/7.png?raw=true "7")

[![8](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/8.png?raw=true "8")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/8.png?raw=true "8")

## Implementar el nuevo nodo

Ejecutar la siguiente linea:

`ansible-playbook -i inventory --private-key=~/.ssh/id_rsa -u "user" --ask-pass site-lacchain-node.yml`

Se preguntará qué tipo de nodo está implementando y en cuál de las redes LACChain. Para este proceso se desplegará un [2] nodo escritor en la red [1] open-protestnet.

```
[0]:validator
[1]:boot
[2]:writer
Please, choose which type of node are you deploying:

[0]:mainnet-omega
[1]:open-protestnet
[2]:legacy-protestnet (DEPRECATED)
Please, choose in which network are you deploying:
```

[![9](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/9.png?raw=true "9")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/9.png?raw=true "9")

Al finalizar todas la tareas con exito se necesita proporcionar la dirección de nodo. Se consígue ejecutando lo siguiente linea:

`$ cd /root/lacchain/data`
`$ cat nodeAddress`

[![10](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/10.png?raw=true "10")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/10.png?raw=true "10")

Dirección del nodo: `0x6f733369a81c0d36d765475f56df93f3da2d3fbb`

## Configuración de nodo

### Iniciar nodo

Una vez que el nodo esté listo, se puede iniciar con este comando en máquina remota:

`$ service pantheon start`

Verificar el estado de pantheon:

`$ service pantheon status`

[![11](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/11.png?raw=true "11")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/11.png?raw=true "11")

Si se necesita reiniciar los servicios, se puede ejecutar los siguientes comandos:

`$ service pantheon restart`

### Comprobando conexión

Para verificar si el nodo está conectado a la red correctamente. Verifique que el nodo haya establecido las conexiones con los pares:

`$ sudo -i`
`$ curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' localhost:4545`


[![12](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/12.png?raw=true "12")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/12.png?raw=true "12")

Ahora puede verificar si el nodo está sincronizando bloques obteniendo el registro de los últimos 100 bloques:

`$ tail -100 /root/lacchain/logs/pantheon_info.log`

# Implementar contratos inteligentes

Para este proceso se tiene en cuenta la Guía oficial de LACNet para Implementar smarts contracts con el nodo escritor desplegado: 
[https://lacnet.lacchain.net/smart-contracts-overview/](https://lacnet.lacchain.net/smart-contracts-overview/ "https://lacnet.lacchain.net/smart-contracts-overview/")


# Desarrollo con herramienta Firefly

Para este proceso se tiene en cuenta la Guía oficial de LACNet para Desarrollo con la herramienta Firefly: 
[https://lacnet.lacchain.net/firefly/](https://lacnet.lacchain.net/firefly/ "https://lacnet.lacchain.net/firefly/")



[![13](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/13.png?raw=true "13")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/13.png?raw=true "13")

[![14](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/14.png?raw=true "14")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/14.png?raw=true "14")

[![15](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/15.png?raw=true "15")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/15.png?raw=true "15")

[![16](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/16.png?raw=true "16")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/16.png?raw=true "16")

[![17](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/17.png?raw=true "17")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/17.png?raw=true "17")



