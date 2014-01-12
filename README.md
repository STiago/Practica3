Practica 2 Copyright (C) 2013 María Victoria Santiago Alcalá. This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version. This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details. You should have received a copy of the GNU General Public License along with this program. If not, see .

Practica3 - Diseño de sistemas usando CPU y almacenamiento virtuales.
====================================================================

## INTRODUCCIÓN

En esta práctica, realizo el diseño de una máquina virtual para una aplicación, la cual desarrollé en la asignatura "Diseño de aplicaciones para Internet". Para ello, se ha de buscar un rendimiento aceptable de las máquinas virtuales creadas las cuales tendran número de procesadores y una cantidad de memoria RAM determinados.
Usaré VMware Player como software de virtualización para esta práctica.

    Hay un total de 6 máquinas virtuales:
      
      UBUNTU:
          Ubuntu : es una máquina servidora.
          Ubuntu1: al igual que la anterior es otra máquina servidora.
          Ubuntu Balanceador: es el balanceador con nginx instalado.
          
      CENTOS:
          Centos 1: máquina servidora.
          Centos 2: máquina servidora.
          Centos Balanceador: máquina balanceadora con el servidor nginx instalado.


Con ello, lo que hacemos es configurar una red entre las dos maquinas virtuales servidoras de cada distribución de teniendo un balanceador que reparta la carga entre los servidores, terminando con el posible problema de sobrecarga de estos servidores.
El fin es balancear nuestros servidores http tras configurarlos como explicaré a continuación en la sección de `MAQUINAS VIRTUALES`, consiguiendo con ello que la infraestructura tenga una alta disponibilidad.

Finalmente probraremos la eficiencia de las diferentes configuraciones de las máquinas, comprobando el rendimiento de las mismas tanto en Ubuntu como en CentOS.

## MÁQUINAS VIRTUALES

A continuación, realizo una breve descripción de las máquinas virtuales que he creado, dandole a cada una memoria RAM y un número de prodesadores determinado y distinto a las demás:

### UBUNTU

#### MÁQUINA 1: Ubuntu Server 12.04 - Ubuntu 
Descripción: 2 procesador / 1024 MB RAM


#### MÁQUINA 2: Ubuntu Server 12.04 - Ubuntu 1
Descripción: 1 procesador / 512 MB RAM


#### MÁQUINA 3: Ubuntu Server 12.04 - Ubuntu Balanceador
Descripción: 1 procesador / 1024 MB RAM

Las máquinas 1 y 2 al ser las servidoras, mostraran nuestra aplicación como he comentado al principio de esta memoria.
Para ello, tenemos que configurarla desde línea de comandos en /var/www/ conde encontraremos el fichero index.html el cual sustituiremos por el de nuestra aplicación.


En la máquina 3, procederemos a configurar el balanceador de http. En mi caso he elegido NginX debido a su buen rendimiento, aunque también se puede hacer con HaProxy, Pound, Light o cualquier otro software o servidor.

A continuación, procedo a describir el proceso de instalación de nginx en ubuntu:

        PASOS:
        
            1. Importamos  la clave del repositorio:
            
                cd /tmp/
                wget http://nginx.org/keys/nginx_signing.key
                apt-key add /tmp/nginx_signing.key
                rm -f /tmp/nginx_signing.key
                
            2. Añadimos el repositorio editando /etc/apt/sources.list :
                
                echo "deb http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list
                echo "deb-src http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list
            
            3. Procecemos a hacer el update y a instalar nginx con :
                
                sudo apt-get install nginx
                
            4. Ahora editamos el fichero de configuración que se encuentra en /etc/nginx.confd/default.conf añadiendo:
            
                upstream apaches {
                    server 192.168.246.128
                    server 192.168.246.130
                }
                
                server{
                    listen 80;
                    server_name miservidor;
                    access_log /var/log/nginx/miservidor.access.log;
                    error_log /var/log/nginx/miservidor.error.log;
                    root /var/www/;
                
                    location /{ 
                        proxy_pass http://apaches;
                        proxy_set_header Host $host;
                        proxy_set_header X-Real-IP $remote_addr;
                        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                        proxy_http_version 1.1;
                        proxy_set_header Connection "";
                    }
                }
                
            5. Lanzamos el servicio nginx:
                
                service nginx restart
                
            6. Hacemos en la anfitriona curl seguido de la ip de nuestra máquina balanceadora:
                
                curl http://192.168.246.129  

Finalmente, obtendremos en el terminal el balanceo de una maquina y de otra como se muestra en la siguiente captura a modo de prueba de que funciona toda la configuración realizada anteriormente:

![Practica3](https://dl.dropbox.com/s/et14umdakwmx2bb/algo1.png)


También podemos abrir nuestro navegador, introduciendo la IP de la máquina balanceadora y nos mostrara nuestra aplicación.






## CENTOS

#### MÁQUINA 4: CentOS 6.5 - CentOS 1
Descripción: 2 procesador / 256 MB RAM

#### MÁQUINA 5: CentOS 6.5 - CentOS 2
Descripción: 1 procesador / 1024 MB RAM


#### MÁQUINA 6: CentOS 6.5 - CentOS Balanceador
Descripción: 2 procesador / 512 MB RAM

## BENCHMARK


