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
# <a name="install-and-configure-postgresql-on-azure"></a><span data-ttu-id="0c5a8-103">Installera och konfigurera PostgreSQL på Azure</span><span class="sxs-lookup"><span data-stu-id="0c5a8-103">Install and configure PostgreSQL on Azure</span></span>
<span data-ttu-id="0c5a8-104">PostgreSQL är en avancerad databas med öppen källkod liknande tooOracle och DB2.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-104">PostgreSQL is an advanced open-source database similar tooOracle and DB2.</span></span> <span data-ttu-id="0c5a8-105">Den innehåller funktioner för enterprise-redo som fullständig ACID kompatibilitet, tillförlitlig transaktionell bearbetning och flera version samtidighetskontroll.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-105">It includes enterprise-ready features such as full ACID compliance, reliable transactional processing, and multi-version concurrency control.</span></span> <span data-ttu-id="0c5a8-106">Det stöder också standarder, till exempel ANSI SQL och SQL/MED (inklusive externa data omslutningar för Oracle, MySQL, MongoDB och många andra).</span><span class="sxs-lookup"><span data-stu-id="0c5a8-106">It also supports standards such as ANSI SQL and SQL/MED (including foreign data wrappers for Oracle, MySQL, MongoDB, and many others).</span></span> <span data-ttu-id="0c5a8-107">Det är mycket utökningsbart med stöd för över 12 procedurmässig språk, GIN och GiST index, stöd för spatial data och flera NoSQL-liknande funktioner för JSON- eller key-värde-baserade program.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-107">It is highly extensible with support for over 12 procedural languages, GIN and GiST indexes, spatial data support, and multiple NoSQL-like features for JSON or key-value-based applications.</span></span>

<span data-ttu-id="0c5a8-108">I den här artikeln får du lära dig hur tooinstall och konfigurera PostgreSQL på en Azure-dator som kör Linux.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-108">In this article, you will learn how tooinstall and configure PostgreSQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a><span data-ttu-id="0c5a8-109">Installera PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0c5a8-109">Install PostgreSQL</span></span>
> [!NOTE]
> <span data-ttu-id="0c5a8-110">Du måste redan ha en Azure-dator som kör Linux i ordning toocomplete den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-110">You must already have an Azure virtual machine running Linux in order toocomplete this tutorial.</span></span> <span data-ttu-id="0c5a8-111">toocreate och skapa en Linux VM innan du fortsätter bör du se den [virtuella Azure Linux-datorn kursen](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c5a8-111">toocreate and set up a Linux VM before proceeding, see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

<span data-ttu-id="0c5a8-112">I det här fallet Använd port 1999 som hello PostgreSQL-port.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-112">In this case, use port 1999 as hello PostgreSQL port.</span></span>  

<span data-ttu-id="0c5a8-113">Ansluta toohello Linux VM du skapat via PuTTY.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-113">Connect toohello Linux VM you created via PuTTY.</span></span> <span data-ttu-id="0c5a8-114">Hello första gången du använder en Azure Linux-dator, se [hur tooUse SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn hur toouse PuTTY tooconnect tooa Linux VM.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-114">If this is hello first time you're using an Azure Linux VM, see [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn how toouse PuTTY tooconnect tooa Linux VM.</span></span>

1. <span data-ttu-id="0c5a8-115">Kör följande kommando tooswitch toohello rot (admin) hello:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-115">Run hello following command tooswitch toohello root (admin):</span></span>
   
        # sudo su -
