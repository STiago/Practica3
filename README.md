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
Para ello, tenemos que configurarla desde línea de comandos en /var/www/ donde encontraremos el fichero index.html el cual sustituiremos por el de nuestra aplicación.

A modo de ejemplo inicial, sin meter aun la aplicación y modificando solo el index.html que viene por defecto quedaría como muestra la siguiente captura, y quedaría pendiente meter la aplicacion la cual he dejado para después de ver que funcionan perfectamente antes.


![Practica3](https://dl.dropbox.com/s/3e48bzbmfj1lq3n/pra9%28modificacion%20contenido%20index%20maquinas%29.png)


En la máquina 3, procederemos a configurar el balanceador de http. En mi caso he elegido NginX debido a su buen rendimiento, aunque también se puede hacer con HaProxy, Pound, Light o cualquier otro software o servidor.

A continuación, procedo a describir el proceso de instalación de nginx en ubuntu:

        PASOS:
        
            1. Importamos  la clave del repositorio:
            
                cd /tmp/
                wget http://nginx.org/keys/nginx_signing.key
                apt-key add /tmp/nginx_signing.key
                rm -f /tmp/nginx_signing.key
                
                
 ![Practica3](https://dl.dropbox.com/s/rtrxp46jkz6o2m4/pra2.png)
                
                
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
               
                
                
![Practica3](https://dl.dropbox.com/s/uh6l32s1ft6kf2h/pra8%28service%20restart%20nginx%29.png)
                
                
                
            5. Lanzamos el servicio nginx:
                
                service nginx restart
                
            6. Hacemos en la anfitriona curl seguido de la ip de nuestra máquina balanceadora:
                
                curl http://192.168.246.129  



Finalmente, obtendremos en el terminal el balanceo de una maquina y de otra como se muestra en la siguiente captura a modo de prueba de que funciona toda la configuración realizada anteriormente.
También podemos abrir nuestro navegador, introduciendo la IP de la máquina balanceadora y nos mostrara nuestra aplicación.



![Practica3](https://dl.dropbox.com/s/zxtq1mf2mqiosdz/pra10%28fin%20de%20balanceo%20de%20carga%29.png)




Podemos observar como va balanceando la carga en el terminal cuando al meter la IP del balanceador nos va saliendo el contenido de nuestras dos maquinas servidoras.




## CENTOS

#### MÁQUINA 4: CentOS 6.5 - CentOS 1
Descripción: 2 procesador / 256 MB RAM

#### MÁQUINA 5: CentOS 6.5 - CentOS 2
Descripción: 1 procesador / 1024 MB RAM


#### MÁQUINA 6: CentOS 6.5 - CentOS Balanceador
Descripción: 2 procesador / 512 MB RAM


Al igual que ocurre en las máquinas de Ubuntu, las máquinas 4 y 5 al ser las servidoras, mostraran nuestra aplicación y  por lo tanto tenemos que configurarlas en /var/www/ donde encontraremos el fichero index.html el cual sustituiremos por el de nuestra aplicación.

En su configuración, debemos de instalar LAMP, manualmente desde consola Apache + MySQL + PHP e la siguiente forma:

		APACHE:
			yum install httpd
			service httpd start
			chkconfig httpd on
			
			
![Practica3](https://dl.dropbox.com/s/dopj7m95dcioivy/centos1%28apache%20install%29.png)
		

		MySQL:
			yum install mysql-server
			service mysqld start
			chkconfig mysqld on
			/usr/bin/mysql_secure_installation

		PHP:
			yum install php php-mysql
			yum searh php-  (para ver los módulos que tenemos a nuestra disposición)
			nano /var/www/html/aplicacion.php
			service httpd restart
			

Procedemos ahora a configurar nginx en CentOS:

		PASOS:
	
			1. Entramos carpeta temporal para descargar el archivo repositorio de Nginx:
					
					cd /tmp
					wget http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm
					rpm -ivh nginx-release-centos-6-0.el6.ngx.noarch.rpm

			2. Instalamos nginx, iniciamos su servicio y hacemos que suba al inicio:

					yum install nginx
					service nginx start
					chkconfig --levels 35 nginx on

			3. Modificamos el archivo de configuración añadiendole el nombre del servidor y demás igual que hemos hecho en el balanceador de Ubuntu:
		
					nano /etc/nginx/conf.d/default.conf


			4. Finalmente reiniciamos el servicio nginx:

					service nginx restart
					
					

![Practica3](https://dl.dropbox.com/s/1m0d5id89pa5r70/centos%28nginx%20configuracion%29.png)



## APLICACIÓN

#### EN MÁQUINAS UBUNTU

Inicialmente, tenemos nuestro fichero con la aplicacion en nuestra máquina anfitriona, para introducirlo en la virtual y hacerla funcionar ahi basta con abrir vmware seleccionar nuestra máquina, editarla y en el menu de opaciones pulsar "shared forlders" y añadir el nomre y ruta de la carpeta que queremos compartir con la anfitriona como muestra la siguiente captura:


![Practica3](https://dl.dropbox.com/s/pwfcav2bxw7tge2/ubuntu1%28comparte%20carpetas%29.png)


Seguidamente procedemos a iniciar la máquina, instalamos los tools y a partir de ahi seguimos los siguientes pasos:

















#### EN ḾÁQUINAS CENTOS

## BENCHMARK


