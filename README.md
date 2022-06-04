EJECUCIÓN DE DOS EJERCICIOS-DEVOPS

# EJERCICIO TERRAFORM
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Antes de iniciar hay que tener instalado lo siguiente en la maquina host:
    *[Terraform v0.12.0]
    *[provider.digitalocean v1.3.0]

# Clone this repository

git clone git@github.com:dgerardomartinez/examenp1-devops.git

# Token Digital Ocean y SSH KEY

Para provisionar aplicaciones y cración de droplets se debe tener una cuenta en Digital Ocean y generar el TOKEN: https://cloud.digitalocean.com/account/api/tokens 

Para generar una key ssh en el host debemos crearla con el siguiente comando:

    1. ssh-keygen y guardamos en una carpeta con un nombre especifico (C:\Users\DenisG Martinez/.ssh/id_rsa)
    2. Con el comando cat ~/.ssh/id_rsa.pub podremos ver la key en nuestro host después agregar la ssh key en Digital Ocean : https://cloud.digitalocean.com/account/security?i=33f45d se agregar una key.pub.


-Se utilizará un archivo de variables vagrant.tfvars (Está en el repositorio):

do_token = "[TOKEN]"
ssh_key_private = "~/.ssh/id_rsa"
droplet_ssh_key_id = "[ID SSH KEY]"
droplet_name = "web"
droplet_size = "s-1vcpu-1gb"
droplet_region = "nyc1"

-Para ver todas las claves ssh disponibles en la cuenta:
doctl  -t [TOKEN] compute ssh-key list

# Ejecutar Terraform

terraform plan (Para ver lo que se ejecutará en el código)
terraform apply (Para aplicar la configuración y cración de recursos en Digital Ocean)
terraform destroy (Para eliminar todos los recursos que se ejecutaron)

Ingresamos en un navegador http://ip_server y tendrá que mostrar el blog de wordpress.

# EJERCICIO EN VAGRANT - DOCKER SWARM - VMWARE
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Antes de iniciar hay que tener instalado lo siguiente en la maquina host:
    *[Vagrant instalado]
    *[Vagrant Plugins = vagrant-docker-compose]
    *[Vagrant plugin license vagrant-vmware-desktop]

# Clone this repository

git clone git@github.com:dgerardomartinez/examenp1-devops.git

# Nota IMPORTANTE (Se utilizaron estos tres SCRIPT para automatizar la tareas de configuración)

# (PARA INSTALAR DOCKER Y CONFIGURAR)
#script installation and processes server´s 
$install_docker_script = <<SCRIPT
echo Installing Docker...
curl -sSL https://get.docker.com/ | sh
SCRIPT

# (PARA AGREGAR UN MANAGER WORKER_TOKEN Y SE PUEDAN CONECTAR LOS WORKER´S)
$manager_script = <<SCRIPT 
echo Swarm Init...
docker swarm init --listen-addr 10.20.100.50:2377 --advertise-addr 10.20.100.50:2377
docker swarm join-token --quiet worker > /vagrant/worker_token
SCRIPT

# (PARA QUE TODOS LOS WORKER PUEDAN REFERIRSE AL IP DEL MANAGER)
$worker_script = <<SCRIPT 
echo Swarm Join...
docker swarm join --token $(cat /vagrant/worker_token) 10.20.100.50:2377
SCRIPT

# Ejecutar Vagrant
$ vagrant up

# Lista de servicios creados
$ docker service ls

# Lista de conteiners ejecutados en swarm
docker service ps <service-name>

# Para borrar toda la configuración e implementación
vagran destroy









