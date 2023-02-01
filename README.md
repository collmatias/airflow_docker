# airflow_docker
empty airflow runing on docker-compose

Despues de clonar el repo, crear las siguientes carpetas al mismo nivel de la carpeta airflow_docker:

    airflow_docker/
        warehouse-template/
        docker-compose.yaml
        Dockerfile
        ...
    src/
        dags/
        logs/
        plugins/


# Configuracion del Docker en Windows O.S

## 1) Instalar Docker Desktop
Ingresar en la pagina oficial de Docker para descargar e instalar Docker Desktop.

https://www.docker.com/products/docker-desktop/

ES IMPORTANTE EJECUTAR EL INSTALADOR COMO ADMINISTRADOR.

## 2) Instalar docker-compose

Es necesario instalar docker-compose por mas que instale la aplicacion Docker. 

Para instalarlo se ejecutan los siguientes comandos en PowerShell abierto como Administrador:

### > Configurar el TLS
``` [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 ```

### > Descargar e instalar docker-compose
``` Start-BitsTransfer -Source "https://github.com/docker/compose/releases/download/v2.15.1/docker-compose-Windows-x86_64.exe" -Destination $Env:ProgramFiles\Docker\docker-compose.exe ```

### > Comprobar que se haya instalado correctamente
``` docker compose version ```

### > Podes revisar la documentacion de docker en caso de tener alguna duda
https://docs.docker.com/compose/install/other/

## 3) Activar el subsistema y maquina virtual de linux WS2

Ejecutamos los siguientes comandos en una consola PowerShell abierto como Administrador:

### > Activar el subsistema de Linux
``` dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart ```

### > Activar el feature de la maquina virtual
``` dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart ```

### > Setear que por default la version del subsistema sea WSL2 
``` wsl --set-default-version 2 ```

### Podes revisar la documentacion oficial de Microsoft en caso de dudas
https://learn.microsoft.com/es-mx/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package

## 4) Crear el docker

### > Crear el docker en base al docker-compose.yaml

Ejecutar el comando: 

``` docker-compose build ```

En caso de que devuelva un error de permisos o de algun otro tipo, revisar las paginas anteriores y seguir todos los pasos nuevamente.

En caso de querer agregar librerias como los providers a base de datos se deben agregar en la siguiente linea del docker-compose.yaml:
```
 _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:- apache-airflow-providers-microsoft-mssql apache-airflow-providers-oracle apache-airflow-providers-snowflake apache-airflow-providers-apache-spark apache-airflow-providers-amazon apache-airflow-providers-ftp}
```

Las librerias van separadas por un espacio. Todo lo que se ponga ahi va ser instalado automaticamente realizando un pip install.

### Levantar el Docker

Ejecutar el comando: 

``` docker-compose up ```

Listo, ya podes verificarlo en http://localhost:8080/

``` docker-compose up -d ```

Levantar docker sin el flood de logs

# Comandos de Docker
## ```docker ps``` 
Devuelve los contenedores corriendo

## ```docker ps -a``` 
Devuelve todos los contenedores (corriendo y apagados)

## ```docker exec -i -t [id_worker] /bin/bash```
Acceder a la consola de un worker de airflow

## ```docker-compose down```
Apagar el docker para que no se inicie airflow automaticamente al abrir Docker Desktop

## ```docker-compose build```
Vuelve a compilar docker con las modificaciones realizadas en el aarchivo docker-compose.yaml






