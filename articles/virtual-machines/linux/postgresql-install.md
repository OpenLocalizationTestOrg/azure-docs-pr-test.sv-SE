---
title: "Ställ in PostgreSQL på en Linux-VM | Microsoft Docs"
description: "Lär dig hur du installerar och konfigurerar PostgreSQL på en Linux-dator i Azure"
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
ms.openlocfilehash: 0bccdc1cfdbda06b57da8cd662373ef137768672
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a><span data-ttu-id="af461-103">Installera och konfigurera PostgreSQL på Azure</span><span class="sxs-lookup"><span data-stu-id="af461-103">Install and configure PostgreSQL on Azure</span></span>
<span data-ttu-id="af461-104">PostgreSQL är en avancerad liknande Oracle och DB2 databas med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="af461-104">PostgreSQL is an advanced open-source database similar to Oracle and DB2.</span></span> <span data-ttu-id="af461-105">Den innehåller funktioner för enterprise-redo som fullständig ACID kompatibilitet, tillförlitlig transaktionell bearbetning och flera version samtidighetskontroll.</span><span class="sxs-lookup"><span data-stu-id="af461-105">It includes enterprise-ready features such as full ACID compliance, reliable transactional processing, and multi-version concurrency control.</span></span> <span data-ttu-id="af461-106">Det stöder också standarder, till exempel ANSI SQL och SQL/MED (inklusive externa data omslutningar för Oracle, MySQL, MongoDB och många andra).</span><span class="sxs-lookup"><span data-stu-id="af461-106">It also supports standards such as ANSI SQL and SQL/MED (including foreign data wrappers for Oracle, MySQL, MongoDB, and many others).</span></span> <span data-ttu-id="af461-107">Det är mycket utökningsbart med stöd för över 12 procedurmässig språk, GIN och GiST index, stöd för spatial data och flera NoSQL-liknande funktioner för JSON- eller key-värde-baserade program.</span><span class="sxs-lookup"><span data-stu-id="af461-107">It is highly extensible with support for over 12 procedural languages, GIN and GiST indexes, spatial data support, and multiple NoSQL-like features for JSON or key-value-based applications.</span></span>