2. <span data-ttu-id="0c5a8-116">Vissa distributioner har beroenden som måste installeras innan du installerar PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-116">Some distributions have dependencies that you must install before installing PostgreSQL.</span></span> <span data-ttu-id="0c5a8-117">Kontrollera om din distro i listan och kör lämplig hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-117">Check for your distro in this list and run hello appropriate command:</span></span>
   
   * <span data-ttu-id="0c5a8-118">Red Hat grundläggande Linux:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-118">Red Hat base Linux:</span></span>
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="0c5a8-119">Debian grundläggande Linux:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-119">Debian base Linux:</span></span>
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="0c5a8-120">SUSE Linux:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-120">SUSE Linux:</span></span>
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. <span data-ttu-id="0c5a8-121">Hämta PostgreSQL i hello rotkatalog och sedan packa hello paketet:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-121">Download PostgreSQL into hello root directory, and then unzip hello package:</span></span>
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    <span data-ttu-id="0c5a8-122">hello ovan är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-122">hello above is an example.</span></span> <span data-ttu-id="0c5a8-123">Du kan hitta hello mer detaljerad hämta adressen i hello [Index/pub/källa /](https://ftp.postgresql.org/pub/source/).</span><span class="sxs-lookup"><span data-stu-id="0c5a8-123">You can find hello more detailed download address in hello [Index of /pub/source/](https://ftp.postgresql.org/pub/source/).</span></span>
4. <span data-ttu-id="0c5a8-124">toostart hello build köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-124">toostart hello build, run these commands:</span></span>
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. <span data-ttu-id="0c5a8-125">Om du vill toobuild allt som kan byggas, inklusive hello dokumentation (HTML och man sidor) och ytterligare moduler (contrib), kör du följande kommando i stället hello:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-125">If  you want toobuild everything that can be built, including hello documentation (HTML and man pages) and additional modules (contrib), run hello following command instead:</span></span>
   
        # gmake install-world
   
    <span data-ttu-id="0c5a8-126">Du bör få hello följande bekräftelsemeddelande:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-126">You should receive hello following confirmation message:</span></span>
   
        PostgreSQL, contrib, and documentation successfully made. Ready tooinstall.

## <a name="configure-postgresql"></a><span data-ttu-id="0c5a8-127">Konfigurera PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0c5a8-127">Configure PostgreSQL</span></span>
1. <span data-ttu-id="0c5a8-128">(Valfritt) Skapa en symbolisk länk tooshorten hello PostgreSQL referens toonot inkluderar hello versionsnumret:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-128">(Optional) Create a symbolic link tooshorten hello PostgreSQL reference toonot include hello version number:</span></span>
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. <span data-ttu-id="0c5a8-129">Skapa en katalog för hello databasen:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-129">Create a directory for hello database:</span></span>
   
        # mkdir -p /opt/pgsql_data
3. <span data-ttu-id="0c5a8-130">Skapa en rotanvändare och ändra användarens profil.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-130">Create a non-root user and modify that user’s profile.</span></span> <span data-ttu-id="0c5a8-131">Växla sedan toothis ny användare (kallas *postgres* i vårt exempel):</span><span class="sxs-lookup"><span data-stu-id="0c5a8-131">Then, switch toothis new user (called *postgres* in our example):</span></span>
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > <span data-ttu-id="0c5a8-132">Av säkerhetsskäl PostgreSQL använder en icke-rotanvändaren tooinitialize, starta eller stänga av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-132">For security reasons, PostgreSQL uses a non-root user tooinitialize, start, or shut down hello database.</span></span>
   > 
   > 
4. <span data-ttu-id="0c5a8-133">Redigera hello *bash_profile* filen genom att ange hello-kommandona nedan.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-133">Edit hello *bash_profile* file by entering hello commands below.</span></span> <span data-ttu-id="0c5a8-134">Dessa rader läggs toohello slutet av hello *bash_profile* fil:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-134">These lines will be added toohello end of hello *bash_profile* file:</span></span>
   
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
5. <span data-ttu-id="0c5a8-135">Köra hello *bash_profile* fil:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-135">Execute hello *bash_profile* file:</span></span>
   
        $ source .bash_profile
6. <span data-ttu-id="0c5a8-136">Verifiera installationen genom att använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-136">Validate your installation by using hello following command:</span></span>
   
        $ which psql
   
    <span data-ttu-id="0c5a8-137">Om installationen lyckas visas följande svar hello:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-137">If your installation is successful, you will see hello following response:</span></span>
   
        /opt/pgsql/bin/psql
7. <span data-ttu-id="0c5a8-138">Du kan också kontrollera hello PostgreSQL-version:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-138">You can also check hello PostgreSQL version:</span></span>
   
        $ psql -V
8. <span data-ttu-id="0c5a8-139">Initiera hello databasen:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-139">Initialize hello database:</span></span>
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    <span data-ttu-id="0c5a8-140">Du bör få hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-140">You should receive hello following output:</span></span>

