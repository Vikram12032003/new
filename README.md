ssh -i [keyname.pem] [OSname]@[PublicIP_of_EC2Server]
sudo apt update
sudo apt upgrade -y
sudo apt install -y openjdk-17-jdk
java -version
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" /etc/apt/sources.list.d/pgdg.list'
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
sudo apt install postgresql postgresql-contrib -y
sudo systemctl enable postgresql
sudo systemctl start postgresql
sudo systemctl status postgresql
psql –version
sudo -i -u postgres
createuser ddsonar
psql
ALTER USER [Created_user_name] WITH ENCRYPTED password 'my_strong_password';
    ALTER USER ddsonar WITH ENCRYPTED password 'mwd#2%#!!#%rgs';
CREATE DATABASE [database_name] OWNER [Created_user_name];
CREATE DATABASE ddsonarqube OWNER ddsonar;
GRANT ALL PRIVILEGES ON DATABASE ddsonarqube to ddsonar;
\l
\du
\q
Exit
sudo apt install zip -y
https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.0.0.68432.zip
sudo unzip sonarqube-10.0.0.68432.zip
sudo mv sonarqube-10.0.0.68432 sonarqube
sudo mv sonarqube /opt/
sudo groupadd ddsonar


sudo useradd -d /opt/sonarqube -g ddsonar ddsonar
sudo chown ddsonar:ddsonar /opt/sonarqube -R
sudo nano /opt/sonarqube/conf/sonar.properties
    	sonar.jdbc.username=ddsonar
     	sonar.jdbc.password=mwd#2%#!!#%rgs
 c) Below these two lines, add the following line of code.
      sonar.jdbc.url=jdbc:postgresql://localhost:5432/ddsonarqube
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
	RUN_AS_USER=ddsonar
sudo nano /etc/systemd/system/sonar.service
              [Unit]
              Description=SonarQube service
              After=syslog.target network.target
              [Service]
              Type=forking
              ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
              ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
              User=ddsonar
              Group=ddsonar
              Restart=always
              LimitNOFILE=65536
              LimitNPROC=4096
              [Install]
              WantedBy=multi-user.target
sudo systemctl enable sonar
sudo systemctl start sonar
sudo systemctl status sonar

sudo nano /etc/sysctl.conf
        vm.max_map_count=262144
        fs.file-max=65536
        ulimit -n 65536
        ulimit -u 4096
sudo reboot

wget https://raw.githubusercontent.com/akshu20791/Deployment-script/refs/heads/main/jenkins.sh

wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.115/bin/apache-tomcat-9.0.115.zip
