
1. tooescalate privilegier, typ:
   
        sudo -s
   
    Ange ditt lösenord
2. tooinstall MySQL Community Server edition, skriver du:
   
        zypper install mysql-community-server
   
    Vänta medan MySQL hämtas och installeras.
3. tooset MySQL toostart när hello systemet startas, typ:
   
        insserv mysql
4. Starta hello MySQL-daemon (mysqld) manuellt med det här kommandot:
   
        rcmysql start
   
    toocheck hello status för hello MySQL daemon, skriver du:
   
        rcmysql status
   
    toostop hello MySQL daemon, skriver du:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Hello MySQL rotlösenordet är tomt som standard efter installationen. Vi rekommenderar att du kör **mysql\_säker\_installation**, ett skript som hjälper till att säkra MySQL. hello ombeds du toochange hello MySQL rotlösenordet, ta bort anonyma användarkonton, inaktivera fjärråtkomst rot-inloggningar, ta bort databaser för test och Läs in hello privilegier tabell. Vi rekommenderar att du svara Ja tooall av dessa alternativ och ändra hello rotlösenordet.
   > 
   > 
5. Skriv toorun hello skriptet MySQL installationsskriptet:
   
        mysql_secure_installation
6. Logga in tooMySQL:
   
        mysql -u root -p
   
    Ange rotlösenordet för hello MySQL (som du har ändrat i hello föregående steg) och visas med en uppmaning där du kan utfärda SQL-instruktioner toointeract med hello-databas.
7. toocreate en ny MySQL-användare kör hello följande på hello **mysql >** prompten:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Observera hello semikolon (;) hello slutet av hello raderna är avgörande för slutar hello-kommandon.
8. toocreate en databas och bevilja hello `mysqluser` behörigheter på den problemet hello följande kommandon:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* too'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Observera att databasen användarnamn och lösenord används endast av skript som ansluter toohello databas.  Databasen användarkontonamn representerar inte nödvändigtvis faktiska användarkonton på hello system.
9. toolog i från en annan dator, typ:
   
        GRANT ALL ON testdatabase.* too'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    där `ip-address` är hello hello datorns som ansluter du tooMySQL IP-adress.
10. tooexit hello MySQL-databas Administrationsverktyg för skriver du:
    
        quit

## <a name="add-an-endpoint"></a>Lägga till en slutpunkt
1. När MySQL installeras, måste tooconfigure en slutpunkt tooaccess MySQL på distans. Logga in toohello [klassiska Azure-portalen][AzurePortal]. Klicka på **virtuella datorer**hello namnet på den nya virtuella datorn och klicka sedan på **slutpunkter**.
2. Klicka på **Lägg till** på hello hello sidans nederkant.
3. Lägga till en slutpunkt med namnet ”MySQL” med protokollet **TCP**, och **offentliga** och **privata** portarna set för ”3306”.
4. tooremotely Anslut toohello virtuell dator från datorn, typ:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Till exempel använda hello virtuell dator som vi skapade i den här kursen, ange följande kommando:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