![Bild](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a><span data-ttu-id="0c5a8-142">Ställ in PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0c5a8-142">Set up PostgreSQL</span></span>
<!--    [postgres@ test ~]$ exit -->

<span data-ttu-id="0c5a8-143">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-143">Run hello following commands:</span></span>

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

<span data-ttu-id="0c5a8-144">Ändra två variabler i hello /etc/init.d/postgresql-filen.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-144">Modify two variables in hello /etc/init.d/postgresql file.</span></span> <span data-ttu-id="0c5a8-145">hello prefix anges toohello installationssökväg för PostgreSQL: **/opt/pgsql**.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-145">hello prefix is set toohello installation path of PostgreSQL: **/opt/pgsql**.</span></span> <span data-ttu-id="0c5a8-146">PGDATA anges toohello lagring datasökväg för PostgreSQL: **/opt/pgsql_data**.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-146">PGDATA is set toohello data storage path of PostgreSQL: **/opt/pgsql_data**.</span></span>

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bild](./media/postgresql-install/no2.png)

<span data-ttu-id="0c5a8-148">Ändra hello filen toomake den körbara:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-148">Change hello file toomake it executable:</span></span>

    # chmod +x /etc/init.d/postgresql

<span data-ttu-id="0c5a8-149">Starta PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-149">Start PostgreSQL:</span></span>

    # /etc/init.d/postgresql start

<span data-ttu-id="0c5a8-150">Kontrollera om hello slutpunkten för PostgreSQL finns på:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-150">Check if hello endpoint of PostgreSQL is on:</span></span>

    # netstat -tunlp|grep 1999

<span data-ttu-id="0c5a8-151">Du bör se hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-151">You should see hello following output:</span></span>

![Bild](./media/postgresql-install/no3.png)

## <a name="connect-toohello-postgres-database"></a><span data-ttu-id="0c5a8-153">Ansluta toohello Postgres databas</span><span class="sxs-lookup"><span data-stu-id="0c5a8-153">Connect toohello Postgres database</span></span>
<span data-ttu-id="0c5a8-154">Växla toohello postgres användaren igen:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-154">Switch toohello postgres user once again:</span></span>

    # su - postgres

<span data-ttu-id="0c5a8-155">Skapa en Postgres databas:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-155">Create a Postgres database:</span></span>

    $ createdb events

<span data-ttu-id="0c5a8-156">Anslut toohello händelser databasen som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-156">Connect toohello events database that you just created:</span></span>

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a><span data-ttu-id="0c5a8-157">Skapa och ta bort en Postgres tabell</span><span class="sxs-lookup"><span data-stu-id="0c5a8-157">Create and delete a Postgres table</span></span>
<span data-ttu-id="0c5a8-158">Nu när du har anslutit toohello databasen kan skapa du tabeller i den.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-158">Now that you have connected toohello database, you can create tables in it.</span></span>

<span data-ttu-id="0c5a8-159">Till exempel skapa en ny exempel Postgres tabell med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-159">For example, create a new example Postgres table by using hello following command:</span></span>

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

<span data-ttu-id="0c5a8-160">Du har nu konfigurerat en tabell med fyra kolumner med hello följande kolumnnamn och begränsningar:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-160">You have now set up a four-column table with hello following column names and restrictions:</span></span>

1. <span data-ttu-id="0c5a8-161">Hej ”name” kolumn har varit begränsade av hello VARCHAR kommandot toobe under 20 tecken.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-161">hello “name” column has been limited by hello VARCHAR command toobe under 20 characters long.</span></span>
2. <span data-ttu-id="0c5a8-162">Hej ”mat” kolumn indikerar hello mat objekt som kommer att få varje person.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-162">hello “food” column indicates hello food item that each person will bring.</span></span> <span data-ttu-id="0c5a8-163">VARCHAR begränsar den här texten toobe under 30 tecken.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-163">VARCHAR limits this text toobe under 30 characters.</span></span>
3. <span data-ttu-id="0c5a8-164">Hej bekräftad ”” kolumn poster om hello person har svarat toohello knytkalas.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-164">hello “confirmed” column records whether hello person has RSVP’d toohello potluck.</span></span> <span data-ttu-id="0c5a8-165">hello godkända värden är ”Y” och ”N”.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-165">hello acceptable values are "Y" and "N".</span></span>
4. <span data-ttu-id="0c5a8-166">Hej ”dag” kolumnen visas när de registrerar sig för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-166">hello “date” column shows when they signed up for hello event.</span></span> <span data-ttu-id="0c5a8-167">Postgres kräver att skrivas datum som åååå-mm-dd.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-167">Postgres requires that dates be written as yyyy-mm-dd.</span></span>