<span data-ttu-id="af461-108">I den här artikeln får du lära dig hur du installerar och konfigurerar PostgreSQL på en Azure-dator som kör Linux.</span><span class="sxs-lookup"><span data-stu-id="af461-108">In this article, you will learn how to install and configure PostgreSQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a><span data-ttu-id="af461-109">Installera PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="af461-109">Install PostgreSQL</span></span>
> [!NOTE]
> <span data-ttu-id="af461-110">Du måste redan ha en Azure-dator som kör Linux för att kunna slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="af461-110">You must already have an Azure virtual machine running Linux in order to complete this tutorial.</span></span> <span data-ttu-id="af461-111">Om du vill skapa och konfigurera en Linux-VM innan du fortsätter, finns det [virtuella Azure Linux-datorn kursen](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="af461-111">To create and set up a Linux VM before proceeding, see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

<span data-ttu-id="af461-112">I det här fallet Använd port 1999 som PostgreSQL-port.</span><span class="sxs-lookup"><span data-stu-id="af461-112">In this case, use port 1999 as the PostgreSQL port.</span></span>  

<span data-ttu-id="af461-113">Ansluta till Linux VM som du skapade via PuTTY.</span><span class="sxs-lookup"><span data-stu-id="af461-113">Connect to the Linux VM you created via PuTTY.</span></span> <span data-ttu-id="af461-114">Om du använder en Azure Linux VM Se [hur du använder SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) information om hur du använder PuTTY för att ansluta till en Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="af461-114">If this is the first time you're using an Azure Linux VM, see [How to Use SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to learn how to use PuTTY to connect to a Linux VM.</span></span>

1. <span data-ttu-id="af461-115">Kör följande kommando för att växla till roten (admin):</span><span class="sxs-lookup"><span data-stu-id="af461-115">Run the following command to switch to the root (admin):</span></span>
   
        # sudo su -
2. <span data-ttu-id="af461-116">Vissa distributioner har beroenden som måste installeras innan du installerar PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="af461-116">Some distributions have dependencies that you must install before installing PostgreSQL.</span></span> <span data-ttu-id="af461-117">Kontrollera om din distro i listan och köra ett kommando:</span><span class="sxs-lookup"><span data-stu-id="af461-117">Check for your distro in this list and run the appropriate command:</span></span>
   
   * <span data-ttu-id="af461-118">Red Hat grundläggande Linux:</span><span class="sxs-lookup"><span data-stu-id="af461-118">Red Hat base Linux:</span></span>
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="af461-119">Debian grundläggande Linux:</span><span class="sxs-lookup"><span data-stu-id="af461-119">Debian base Linux:</span></span>
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="af461-120">SUSE Linux:</span><span class="sxs-lookup"><span data-stu-id="af461-120">SUSE Linux:</span></span>
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. <span data-ttu-id="af461-121">Hämta PostgreSQL i rotkatalogen och packa upp paketet:</span><span class="sxs-lookup"><span data-stu-id="af461-121">Download PostgreSQL into the root directory, and then unzip the package:</span></span>
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    <span data-ttu-id="af461-122">Detta är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="af461-122">The above is an example.</span></span> <span data-ttu-id="af461-123">Du kan hitta mer detaljerad hämta adressen i den [Index/pub/källa /](https://ftp.postgresql.org/pub/source/).</span><span class="sxs-lookup"><span data-stu-id="af461-123">You can find the more detailed download address in the [Index of /pub/source/](https://ftp.postgresql.org/pub/source/).</span></span>
4. <span data-ttu-id="af461-124">Du börjar bygga genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="af461-124">To start the build, run these commands:</span></span>
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. <span data-ttu-id="af461-125">Om du vill skapa allt som kan byggas, inklusive dokumentation (HTML och man sidor) och ytterligare moduler (contrib), kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af461-125">If  you want to build everything that can be built, including the documentation (HTML and man pages) and additional modules (contrib), run the following command instead:</span></span>
   
        # gmake install-world
   
    <span data-ttu-id="af461-126">Du får följande bekräftelsemeddelande:</span><span class="sxs-lookup"><span data-stu-id="af461-126">You should receive the following confirmation message:</span></span>
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a><span data-ttu-id="af461-127">Konfigurera PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="af461-127">Configure PostgreSQL</span></span>
1. <span data-ttu-id="af461-128">(Valfritt) Skapa en symbolisk länk för att förkorta PostgreSQL-referens för att inte inkludera versionsnumret:</span><span class="sxs-lookup"><span data-stu-id="af461-128">(Optional) Create a symbolic link to shorten the PostgreSQL reference to not include the version number:</span></span>
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. <span data-ttu-id="af461-129">Skapa en katalog för databasen:</span><span class="sxs-lookup"><span data-stu-id="af461-129">Create a directory for the database:</span></span>
   
        # mkdir -p /opt/pgsql_data
3. <span data-ttu-id="af461-130">Skapa en rotanvändare och ändra användarens profil.</span><span class="sxs-lookup"><span data-stu-id="af461-130">Create a non-root user and modify that user’s profile.</span></span> <span data-ttu-id="af461-131">Växla sedan till den nya användaren (kallas *postgres* i vårt exempel):</span><span class="sxs-lookup"><span data-stu-id="af461-131">Then, switch to this new user (called *postgres* in our example):</span></span>
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > <span data-ttu-id="af461-132">Av säkerhetsskäl använder en rotanvändare PostgreSQL för att initiera, starta eller stänga av databasen.</span><span class="sxs-lookup"><span data-stu-id="af461-132">For security reasons, PostgreSQL uses a non-root user to initialize, start, or shut down the database.</span></span>
   > 
   > 
4. <span data-ttu-id="af461-133">Redigera den *bash_profile* filen genom att ange nedanstående kommandon.</span><span class="sxs-lookup"><span data-stu-id="af461-133">Edit the *bash_profile* file by entering the commands below.</span></span> <span data-ttu-id="af461-134">Dessa rader läggs till i slutet av den *bash_profile* fil:</span><span class="sxs-lookup"><span data-stu-id="af461-134">These lines will be added to the end of the *bash_profile* file:</span></span>
   
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
5. <span data-ttu-id="af461-135">Köra den *bash_profile* fil:</span><span class="sxs-lookup"><span data-stu-id="af461-135">Execute the *bash_profile* file:</span></span>
   
        $ source .bash_profile
6. <span data-ttu-id="af461-136">Verifiera installationen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af461-136">Validate your installation by using the following command:</span></span>
   
        $ which psql
   
    <span data-ttu-id="af461-137">Om installationen lyckas visas följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="af461-137">If your installation is successful, you will see the following response:</span></span>
   
        /opt/pgsql/bin/psql
7. <span data-ttu-id="af461-138">Du kan också kontrollera PostgreSQL-version:</span><span class="sxs-lookup"><span data-stu-id="af461-138">You can also check the PostgreSQL version:</span></span>
   
        $ psql -V
8. <span data-ttu-id="af461-139">Initiera databasen:</span><span class="sxs-lookup"><span data-stu-id="af461-139">Initialize the database:</span></span>
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    <span data-ttu-id="af461-140">Du bör få följande utdata:</span><span class="sxs-lookup"><span data-stu-id="af461-140">You should receive the following output:</span></span>

![Bild](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a><span data-ttu-id="af461-142">Ställ in PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="af461-142">Set up PostgreSQL</span></span>
<!--    [postgres@ test ~]$ exit -->

<span data-ttu-id="af461-143">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="af461-143">Run the following commands:</span></span>

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

<span data-ttu-id="af461-144">Ändra två variabler i filen /etc/init.d/postgresql.</span><span class="sxs-lookup"><span data-stu-id="af461-144">Modify two variables in the /etc/init.d/postgresql file.</span></span> <span data-ttu-id="af461-145">Prefixet är inställd på installationssökvägen för PostgreSQL: **/opt/pgsql**.</span><span class="sxs-lookup"><span data-stu-id="af461-145">The prefix is set to the installation path of PostgreSQL: **/opt/pgsql**.</span></span> <span data-ttu-id="af461-146">PGDATA är inställd på datasökväg för lagring av PostgreSQL: **/opt/pgsql_data**.</span><span class="sxs-lookup"><span data-stu-id="af461-146">PGDATA is set to the data storage path of PostgreSQL: **/opt/pgsql_data**.</span></span>

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bild](./media/postgresql-install/no2.png)

<span data-ttu-id="af461-148">Ändra så att den körbara filen:</span><span class="sxs-lookup"><span data-stu-id="af461-148">Change the file to make it executable:</span></span>

    # chmod +x /etc/init.d/postgresql

<span data-ttu-id="af461-149">Starta PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="af461-149">Start PostgreSQL:</span></span>

    # /etc/init.d/postgresql start

<span data-ttu-id="af461-150">Kontrollera om slutpunkten för PostgreSQL finns på:</span><span class="sxs-lookup"><span data-stu-id="af461-150">Check if the endpoint of PostgreSQL is on:</span></span>

    # netstat -tunlp|grep 1999

<span data-ttu-id="af461-151">Du bör se följande utdata:</span><span class="sxs-lookup"><span data-stu-id="af461-151">You should see the following output:</span></span>

![Bild](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a><span data-ttu-id="af461-153">Ansluta till databasen Postgres</span><span class="sxs-lookup"><span data-stu-id="af461-153">Connect to the Postgres database</span></span>
<span data-ttu-id="af461-154">Växla till postgres användaren igen:</span><span class="sxs-lookup"><span data-stu-id="af461-154">Switch to the postgres user once again:</span></span>

    # su - postgres

<span data-ttu-id="af461-155">Skapa en Postgres databas:</span><span class="sxs-lookup"><span data-stu-id="af461-155">Create a Postgres database:</span></span>

    $ createdb events

<span data-ttu-id="af461-156">Anslut till databasen händelser som du just har skapat:</span><span class="sxs-lookup"><span data-stu-id="af461-156">Connect to the events database that you just created:</span></span>

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a><span data-ttu-id="af461-157">Skapa och ta bort en Postgres tabell</span><span class="sxs-lookup"><span data-stu-id="af461-157">Create and delete a Postgres table</span></span>
<span data-ttu-id="af461-158">Nu när du har anslutit till databasen kan skapa du tabeller i den.</span><span class="sxs-lookup"><span data-stu-id="af461-158">Now that you have connected to the database, you can create tables in it.</span></span>

<span data-ttu-id="af461-159">Till exempel skapa en ny exempel Postgres tabell med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af461-159">For example, create a new example Postgres table by using the following command:</span></span>

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

<span data-ttu-id="af461-160">Du har nu konfigurerat en tabell med fyra kolumner med följande kolumnnamn och begränsningar:</span><span class="sxs-lookup"><span data-stu-id="af461-160">You have now set up a four-column table with the following column names and restrictions:</span></span>

1. <span data-ttu-id="af461-161">Kolumnen ”namn” begränsade av kommandot VARCHAR ska vara mindre än 20 tecken.</span><span class="sxs-lookup"><span data-stu-id="af461-161">The “name” column has been limited by the VARCHAR command to be under 20 characters long.</span></span>
2. <span data-ttu-id="af461-162">Kolumnen ”mat” anger mat-objekt som kommer att få varje person.</span><span class="sxs-lookup"><span data-stu-id="af461-162">The “food” column indicates the food item that each person will bring.</span></span> <span data-ttu-id="af461-163">VARCHAR begränsar texten för att vara under 30 tecken.</span><span class="sxs-lookup"><span data-stu-id="af461-163">VARCHAR limits this text to be under 30 characters.</span></span>
3. <span data-ttu-id="af461-164">Kolumnen ”bekräftad” innehåller information om personen som har svarat på knytkalas.</span><span class="sxs-lookup"><span data-stu-id="af461-164">The “confirmed” column records whether the person has RSVP’d to the potluck.</span></span> <span data-ttu-id="af461-165">Godkända värden är ”Y” och ”N”.</span><span class="sxs-lookup"><span data-stu-id="af461-165">The acceptable values are "Y" and "N".</span></span>
4. <span data-ttu-id="af461-166">I ”dag” kolumnen visas när de registrerar sig för händelsen.</span><span class="sxs-lookup"><span data-stu-id="af461-166">The “date” column shows when they signed up for the event.</span></span> <span data-ttu-id="af461-167">Postgres kräver att skrivas datum som åååå-mm-dd.</span><span class="sxs-lookup"><span data-stu-id="af461-167">Postgres requires that dates be written as yyyy-mm-dd.</span></span>

<span data-ttu-id="af461-168">Om din tabell har skapats bör du se följande:</span><span class="sxs-lookup"><span data-stu-id="af461-168">You should see the following if your table has been successfully created:</span></span>

![Bild](./media/postgresql-install/no4.png)

<span data-ttu-id="af461-170">Du kan också kontrollera tabellstrukturen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af461-170">You can also check the table structure by using the following command:</span></span>

![Bild](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a><span data-ttu-id="af461-172">Lägg till data i en tabell</span><span class="sxs-lookup"><span data-stu-id="af461-172">Add data to a table</span></span>
<span data-ttu-id="af461-173">Börja lägga till information i en rad:</span><span class="sxs-lookup"><span data-stu-id="af461-173">First, insert information into a row:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

<span data-ttu-id="af461-174">Du bör se dessa utdata:</span><span class="sxs-lookup"><span data-stu-id="af461-174">You should see this output:</span></span>

![Bild](./media/postgresql-install/no6.png)

<span data-ttu-id="af461-176">Du kan lägga till några fler personer tabellen.</span><span class="sxs-lookup"><span data-stu-id="af461-176">You can add a couple more people to the table as well.</span></span> <span data-ttu-id="af461-177">Här följer några alternativ eller skapa egna:</span><span class="sxs-lookup"><span data-stu-id="af461-177">Here are some options, or you can create your own:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a><span data-ttu-id="af461-178">Visa tabeller</span><span class="sxs-lookup"><span data-stu-id="af461-178">Show tables</span></span>
<span data-ttu-id="af461-179">Använd följande kommando om du vill visa en tabell:</span><span class="sxs-lookup"><span data-stu-id="af461-179">Use the following command to show a table:</span></span>

    select * from potluck;

<span data-ttu-id="af461-180">Utdata är:</span><span class="sxs-lookup"><span data-stu-id="af461-180">The output is:</span></span>

![Bild](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a><span data-ttu-id="af461-182">Ta bort data i en tabell</span><span class="sxs-lookup"><span data-stu-id="af461-182">Delete data in a table</span></span>
<span data-ttu-id="af461-183">Använd följande kommando för att ta bort data i en tabell:</span><span class="sxs-lookup"><span data-stu-id="af461-183">Use the following command to delete data in a table:</span></span>

    delete from potluck where name=’John’;

<span data-ttu-id="af461-184">Detta tar bort all information på raden ”John”.</span><span class="sxs-lookup"><span data-stu-id="af461-184">This deletes all the information in the "John" row.</span></span> <span data-ttu-id="af461-185">Utdata är:</span><span class="sxs-lookup"><span data-stu-id="af461-185">The output is:</span></span>

![Bild](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a><span data-ttu-id="af461-187">Uppdatera data i en tabell</span><span class="sxs-lookup"><span data-stu-id="af461-187">Update data in a table</span></span>
<span data-ttu-id="af461-188">Använd följande kommando för att uppdatera data i en tabell.</span><span class="sxs-lookup"><span data-stu-id="af461-188">Use the following command to update data in a table.</span></span> <span data-ttu-id="af461-189">För den här har Sandy bekräftat att hon deltar, så vi ändrar sin RSVP från ”N” till ”Y”:</span><span class="sxs-lookup"><span data-stu-id="af461-189">For this one, Sandy has confirmed that she is attending, so we will change her RSVP from "N" to "Y":</span></span>

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a><span data-ttu-id="af461-190">Mer information om PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="af461-190">Get more information about PostgreSQL</span></span>
<span data-ttu-id="af461-191">Nu när du har slutfört installationen av PostgreSQL i en Azure Linux-dator kan få du med i Azure.</span><span class="sxs-lookup"><span data-stu-id="af461-191">Now that you have completed the installation of PostgreSQL in an Azure Linux VM, you can enjoy using it in Azure.</span></span> <span data-ttu-id="af461-192">Mer information om PostgreSQL finns i [PostgreSQL webbplats](http://www.postgresql.org/).</span><span class="sxs-lookup"><span data-stu-id="af461-192">To learn more about PostgreSQL, visit the [PostgreSQL website](http://www.postgresql.org/).</span></span>

