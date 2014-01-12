` Practica 2 Copyright (C) 2013 María Victoria Santiago Alcalá. This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version. This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details. You should have received a copy of the GNU General Public License along with this program. If not, see .`

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
          Centos Balanceador: máquina balanceadora con nginx instalado.


Con ello, lo que hacemos es configurar una red entre las dos maquinas virtuales servidoras de cada distribución de teniendo un balanceador que reparta la carga entre los servidores, terminando con el posible problema de sobrecarga de estos servidores.
El fin es balancear nuestros servidores http tras configurarlos como explicaré a continuación en la sección de `MAQUINAS VIRTUALES`, consiguiendo con ello que la infraestructura tenga una alta disponibilidad.

Finalmente probraremos la eficiencia de las diferentes configuraciones de las máquinas, comprobando el rendimiento de las mismas tanto en Ubuntu como en CentOS.

## MÁQUINAS VIRTUALES

A continuación, realizo una breve descripción de las máquinas virtuales que he creado, dandole a cada una memoria RAM y un número de prodesadores determinado y distinto a las demás:

### MÁQUINA 1: Ubuntu Server 12.04 - Ubuntu 1



### MÁQUINA 2: Ubuntu Server 12.04 - Ubuntu 2



### MÁQUINA 3: Ubuntu Server 12.04 - Ubuntu Balanceador



### MÁQUINA 4: CentOS 6.5 - CentOS 1


### MÁQUINA 5: CentOS 6.5 - CentOS 2



### MÁQUINA 6: CentOS 6.5 - CentOS Balanceador


## BENCHMARK