<span data-ttu-id="0c5a8-168">Om din tabell har skapats bör du se hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-168">You should see hello following if your table has been successfully created:</span></span>

![Bild](./media/postgresql-install/no4.png)

<span data-ttu-id="0c5a8-170">Du kan också kontrollera hello tabellstrukturen med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-170">You can also check hello table structure by using hello following command:</span></span>

![Bild](./media/postgresql-install/no5.png)

### <a name="add-data-tooa-table"></a><span data-ttu-id="0c5a8-172">Lägg till data tooa tabell</span><span class="sxs-lookup"><span data-stu-id="0c5a8-172">Add data tooa table</span></span>
<span data-ttu-id="0c5a8-173">Börja lägga till information i en rad:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-173">First, insert information into a row:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

<span data-ttu-id="0c5a8-174">Du bör se dessa utdata:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-174">You should see this output:</span></span>

![Bild](./media/postgresql-install/no6.png)

<span data-ttu-id="0c5a8-176">Du kan lägga till några fler personer toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-176">You can add a couple more people toohello table as well.</span></span> <span data-ttu-id="0c5a8-177">Här följer några alternativ eller skapa egna:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-177">Here are some options, or you can create your own:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a><span data-ttu-id="0c5a8-178">Visa tabeller</span><span class="sxs-lookup"><span data-stu-id="0c5a8-178">Show tables</span></span>
<span data-ttu-id="0c5a8-179">Använd följande kommando tooshow en tabell hello:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-179">Use hello following command tooshow a table:</span></span>

    select * from potluck;

<span data-ttu-id="0c5a8-180">hello-utdata är:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-180">hello output is:</span></span>

![Bild](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a><span data-ttu-id="0c5a8-182">Ta bort data i en tabell</span><span class="sxs-lookup"><span data-stu-id="0c5a8-182">Delete data in a table</span></span>
<span data-ttu-id="0c5a8-183">Använd följande kommando toodelete data i en tabell hello:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-183">Use hello following command toodelete data in a table:</span></span>

    delete from potluck where name=’John’;

<span data-ttu-id="0c5a8-184">Detta tar bort alla hello informationen i hello ”John” rad.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-184">This deletes all hello information in hello "John" row.</span></span> <span data-ttu-id="0c5a8-185">hello-utdata är:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-185">hello output is:</span></span>

![Bild](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a><span data-ttu-id="0c5a8-187">Uppdatera data i en tabell</span><span class="sxs-lookup"><span data-stu-id="0c5a8-187">Update data in a table</span></span>
<span data-ttu-id="0c5a8-188">Använd hello följande kommando tooupdate data i en tabell.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-188">Use hello following command tooupdate data in a table.</span></span> <span data-ttu-id="0c5a8-189">För den här Sandy har bekräftat att hon deltar, så vi ändrar sin RSVP från ”N” för ”Y”:</span><span class="sxs-lookup"><span data-stu-id="0c5a8-189">For this one, Sandy has confirmed that she is attending, so we will change her RSVP from "N" too"Y":</span></span>

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a><span data-ttu-id="0c5a8-190">Mer information om PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0c5a8-190">Get more information about PostgreSQL</span></span>
<span data-ttu-id="0c5a8-191">Nu när du har slutfört hello installation av PostgreSQL i en Azure Linux-dator kan få du med i Azure.</span><span class="sxs-lookup"><span data-stu-id="0c5a8-191">Now that you have completed hello installation of PostgreSQL in an Azure Linux VM, you can enjoy using it in Azure.</span></span> <span data-ttu-id="0c5a8-192">toolearn mer om PostgreSQL, besök hello [PostgreSQL webbplats](http://www.postgresql.org/).</span><span class="sxs-lookup"><span data-stu-id="0c5a8-192">toolearn more about PostgreSQL, visit hello [PostgreSQL website](http://www.postgresql.org/).</span></span>

