# ContenedorDNS

Comenzamos creando el docker compose para lanzarlo :

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

Creamos la carpeta de configuracion con los siguientes documentos:

name.conf

~~~
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
~~~

named.conf.local
~~~
zone "asircastelao.int" {
	type master;
	file "/var/lib/bind/db.asircastelao.int";
	allow-query {
		any;
		};
	};
~~~

name.conf.options
~~~
options {
	directory "/var/cache/bind";

	forwarders {
	 	8.8.8.8;
		1.1.1.1;
	 };
	 forward only;

	listen-on { any; };
	listen-on-v6 { any; };

	allow-query {
		any;
	};
};
~~~

por ultimo la named.conf.default-zones

~~~
// prime the server with knowledge of the root servers
zone "." {
	type hint;
	file "/usr/share/dns/root.hints";
};

// be authoritative for the localhost forward and reverse zones, and for
// broadcast zones as per RFC 1912

zone "localhost" {
	type master;
	file "/etc/bind/db.local";
};

zone "127.in-addr.arpa" {
	type master;
	file "/etc/bind/db.127";
};

zone "0.in-addr.arpa" {
	type master;
	file "/etc/bind/db.0";
};

zone "255.in-addr.arpa" {
	type master;
	file "/etc/bind/db.255";
};
~~~

Creamos la carpeta zonas y dentro introducimos el documento db.asircastelao.int
~~~
$TTL 38400	; 10 hours 40 minutes
@		IN SOA	ns.asircastelao.int. some.email.address. (
				10000002   ; serial
				10800      ; refresh (3 hours)
				3600       ; retry (1 hour)
				604800     ; expire (1 week)
				38400      ; minimum (10 hours 40 minutes)
				)
@		IN NS	ns.asircastelao.int.
ns  	IN A		172.28.5.1
pag2	IN A		172.28.5.4
pag3	IN A 		172.28.5.7
www	    IN CNAME	test
global	IN TXT		mensaje
~~~





