# Tarea 3 redes 2021 - 2

Integrantes: 
- Francisco López 17637155
- Victoria López 15639622 

Ubicacion Tarea en servidor:
- vblopez -> Carpeta T3

## Consideraciones generales


## Preguntas 

1. ¿Cual es el largo en bits de la direccion IP de destino? (2 puntos)
```
El largo es de 32 bits
```
2. ¿Cual es la direccion IP de origen cuando el paquete se encuentra en el ´ router central y el ultimo dispositivo ´
visitado es el router gateway de la red Eduroam? (4 puntos)
```
La dirección IP es 172.67.7.2
```
3. ¿Cual es la direcci ´ on IP de origen cuando el paquete se encuentra en el ´ router central y el ultimo dispositivo ´
visitado es el router gateway de la red DNS? (4 puntos)
```
La dirección IP es 3.3.0.2
```
4. Describa, en orden y separado por capas de entrada y salida, todo lo que ocurre con el paquete cuando este se
encuentra en el servidor de la red DNS y el ultimo dispositivo visitado es el router ´ gateway de la red DNS. (6
puntos)
```
Primero el mensaje llega al Servidor DNS (desde la red Eduroam):
[aqui va la foto]
- Capa Entrada 1: FastEthernet0 Se recibe el frame
- Capa Entrada 2: Se revisa que la dirección MAC concuerde con la direccion MAC de destino. Y el dispositivo desencapsula el PDU del frame
- Capa Entrada 3: La dirección IP de destino concuerda con la direccion IP del dispositivo y desencapsula el paquete. El proceso ICMP recibe la echo request 
- Capa Salida 3: El proceso ICMP responde la request enviando un ICMP al Echo Reply, como la IP de destino no esta en la subred debe enviarse al siguiente punto (gateway).
- Capa Salida 2: Se busca la siguiente IP y se setea el destino del frame (dirección MAC). El dispositivo encapsula el paquete en un frame Ethernet.
- Capa Salida 1: El frame es enviado por el el FastEthernet0


Luego el mensaje pasa al switch: 
[otra foto]
- Capa Entrada 1: Aqui recibe el frame (desde FastEthernet0/2).
- Capa Entrada 2 y Capa Salida 2: revisa las direcciones MAC en la tabla MAC para enviar el mensaje 
- Capa Salida 1: Envia efectivamente el frame por el puerto correspondiente (FastEthernet0/1)

Por ultimo, el mensaje llega al router gateway de la red DNS:
[ultima foto]
- Capa Entrada 1: FastEthernet0/0 Se recibe el frame
- Capa Entrada 2: Se revisa que la dirección MAC concuerde con la direccion MAC de destino. Y el dispositivo desencapsula el PDU del frame
- Capa Entrada 3: El dispositivo busca la direccion IP en la tabla de rutas.
- Capa Salida 3: Se genera la ruta a la dirección IP de destino. El dispositivo disminuye el TTL del paquete.
- Capa Salida 2: El dispositivo encapsula el paquete en un frame HDLC.
- Capa Salida 1: El frame es enviado por el puerto Serial0/0

```


Ahora active todos los paquetes y desde uno de las laptops de la red de Casa Jorge conectese a netflix, luego responda
las siguientes preguntas:
1. ¿Cual es el largo en bytes del HTTP Request del paquete HTTP? (2 puntos)
```
El http request contiene: 
HTTP Data:Accept-Language: en-us
Accept: */*
Connection: close
Host: www.netflix.com

Lo cual tiene un largo de 32 Bytes
[insertar imagen]

```
2. Describa que tipos de paquetes se estan usando, es decir, decir que tipo de paquete son, por que se usan estos
paquetes y que deben contener. (6 puntos)
```
Se usan paquetes DNS, TCP, HTTP.
- Los paquetes DNS se usan para la comunicación entre la laptop y el Servidor DNS, de manera que la laptop pueda saber la dirección IP a la que corresponte la web wwww.netflix.com Este paquete debe contener el mensaje y la consulta DNS (DNS message y DNS query).
[imagen]
- Los paquetes TCP son utiles para comunicarse de manera efectiva con el servidor, ya que al tener la ip del servidor de netflix ya puede hacer la comunicación con el a traves de TCP. Este paquete contiene todo lo que debe contener un paquete TCP, puertos de destino y origen, SEQ, ACK, offset, flags, checksum, la data, etc.
[imagen]
- Los paquetes HTTP se utilizan para efectivamente pedir y mostrar el contenido de la pagina web. El contenido dependera de si es una request o response. En el primer caso, debe contener el metodo, host, conneccion, user-agent, accept-lengujae. En el segundo caso puede contener el protocolo, conneection, fecha, servidor, ultima modificación, largo, tipo contenido y la data.
En la pregunta 1 se muestra el HTTP request.

```
3. Describa de forma ordenada que rutas toman los distintos paquetes (especificar por donde pasan y en que orden)
```
Se inicia con los paquetes DNS. Sale un paquete de la laptop al homerouter, del que pasa al gateway para luego llegar al router central. Desde este ultimo va al gateway del DNS, pasa por el switch y llega al Servidor.
Desde el servidor DNS se devuelve al router central, pasando por el switch y el gateway DNS.
Luego se dirige a la laptop pasando así del router central al gateway de la casa de jorge y desde este al homerouter a la laptop.

Luego continuan los paquetes TCP. Sale un paquete desde la laptop al router central, pasando por el homerouter y el gateway correspondiente. Para desde este router dirigirse al Servidor Netflix, pasando por el gateway Netflix y el switch. Hecho esto se devuelve por donde mismo a la laptop.

Ahora se envian paquetes TCP y HTTP. De la laptop sale el paquete TCP y HTTP al Servidor de Netflix, por la misma ruta antes descrita (pasando por el homerouter, el gateway de la casa de jorge, el router central, el getawey de netflix, el switch y el servidor.) Hecho esto, se devuelve un paquete HTTP con la respuesta a la laptop, haciendo esta misma ruta en reversa.


```