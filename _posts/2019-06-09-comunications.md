---
layout: post
title: Communications
subtitle: Apuntes de comnicaciones
tags: [Linux, Terminal]
author: Nayeong Kim
comments: False
---

## nmap

Escanea hasta el puerto <port> y solo muestra abiertos
`$nmap -p-<port> --open`

- -T <1-5> : ...

## VIM

`:colorscheme :set nu`  
`:syntax on`  
`:set cursorline`  
`:set hlsearch`  
`:sp / :usp --> divide pantalla`

## ping

- -c 1 --> número de intentos
- -R --> opcion trace

`ping -c 1 -T5 <ipaddress> -R`

## Kitty

- `<Shift+Ctrl> + n --> nueva ventana`
- `<Shift+Ctrl> + Enter --> nueva división`
- `<Shift+Ctrl> + q --> cierra tab`
- `<Shift+Ctrl> + t --> nuevo tab`
- `<Shift+Ctrl> + F2 --> config kitty`

--- ranger
--- $kitty -kitten icat <imagen>

## Exponer puerto python

```bash
	#Python2
	$python -m SimpleHTTPServer 8000
	#Python3
	$python -m http. server 8000
```

## DOCKER

Inicia contenedor y en segudo plano:  
`$docker-compose up -d`

Mostrar Logs ininterrumpido:  
`$docker logs --tail <files> -f <container> `

`$docker exec -it <container> bash`
`$docker inspect <container>`

`$docker cp file.txt <container>:</path//>`

Docker backup
`$docker commit -p <container>`
`$docker save -o out.tar`
`$docker load -i out.tar`

Directorio de contenedores...
/var/lib/docker/container

## jekyll docker

descargar theme jekyll

```
$docker run --rm --volume="$PWD:/srv/jekyll" -p 4000:4000 jekyll/jekyll:4.0 jekyll serve
```

#### flags:

- _-\-rm:_ elimina el contenedor al salir.

## SSH

### Túnel inverso - Port forwarding

### flags:

-L listen port

### Local Port forwarding

`ssh -L <puerto-local-escucha>:<host-remoto>:<puerto-remoto> <servidor-ssh>`

Acceso a puerto 22 remoto desde el puerto 9999 del localhost:  
`$ssh -L 9999:10.130.20.113:22 tics@10.130.20.113`  
Si establecida la comunicación con otro terminal:  
`$ssh tics@localhost -p 9999`$

Acceso a puerto 8000(api-https) remoto desde el puerto 9999 del localhost:  
`$ssh -NfL 9999:10.130.20.113:5000 tics@10.130.20.113`
`$curl -X GET 'https://localhost:9999/hosts' -k`

Crea regla de portforwarding para SSH:  
`$ssh -NfL <ip_origen:port>:<ip_destino:port> <@ssh_destino>`  
`Ej: $ssh -Nfl localhost:9000:localhost:5000 pi`

Eliminar la regla:  
`$ps aux | grep ssh`  
`$kill ??`

#### Dynamic Port forwarding

Permite acceder a una web blocked  
`$ssh -D <puerto-origen-dinámico-escucha> <servidor-ssh>`  
Cambiar los socks del proxy

#### Reverse Port forwarding

`$ssh -R <puerto-remoto-escucha>:<host-local>:<puerto-local> <servidor-ssh>`

SSH config (/etc/ssh/sshd_config) deben tener a 'yes'
AllowAgentForwarding & AllowTcpForwarding

## CURL

curl -X GET 'https://10.130.20.113:5000/hosts'

### flags

-X method  
-k -\-insecure --> inseguro TLS/SSL

## INFLUXDB

#### Influx backup

`$influxd backup -portable -database xxxx /path/to/store`

    ./influxd.exe (Windows)

#### Influx restore

`$influxd restore -portable /path/stored`

### Influx terminal

$>drop <database> -> elimina database
$>show series
$>show measurements
$>show tag keys
$>show field keys

- measurements (tables)  
  |- name  
  |- tags  
  |- field  
  |- time

- tags (columns)  
  |- tagkey  
  |- tagvalue

- fields (columns)  
  |- fieldkey  
  |- fieldvalue

# ARCH Linux

## PACMAN

$pacman -Syu --> actualizar   
$pacman -Qdt --> paquetes huérfanos  
$pacman -R <paquete> --> eliminar paquete

### AUR

$makepkg -si

### ERRORES RESUELTOS

ERROR - SSL certificate problem: self signed certificate in certificate chain
ERROR - Protocol "rsync" not supported or disabled in libcurl

$reflector --verbose --protocol https --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
