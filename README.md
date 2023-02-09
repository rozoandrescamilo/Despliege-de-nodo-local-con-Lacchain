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
    - [Herramientas de monitoreo](#herramientas-de-monitoreo)
- [Implementar contratos inteligentes](#implementar-contratos-inteligentes)
  - [Ejecutar el componente de retransmisor](#ejecutar-el-componente-de-retransmisor)
  - [Truffle como entorno de desarrollo](#truffle-como-entorno-de-desarrollo)
    - [Compilación de contratos](#compilación-de-contratos)
    - [Compilación de contratos](#compilación-de-contratos)
- [Consideraciones para red principal y Firefly](#consideraciones-para-red-principal-y-firefly)



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

No olvidarse de escribir el enode que se muestra en las salidas, para que se le permita permisionar el nodo como se indica junto con el formato de Autorización de acuerdo comunicandose con tech.support@lac-net.net y comercial@lac-net.net 

```
TASK [lacchain-validator-node : print enode key] ***********************************************
ok: [x.x.x.x] => {
    "msg": "enode://cb24877f329e0e3fff6c7d7b88d601b698a9df6efbe1d91ce77130f065342b523418b38cb3c92ea3bcca15344e68c7d85a696eb9f8c0152c51b9b7b74729064e@a.b.c.d:60606"
}
```
Dirección enode registrada:

`enode://0c2cf6b7e3ac442d92d025af6fcec662fe45678990ab4abdaee2e9da6387fa3d3205673096387b35ee28219a2c0706b3eaa8f45cffcff65f406dc9f6bddd6be5@138.197.8.244:60606`

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

[![13](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/13.png?raw=true "13")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/13.png?raw=true "13")

### Herramientas de monitoreo

- **Dashboard de LacChain:** [https://dashboard.lacchain.net/](https://dashboard.lacchain.net/ "https://dashboard.lacchain.net/")

Es un panel de control para LacChain, una iniciativa de código abierto para crear una infraestructura de blockchain para América Latina y el Caribe. En este panel, los usuarios pueden ver información sobre la red LacChain, como la cantidad de nodos, transacciones, estado de la red, y otros datos relevantes. Además, permite a los participantes de la red monitorear y visualizar el rendimiento de la red, y proporciona una interfaz para interactuar con la misma.

[![14](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/14.png?raw=true "14")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/14.png?raw=true "14")

- **Explorador de LacChain:** [https://explorer.lacchain.net/](https://explorer.lacchain.net/ "https://explorer.lacchain.net/")

El explorador de bloques permite a los usuarios ver y rastrear transacciones y otra información en tiempo real en la red de blockchain de LacChain. Con esta herramienta, los usuarios pueden ver detalles sobre las transacciones, incluyendo el momento en que se realizaron, las direcciones de las cuentas implicadas y la cantidad de tokens transferidos.

En resumen, la función del Explorador es proporcionar una vista detallada y en tiempo real de la actividad en la red de blockchain de LacChain.

[![15](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/15.png?raw=true "15")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/15.png?raw=true "15")

# Implementar contratos inteligentes

Para este proceso se tiene en cuenta la Guía oficial de LACNet para Implementar smarts contracts con el nodo escritor desplegado: 
[https://lacnet.lacchain.net/smart-contracts-overview/](https://lacnet.lacchain.net/smart-contracts-overview/ "https://lacnet.lacchain.net/smart-contracts-overview/")

## Ejecutar el componente de retransmisor

Se inicializa en entorno de trabajo con el siguiente comando y se debe tener configurada la variable WRITER_KEY.

En caso de que no se tenga la variable WRITER_KEY en el entorno, configure esta variable con el contenido del archivo/lacchain/data/key

`$ env`

`$ export WRITER_KEY=PRIVATE_KEY  //where PRIVATE_KEY is content of /lacchain/data/key`

[![16](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/16.png?raw=true "16")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/16.png?raw=true "16")

Ingresar a la consola del nodo y ejecutar los siguientes comandos:

`cd /root/lacchain/gas-relay-signer`

`systemctl import-environment WRITER_KEY`

`service relaysigner start`

[![17](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/17.png?raw=true "17")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/17.png?raw=true "17")

Verificar que el retransmisor funcione bien:

`cd /root/lacchain/gas-relay-signer/log`

`tail -100 idbServiceLog.log`

Las primeras líneas deberían ser algo como esto:

[![18](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/18.png?raw=true "18")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/18.png?raw=true "18")

Eso significa que el retransmisor está configurado y funciona bien.

## Truffle como entorno de desarrollo

Truffle es un entorno de desarrollo donde puede desarrollar fácilmente contratos inteligentes con su marco de prueba incorporado, compilación e implementación de contratos inteligentes, consola interactiva y muchas más funciones. Hemos creado un proveedor personalizado de código abierto de truffles para trabajar con el nuevo modelo de gas para permitirle usar Truffle en LACChain Networks.

Como primer paso se requiere instalar Truffle:

`npm install -g truffle`

`truffle version`

[![19](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/19.png?raw=true "19")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/19.png?raw=true "19")

[![20](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/20.png?raw=true "20")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/20.png?raw=true "20")

Se debe crear la carpeta de proyecto, por ejemplo MyDapp.

`mkdir MyDapp`

`cd MyDapp`

Para iniciar el servicio de truffle se ejecuta el siguiente comando en el directorio MyApp:

`truffle init`

[![21](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/21.png?raw=true "21")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/21.png?raw=true "21")

Este comando creará un proyecto de truffle nuevo. Después de hacerlo, debe tener los siguientes archivos y carpetas:

contracts/: Directorio para contratos de Solidity
migrations/: Directorio para implementación con scriptable
test/: Directorio para archivos de prueba para probar su aplicación y contratos
truffle-config.js: archivo de configuración de truffle

### Compilación de contratos

Para iniciar se crea un contrato inteligente muy simple llamado MyContract.sol y almacenarlo en la carpeta de contratos. Todos los contratos inteligentes que se creen deben almacenarse allí. 

Este contrato inteligente contendrá código tan simple como este:

- Básicamente, nuestro contrato inteligente tiene una variable llamada menssage, que contiene un pequeño mensaje que se inicializa como First Smart Contract. También tenemos dos funciones que pueden establecer u obtener la variable message.

[![22](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/22.png?raw=true "22")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/22.png?raw=true "22")

[![23](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/23.png?raw=true "23")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/23.png?raw=true "23")

Para compilar el contrato inteligente, se ejecuta el comando:

`truffle compile`

[![24](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/24.png?raw=true "24")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/24.png?raw=true "24")

Se requiere instalar el gas-model-provider de truffle según Using Hyperledger Besu with Truffle para poder desplegar contratos y enviar transacciones con truffle:

`npm install -g @lacchain/truffle-gas-model-provider`

[![25](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/25.png?raw=true "25")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/25.png?raw=true "25")

Ahora, elimine el archivo "1_initial_migrations.js" y cree un nuevo archivo en el directorio de migraciones. Crea un nuevo archivo llamado 1_deploy_contracts.js, y escribe el siguiente código:

[![26](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/26.png?raw=true "26")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/26.png?raw=true "26")

A continuación, dado que [LACChain Networks requiere incluir dos parámetros(nodeAddress, expiration)](link), es necesario añadirlos como parámetros a la ABI del contrato. Por favor, añada estos dos parámetros como entradas en la función constructora.

[![27](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/27.png?raw=true "27")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/27.png?raw=true "27")

A continuación, debemos editar la configuración de Truffle (truffle-config.js).

Para describir brevemente las partes que componen la configuración:

- networks: Contendrá la configuración de nuestro cliente Ethereum donde desplegaremos nuestros contratos
- compilers: Contendrá la configuración del compilador Solc

Se debe escribir la clave privada, el nodo IP de la dirección de red y el puerto RPC en la parte de redes:

[![28](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/28.png?raw=true "28")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/28.png?raw=true "28")

Finalmente se obtiene el informe de despliegue donde se puede ver el contrato de dirección similar al siguiente usando el comando:

`truffle migrate -network lacchain`

```
Deploying 'MyDapp'
--------------------
transaction hash:0x31d91fa2524953e49cfc4c433ac939b56df8d9371fdde74c56a75634efcf823d
Blocks: 0            Seconds: 0
contract address:    0xFA3F403BeC6D3dd2eF9008cf8D21e3CA0FD1B9C4
block number:        4006082
block timestamp:     1574190784
account:             0xbcEda2Ba9aF65c18C7992849C312d1Db77cF008E
balance:             0
gas used:            340697
gas price:           0 gwei
value sent:          0 ETH
total cost:          0 ETH

```

# Consideraciones para red principal y Firefly

LACChain Networks son redes blockchain desarrolladas por LACChain Alliance y orquestadas por LACNet. Estas redes están clasificadas como redes públicas de blockchain autorizadas, tal como se define en la norma ISO TC307 WG5 TS23635 y han sido diseñadas con un enfoque especial en América Latina y el Caribe. Como redes públicas de blockchain, las Redes LACChain están abiertas a cualquier entidad en el mundo. Como redes permisionadas, todos los operadores de nodos, excepto los nodos observadores, deben autenticarse y comprometerse a cumplir con la regulación para poder ser permisionados y enviar transacciones o participar en el protocolo de consenso.

Las redes LACChain no tienen costes de transacción, lo que permite la escalabilidad de las aplicaciones. Existen dos tipos de Redes LACChain orquestadas por LACNet, las ProTestnets y las Mainnets. Las ProTestnets distribuyen los recursos necesarios para emitir transacciones de forma equitativa entre todos los nodos escritores y su uso es totalmente gratuito. Las Mainnets tienen un modelo basado en membresías para cubrir los costes del equipo de soporte de LACNet. Las membresías de nivel superior incluyen más tx por segundo y una respuesta más rápida a los tickets de soporte. En la actualidad, las redes LACChain son:

- Mainnet Omega network 
- Pro-Testnet network 

[![29](https://github.com/rozoandrescamilo/Despliege-de-nodo-local-con-Lacchain/blob/main/img/29.png?raw=true "29")](https://github.com/Despliege-de-nodo-local-con-Lacchain/blob/main/img/29.png?raw=true "29")

Membresias anuales: [https://lacnet.lacchain.net/get-your-membership/](https://lacnet.lacchain.net/get-your-membership/ "https://lacnet.lacchain.net/get-your-membership/")

En cuanto al stack de Firefly, que proporciona herramientas para gestionar transacciones, desplegar contratos inteligentes y APIs, y escalar aplicaciones web3 seguras. Es necesario desplegarlo sobre la Mainnet Omega network, siendo necesario contar con alguna de las membresías para cubrir los costes del equipo de soporte de LACNet.

Para este proceso se tendría de referencia la Guía oficial de LACNet para Desarrollo con la herramienta Firefly: [https://lacnet.lacchain.net/firefly/](https://lacnet.lacchain.net/firefly/ "https://lacnet.lacchain.net/firefly/")





















