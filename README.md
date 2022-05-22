# INSTALACION DE ODOO15 EN UN ENTORNO VIRTUAL PYTHON EN UBUNTU 20.04
Este tutorial cubre la instalación e implementación de Odoo 15 dentro de un entorno virtual Python en Ubuntu 20.04. Descargaremos Odoo del repositorio oficial de GitHub y usaremos Nginx como proxy inverso.
## 1. Instalar dependencias
El primer paso es instalar Git , Pip , Node.js y las herramientas necesarias para compilar Odoo:

**_sudo apt update_**

**_sudo apt install git python3-pip build-essential wget python3-dev python3-venv \
    python3-wheel libfreetype6-dev libxml2-dev libzip-dev libldap2-dev libsasl2-dev \
    python3-setuptools node-less libjpeg-dev zlib1g-dev libpq-dev \
    libxslt1-dev libldap2-dev libtiff5-dev libjpeg8-dev libopenjp2-7-dev \
    liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev_**
    
## 2. Crea un usuario del sistema
Ejecutar Odoo como root presenta un gran riesgo de seguridad. Crearemos un nuevo usuario y grupo del sistema con el directorio de inicio /opt/odoo15 que ejecutará el servicio Odoo. Para hacer esto, ejecute el siguiente comando:

**_sudo useradd -m -d /opt/odoo15 -U -r -s /bin/bash odoo15_**

Puede nombrar al usuario como desee, siempre que cree un usuario de PostgreSQL con el mismo nombre.

## 3. Instalar y configurar PostgreSQL
Odoo usa PostgreSQL como el backend de la base de datos. PostgreSQL está incluido en los repositorios estándar de Ubuntu. La instalación es sencilla:

**_sudo apt install postgresql_**

Una vez que el servicio esté instalado, cree un usuario de PostgreSQL con el mismo nombre que el usuario del sistema creado anteriormente. En este ejemplo, eso es odoo15:

**_sudo su - postgres -c "createuser -s odoo15"_**

## 4. Instalar wkhtmltopdf
wkhtmltopdf es un conjunto de herramientas de línea de comandos de código abierto para renderizar páginas HTML en PDF y varios formatos de imagen. Para imprimir informes PDF en Odoo, deberá instalar el paquete wkhtmltox.

La versión de wkhtmltopdf incluida en los repositorios de Ubuntu no admite encabezados ni pies de página. La versión recomendada para Odoo es 0.12.5. Descargaremos e instalaremos el paquete desde Github:

## 5. Instalar y configurar Odoo 15
Instalaremos Odoo desde la fuente dentro de un entorno virtual de Python aislado.
Primero, cambie al usuario "odoo15":

**_sudo su - odoo15_**

Clona el código fuente de Odoo 15 desde GitHub:

**_git clone https://www.github.com/odoo/odoo --depth 1 --branch 15.0 /opt/odoo15/odoo_**

Cree un nuevo entorno virtual de Python para Odoo:

**_cd /opt/odoo15_**

**_python3 -m venv odoo-venv_**

Activar el entorno virtual:

**_source odoo-venv/bin/activate_**

Las dependencias de Odoo se especifican en el archivo require.txt. Instale todos los módulos de Python necesarios con pip3:

**_pip3 install wheel_**

**_pip3 install -r odoo/requirements.txt_**

Una vez hecho esto, desactive el entorno escribiendo:

**_deactivate_**

Cree un nuevo directorio, un directorio separado para los complementos de terceros:

**_mkdir /opt/odoo15/odoo-custom-addons_**

Luego agregaremos este directorio al parámetro addons_path. Este parámetro define una lista de directorios en la que Odoo busca módulos.

Regrese a su usuario de sudo:

**_exit_**
