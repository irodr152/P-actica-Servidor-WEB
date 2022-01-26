# Readme 

## Apache
~~~
apache:

    container_name: apache_sserver
    image: httpd #seleccionamos la imagen
    networks:
      nt01:
        ipv4_address: 192.0.0.10
    ports:
      - 90:90
    volumes:
      - htdocs:/usr/local/apache2/htdocs # nombre del volumen asociado a esa ruta
~~~
- Dentro de la red nt01 le ponemos la ip fija 192.168.0.10
- Para que funcione el volumen, deberemos crearlo antes de ejecutar el docker compose up

## DNS

~~~
  bind9:
    container_name: bind9_server
    image: internetsystemsconsortium/bind9:9.16

    networks:
      nt01:
        ipv4_address: 192.0.0.254 # ponemos la ip
    ports:
      - 56:56 #ponemos puerto
    volumes:
      - bind:/etc/bind
~~~

- Hacemos lo mismo que con el apache pero cambiando la ip y el volumen

## Cliente KSMWEB

~~~
cliente:
    container_name: apache_cliente
    image: kasmweb/desktop-deluxe:1.9.0-rolling

    networks:
      nt01:
        ipv4_address: 192.0.0.2
    ports:
      - 6901:6901
    dns:
      - 192.0.0.254 
    environment:
      VNC-PW: password
~~~

-Precisamente la imagen que tenemos por asi decirlo es como una esxperiencia de escritorio pero desde nuestro navegador web

-Donde pone environment: es para especificarle la contraseña que va a usar para conectarse al VNC que es con lo que podemos acceder a ese escritorio


## Configuracion general

~~~
volumes:
  bind:
    external: true
  htdocs:
    external: true
    
networks:
  nt01:
    ipam:
      config:
        - subnet: 192.0.0.0/24 
~~~

- En subnet: le decimos la subred que vamos a usar en todas las maquinas, en caso de que esta direccion de red no se corresponda con las ip dadas para el apache, dns etc no funcionará

- En external: con este parametro a true, tenemos que crear el volumen a mano, no nos lo crea directamente al hacer el docker compose


## Cuestiones propuestas
Apache (IP fija)

container_name: apache_sserver
    image: httpd #seleccionamos la imagen
    networks:
      nt01:
        ipv4_address: 192.0.0.10


## Dos host virtuales diferentes utilizando "site-enable"
~~~


~~~
## Protocolo seguro HTTPS
~~~

~~~
## Instala Wireshark para snifar paquetes DNS y HTTP

~~~
wireshark:
    container_name: wireshark_docker
    image: linuxserver/wireshark
    networks:
      nt01:
        ipv4_address: 192.0.0.40
    ports:
      - 3000:3000
    volumes:
      - wireshark_docker:/config

  wireshark_docker:
    external: true      
~~~



