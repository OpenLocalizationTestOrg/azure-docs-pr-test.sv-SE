---
title: "aaaSet in PostgreSQL på en Linux-VM | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera PostgreSQL på en Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: 40209647924dffce11500705eb2d9f41c14df6ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a>Installera och konfigurera PostgreSQL på Azure
PostgreSQL är en avancerad databas med öppen källkod liknande tooOracle och DB2. Den innehåller funktioner för enterprise-redo som fullständig ACID kompatibilitet, tillförlitlig transaktionell bearbetning och flera version samtidighetskontroll. Det stöder också standarder, till exempel ANSI SQL och SQL/MED (inklusive externa data omslutningar för Oracle, MySQL, MongoDB och många andra). Det är mycket utökningsbart med stöd för över 12 procedurmässig språk, GIN och GiST index, stöd för spatial data och flera NoSQL-liknande funktioner för JSON- eller key-värde-baserade program.

I den här artikeln får du lära dig hur tooinstall och konfigurera PostgreSQL på en Azure-dator som kör Linux.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>Installera PostgreSQL
> [!NOTE]
> Du måste redan ha en Azure-dator som kör Linux i ordning toocomplete den här kursen. toocreate och skapa en Linux VM innan du fortsätter bör du se den [virtuella Azure Linux-datorn kursen](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

I det här fallet Använd port 1999 som hello PostgreSQL-port.  

Ansluta toohello Linux VM du skapat via PuTTY. Hello första gången du använder en Azure Linux-dator, se [hur tooUse SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn hur toouse PuTTY tooconnect tooa Linux VM.

1. Kör följande kommando tooswitch toohello rot (admin) hello:
   
        # sudo su -
2. Vissa distributioner har beroenden som måste installeras innan du installerar PostgreSQL. Kontrollera om din distro i listan och kör lämplig hello-kommando:
   
   * Red Hat grundläggande Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian grundläggande Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Hämta PostgreSQL i hello rotkatalog och sedan packa hello paketet:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    hello ovan är ett exempel. Du kan hitta hello mer detaljerad hämta adressen i hello [Index/pub/källa /](https://ftp.postgresql.org/pub/source/).
4. toostart hello build köra följande kommandon:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Om du vill toobuild allt som kan byggas, inklusive hello dokumentation (HTML och man sidor) och ytterligare moduler (contrib), kör du följande kommando i stället hello:
   
        # gmake install-world
   
    Du bör få hello följande bekräftelsemeddelande:
   
        PostgreSQL, contrib, and documentation successfully made. Ready tooinstall.

## <a name="configure-postgresql"></a>Konfigurera PostgreSQL
1. (Valfritt) Skapa en symbolisk länk tooshorten hello PostgreSQL referens toonot inkluderar hello versionsnumret:
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. Skapa en katalog för hello databasen:
   
        # mkdir -p /opt/pgsql_data
3. Skapa en rotanvändare och ändra användarens profil. Växla sedan toothis ny användare (kallas *postgres* i vårt exempel):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Av säkerhetsskäl PostgreSQL använder en icke-rotanvändaren tooinitialize, starta eller stänga av hello-databasen.
   > 
   > 
4. Redigera hello *bash_profile* filen genom att ange hello-kommandona nedan. Dessa rader läggs toohello slutet av hello *bash_profile* fil:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Köra hello *bash_profile* fil:
   
        $ source .bash_profile
6. Verifiera installationen genom att använda hello följande kommando:
   
        $ which psql
   
    Om installationen lyckas visas följande svar hello:
   
        /opt/pgsql/bin/psql
7. Du kan också kontrollera hello PostgreSQL-version:
   
        $ psql -V
8. Initiera hello databasen:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Du bör få hello följande utdata:

![Bild](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Ställ in PostgreSQL
<!--    [postgres@ test ~]$ exit -->

Kör följande kommandon hello:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Ändra två variabler i hello /etc/init.d/postgresql-filen. hello prefix anges toohello installationssökväg för PostgreSQL: **/opt/pgsql**. PGDATA anges toohello lagring datasökväg för PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bild](./media/postgresql-install/no2.png)

Ändra hello filen toomake den körbara:

    # chmod +x /etc/init.d/postgresql

Starta PostgreSQL:

    # /etc/init.d/postgresql start

Kontrollera om hello slutpunkten för PostgreSQL finns på:

    # netstat -tunlp|grep 1999

Du bör se hello följande utdata:

![Bild](./media/postgresql-install/no3.png)

## <a name="connect-toohello-postgres-database"></a>Ansluta toohello Postgres databas
Växla toohello postgres användaren igen:

    # su - postgres

Skapa en Postgres databas:

    $ createdb events

Anslut toohello händelser databasen som du just skapat:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Skapa och ta bort en Postgres tabell
Nu när du har anslutit toohello databasen kan skapa du tabeller i den.

Till exempel skapa en ny exempel Postgres tabell med hjälp av hello följande kommando:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Du har nu konfigurerat en tabell med fyra kolumner med hello följande kolumnnamn och begränsningar:

1. Hej ”name” kolumn har varit begränsade av hello VARCHAR kommandot toobe under 20 tecken.
2. Hej ”mat” kolumn indikerar hello mat objekt som kommer att få varje person. VARCHAR begränsar den här texten toobe under 30 tecken.
3. Hej bekräftad ”” kolumn poster om hello person har svarat toohello knytkalas. hello godkända värden är ”Y” och ”N”.
4. Hej ”dag” kolumnen visas när de registrerar sig för hello-händelse. Postgres kräver att skrivas datum som åååå-mm-dd.

Om din tabell har skapats bör du se hello följande:

![Bild](./media/postgresql-install/no4.png)

Du kan också kontrollera hello tabellstrukturen med hjälp av hello följande kommando:

![Bild](./media/postgresql-install/no5.png)

### <a name="add-data-tooa-table"></a>Lägg till data tooa tabell
Börja lägga till information i en rad:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Du bör se dessa utdata:

![Bild](./media/postgresql-install/no6.png)

Du kan lägga till några fler personer toohello tabell. Här följer några alternativ eller skapa egna:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Visa tabeller
Använd följande kommando tooshow en tabell hello:

    select * from potluck;

hello-utdata är:

![Bild](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Ta bort data i en tabell
Använd följande kommando toodelete data i en tabell hello:

    delete from potluck where name=’John’;

Detta tar bort alla hello informationen i hello ”John” rad. hello-utdata är:

![Bild](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Uppdatera data i en tabell
Använd hello följande kommando tooupdate data i en tabell. För den här Sandy har bekräftat att hon deltar, så vi ändrar sin RSVP från ”N” för ”Y”:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Mer information om PostgreSQL
Nu när du har slutfört hello installation av PostgreSQL i en Azure Linux-dator kan få du med i Azure. toolearn mer om PostgreSQL, besök hello [PostgreSQL webbplats](http://www.postgresql.org/).

