# ContenedorDNS

Comenzamos creando el docker compose para lanzarlo.

~~~
services:
  bind9:
      image: internetsystemsconsortium/bind9:9.16
      container_name: bind9.16
      ports:
        - "53:53/tcp"
        - "53:53/udp"
      networks:
        bind916_subnet:
          ipv4_address: 172.0.8.152
      volumes:
        - ./conf:/etc/bind
        - ./lib:/etc/bind
networks:
  bind916_subnet: 
    external: true
~~~


