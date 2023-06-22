# Pasos para instalar driver SQL server en Ubuntu!

Primero visitar la documentación oficial en ms para la instalación del driver: [aquí](https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver15&tabs=ubuntu18-install,alpine17-install,debian8-install,redhat7-13-install,rhel7-offline#ubuntu18)

Por motivos de seguridad hacemos un **copy & paste** del artículo:

    sudo su 
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - 
    curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list > /etc/apt/sources.list.d/mssql-release.list
    
    sudo apt-get update 
    sudo ACCEPT_EULA=Y apt-get install -y msodbcsql18
    # optional: for bcp and sqlcmd 
    sudo ACCEPT_EULA=Y apt-get install -y mssql-tools18
    echo  'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc 
    source ~/.bashrc
    # optional: for unixODBC development headers 
    sudo apt-get install -y unixodbc-dev
    
Una vez instalado hacemos la prueba con la url de conexión

    isql -v -k "DRIVER=ODBC Driver 18 for SQL Server;SERVER=ip_server;DATABASE=name_db;UID=user_id;PWD=password;TrustServerCertificate=yes;"

 - DRIVER: debe llevar el nombre del driverSQL que se instaló
 -  TrustServerCertificate=yes; es necesario para aceptar comunicaciones TSL

**Importante:** una vez instaladas las nuevas dependencias, al momento de hacer el update *sudo apt-update*, es seguro que se obtenga la advertencia: 

  ``W: Key is stored in legacy trusted.gpg keyring``
  
Solo es una advertencia y no obstruye el proceso.

