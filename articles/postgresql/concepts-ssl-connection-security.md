---
title: "aaaConfigure SSL-anslutning i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Anvisningar och information tooconfigure Azure-databas för PostgreSQL och associerade program tooproperly använda SSL-anslutningar."
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 05/15/2017
ms.openlocfilehash: 96a68088acd924196701e8d618d9d5edf44cb548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a><span data-ttu-id="30445-103">Konfigurera SSL-anslutning i Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="30445-103">Configure SSL connectivity in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="30445-104">Azure-databas för PostgreSQL föredrar ansluta din klient program toohello PostgreSQL-tjänsten med hjälp av Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="30445-104">Azure Database for PostgreSQL prefers connecting your client applications toohello PostgreSQL service using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="30445-105">Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.</span><span class="sxs-lookup"><span data-stu-id="30445-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

<span data-ttu-id="30445-106">Hej PostgreSQL database-tjänsten är som standard konfigurerade toorequire SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="30445-106">By default, hello PostgreSQL database service is configured toorequire SSL connection.</span></span> <span data-ttu-id="30445-107">Alternativt kan inaktivera du kräver SSL för att ansluta tooyour databastjänsten om klientprogrammet inte har stöd för SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="30445-107">Optionally, you can disable requiring SSL for connecting tooyour database service if your client application does not support SSL connectivity.</span></span> 

## <a name="enforcing-ssl-connections"></a><span data-ttu-id="30445-108">Att framtvinga SSL-anslutningar</span><span class="sxs-lookup"><span data-stu-id="30445-108">Enforcing SSL connections</span></span>
<span data-ttu-id="30445-109">Tillämpning av SSL-anslutningar är aktiverad som standard för alla Azure-databas för PostgreSQL-servrar som etablerats via hello Azure-portalen och CLI.</span><span class="sxs-lookup"><span data-stu-id="30445-109">For all Azure Database for PostgreSQL servers provisioned through hello Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="30445-110">På samma sätt kan inkludera anslutningssträngar som redan har definierats i hello ”anslutningssträngar” inställningar under din server i hello Azure-portalen hello krävs parametrar för vanliga språk tooconnect tooyour databasservern med SSL.</span><span class="sxs-lookup"><span data-stu-id="30445-110">Likewise, connection strings that are pre-defined in hello "Connection Strings" settings under your server in hello Azure portal include hello required parameters for common languages tooconnect tooyour database server using SSL.</span></span> <span data-ttu-id="30445-111">hello SSL parametern varierar beroende på hello koppling, till exempel ”ssl = true” eller ”sslmode = kräver” eller ”sslmode = krävs” eller andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="30445-111">hello SSL parameter varies based on hello connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

## <a name="configure-enforcement-of-ssl"></a><span data-ttu-id="30445-112">Konfigurera tvingande av SSL</span><span class="sxs-lookup"><span data-stu-id="30445-112">Configure Enforcement of SSL</span></span>
<span data-ttu-id="30445-113">Alternativt kan du inaktivera att framtvinga SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="30445-113">You can optionally disable enforcing SSL connectivity.</span></span> <span data-ttu-id="30445-114">Microsoft Azure rekommenderar tooalways aktiverar **framtvinga SSL-anslutning** för ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="30445-114">Microsoft Azure recommends tooalways enable **Enforce SSL connection** setting for enhanced security.</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="30445-115">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="30445-115">Using hello Azure portal</span></span>
<span data-ttu-id="30445-116">Besök din Azure-databas för PostgreSQL-servern och på **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="30445-116">Visit your Azure Database for PostgreSQL server and click **Connection security**.</span></span> <span data-ttu-id="30445-117">Använd hello växla knappen tooenable eller inaktivera hello **framtvinga SSL-anslutning** inställningen.</span><span class="sxs-lookup"><span data-stu-id="30445-117">Use hello toggle button tooenable or disable hello **Enforce SSL connection** setting.</span></span> <span data-ttu-id="30445-118">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="30445-118">Then click **Save**.</span></span> 

![Anslutningssäkerhet - inaktivera framtvinga SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

<span data-ttu-id="30445-120">Du kan bekräfta hello inställningen genom att visa hello **översikt** sidan toosee hello **SSL genomdriva status** indikatorn.</span><span class="sxs-lookup"><span data-stu-id="30445-120">You can confirm hello setting by viewing hello **Overview** page toosee hello **SSL enforce status** indicator.</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="30445-121">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="30445-121">Using Azure CLI</span></span>
<span data-ttu-id="30445-122">Du kan aktivera eller inaktivera hello **ssl tvingande** parameter med hjälp av `Enabled` eller `Disabled` värden i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="30445-122">You can enable or disable hello **ssl-enforcement** parameter using `Enabled` or `Disabled` values respectively in Azure CLI.</span></span>

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a><span data-ttu-id="30445-123">Se till att ditt program eller framework stöder SSL-anslutningar</span><span class="sxs-lookup"><span data-stu-id="30445-123">Ensure your application or framework supports SSL connections</span></span>
<span data-ttu-id="30445-124">Många vanliga ramverk för programmet som använder PostgreSQL för databastjänster, till exempel Drupal och Django, aktiverar inte SSL som standard under installationen.</span><span class="sxs-lookup"><span data-stu-id="30445-124">Many common application frameworks that use PostgreSQL for database services, such as Drupal and Django, do not enable SSL by default during installation.</span></span> <span data-ttu-id="30445-125">Aktivera SSL-anslutning måste göras efter installationen eller via CLI-kommandon specifika toohello program.</span><span class="sxs-lookup"><span data-stu-id="30445-125">Enabling SSL connectivity must be done after installation or through CLI commands specific toohello application.</span></span> <span data-ttu-id="30445-126">Om servern PostgreSQL tvingar SSL-anslutningar och hello associerade program är inte korrekt konfigurerad, fungerar hello program inte tooconnect tooyour databasserver.</span><span class="sxs-lookup"><span data-stu-id="30445-126">If your PostgreSQL server is enforcing SSL connections and hello associated application is not configured properly, hello application may fail tooconnect tooyour database server.</span></span> <span data-ttu-id="30445-127">Kontakta ditt programs dokumentationen toolearn hur tooenable SSL-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="30445-127">Consult your application's documentation toolearn how tooenable SSL connections.</span></span>


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a><span data-ttu-id="30445-128">Program som kräver certifikatverifiering för SSL-anslutning</span><span class="sxs-lookup"><span data-stu-id="30445-128">Applications that require certificate verification for SSL connectivity</span></span>
<span data-ttu-id="30445-129">I vissa fall kan kräver program en lokal certifikatfil som genereras från en betrodd certifikatutfärdare (CA) certifikat filen (.cer) tooconnect på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="30445-129">In some cases, applications require a local certificate file generated from a trusted Certificate Authority (CA) certificate file (.cer) tooconnect securely.</span></span> <span data-ttu-id="30445-130">Se hello följande steg tooobtain hello .cer-fil, avkoda hello certifikat och bind det tooyour program.</span><span class="sxs-lookup"><span data-stu-id="30445-130">See hello following steps tooobtain hello .cer file, decode hello certificate and bind it tooyour application.</span></span>

### <a name="download-hello-certificate-file-from-hello-certificate-authority-ca"></a><span data-ttu-id="30445-131">Hämta hello certifikatfilen från hello certifikatutfärdare (CA)</span><span class="sxs-lookup"><span data-stu-id="30445-131">Download hello certificate file from hello Certificate Authority (CA)</span></span> 
<span data-ttu-id="30445-132">hello certifikat behövs toocommunicate via SSL med din Azure-databas för PostgreSQL-servern är belägen [här](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span><span class="sxs-lookup"><span data-stu-id="30445-132">hello certificate needed toocommunicate over SSL with your Azure Database for PostgreSQL server is located [here](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span></span> <span data-ttu-id="30445-133">Hämta hello certifikatfilen lokalt.</span><span class="sxs-lookup"><span data-stu-id="30445-133">Download hello certificate file locally.</span></span>

### <a name="download-and-install-openssl-on-your-machine"></a><span data-ttu-id="30445-134">Hämta och installera OpenSSL på din dator</span><span class="sxs-lookup"><span data-stu-id="30445-134">Download and install OpenSSL on your machine</span></span> 
<span data-ttu-id="30445-135">toodecode hello certifikatfil krävs för ditt program tooconnect på ett säkert sätt tooyour databasservern måste tooinstall OpenSSL på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="30445-135">toodecode hello certificate file needed for your application tooconnect securely tooyour database server, you need tooinstall OpenSSL on your local computer.</span></span>

#### <a name="for-linux-os-x-or-unix"></a><span data-ttu-id="30445-136">För Linux, OS X eller Unix</span><span class="sxs-lookup"><span data-stu-id="30445-136">For Linux, OS X, or Unix</span></span>
<span data-ttu-id="30445-137">Hej OpenSSL-bibliotek finns i källkoden direkt från hello [OpenSSL Software Foundation](http://www.openssl.org).</span><span class="sxs-lookup"><span data-stu-id="30445-137">hello OpenSSL libraries are provided in source code directly from hello [OpenSSL Software Foundation](http://www.openssl.org).</span></span> <span data-ttu-id="30445-138">hello följande instruktionerna leder dig igenom hello steg nödvändiga tooinstall OpenSSL på Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="30445-138">hello following instructions guide you through hello steps necessary tooinstall OpenSSL on your Linux PC.</span></span> <span data-ttu-id="30445-139">Den här artikeln använder kommandon kända toowork på Ubuntu 12.04 och högre.</span><span class="sxs-lookup"><span data-stu-id="30445-139">This article uses commands known toowork on Ubuntu 12.04 and higher.</span></span>

<span data-ttu-id="30445-140">Öppna en terminalsession och installera OpenSSL</span><span class="sxs-lookup"><span data-stu-id="30445-140">Open a terminal session and install OpenSSL</span></span>
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
<span data-ttu-id="30445-141">Extrahera hello filer från hello-paketet</span><span class="sxs-lookup"><span data-stu-id="30445-141">Extract hello files from hello download package</span></span>
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
<span data-ttu-id="30445-142">Ange hello katalogen där hello filerna har extraherats.</span><span class="sxs-lookup"><span data-stu-id="30445-142">Enter hello directory where hello files were extracted.</span></span> <span data-ttu-id="30445-143">Som standard ska det vara på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="30445-143">By default, it should be as follows.</span></span>

```bash
cd openssl-1.1.0e
```
<span data-ttu-id="30445-144">Konfigurera OpenSSL genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="30445-144">Configure OpenSSL by executing hello following command.</span></span> <span data-ttu-id="30445-145">Om du vill hello filer i en mapp skiljer sig /usr/local/openssl, kontrollera att toochange hello följande efter behov.</span><span class="sxs-lookup"><span data-stu-id="30445-145">If you want hello files in a folder different than /usr/local/openssl, make sure toochange hello following as appropriate.</span></span>

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
<span data-ttu-id="30445-146">Nu när OpenSSL är korrekt konfigurerad, måste toocompile den tooconvert ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="30445-146">Now that OpenSSL is configured properly, you need toocompile it tooconvert your certificate.</span></span> <span data-ttu-id="30445-147">Kör följande kommando hello toocompile:</span><span class="sxs-lookup"><span data-stu-id="30445-147">toocompile, run hello following command:</span></span>

```bash
make
```
<span data-ttu-id="30445-148">När kompilering är klar kan är du klar tooinstall OpenSSL som en körbar fil genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="30445-148">Once compiling is complete, you're ready tooinstall OpenSSL as an executable by running hello following command:</span></span>
```bash
make install
```
<span data-ttu-id="30445-149">tooconfirm som du har installerat OpenSSL på datorn som kör hello följande kommando och kontrollera att du får hello toomake samma utdata.</span><span class="sxs-lookup"><span data-stu-id="30445-149">tooconfirm that you've successfully installed OpenSSL on your system, run hello following command and check toomake sure you get hello same output.</span></span>

```bash
/usr/local/openssl/bin/openssl version
```
<span data-ttu-id="30445-150">Du bör se hello efter meddelande om det lyckas.</span><span class="sxs-lookup"><span data-stu-id="30445-150">If successful you should see hello following message.</span></span>
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a><span data-ttu-id="30445-151">För Windows</span><span class="sxs-lookup"><span data-stu-id="30445-151">For Windows</span></span>
<span data-ttu-id="30445-152">Installera OpenSSL på en Windows-dator kan göras i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="30445-152">Installing OpenSSL on a Windows PC can be done in hello following ways:</span></span>
1. <span data-ttu-id="30445-153">**(Rekommenderas)**  Använder hello inbyggda Bash för Windows-funktioner i Windows 10 och senare, OpenSSL installeras som standard.</span><span class="sxs-lookup"><span data-stu-id="30445-153">**(Recommended)** Using hello built-in Bash for Windows functionality in Window 10 and above, OpenSSL is installed by default.</span></span> <span data-ttu-id="30445-154">Anvisningar om hur du kan hitta tooenable Bash för Windows-funktioner i Windows 10 [här](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span><span class="sxs-lookup"><span data-stu-id="30445-154">Instructions on how tooenable Bash for Windows functionality in Windows 10 can be found [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span></span>
2. <span data-ttu-id="30445-155">Genom att hämta ett Win32/64-program som tillhandahålls av hello community.</span><span class="sxs-lookup"><span data-stu-id="30445-155">Through downloading a Win32/64 application provided by hello community.</span></span> <span data-ttu-id="30445-156">Medan hello OpenSSL Software Foundation inte stöder eller stödja en specifik Windows-installationsprogram, ger en lista över tillgängliga installationsprogram [här](https://wiki.openssl.org/index.php/Binaries)</span><span class="sxs-lookup"><span data-stu-id="30445-156">While hello OpenSSL Software Foundation does not provide or endorse any specific Windows installers, they provide a list of available installers [here](https://wiki.openssl.org/index.php/Binaries)</span></span>

### <a name="decode-your-certificate-file"></a><span data-ttu-id="30445-157">Avkoda certifikatfil</span><span class="sxs-lookup"><span data-stu-id="30445-157">Decode your certificate file</span></span>
<span data-ttu-id="30445-158">hello ned rot-CA som är i krypterat format.</span><span class="sxs-lookup"><span data-stu-id="30445-158">hello downloaded Root CA file is in encrypted format.</span></span> <span data-ttu-id="30445-159">Använda OpenSSL toodecode hello-certifikatfil.</span><span class="sxs-lookup"><span data-stu-id="30445-159">Use OpenSSL toodecode hello certificate file.</span></span> <span data-ttu-id="30445-160">toodo kör så OpenSSL kommandot:</span><span class="sxs-lookup"><span data-stu-id="30445-160">toodo so, run this OpenSSL command:</span></span>

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-tooazure-database-for-postgresql-with-ssl-certificate-authentication"></a><span data-ttu-id="30445-161">Anslutande tooAzure för PostgreSQL-databas med SSL-certifikat-autentisering</span><span class="sxs-lookup"><span data-stu-id="30445-161">Connecting tooAzure Database for PostgreSQL with SSL certificate authentication</span></span>
<span data-ttu-id="30445-162">Nu när du har har avkodas certifikatet, kan du nu ansluta tooyour databasserver på ett säkert sätt via SSL.</span><span class="sxs-lookup"><span data-stu-id="30445-162">Now that you have successfully decoded your certificate, you can now connect tooyour database server securely over SSL.</span></span> <span data-ttu-id="30445-163">tooallow för certifikatet serververifiering hello certifikatet måste placeras i hello filen ~/.postgresql/root.crt i hello användarens hemkatalog.</span><span class="sxs-lookup"><span data-stu-id="30445-163">tooallow server certificate verification, hello certificate must be placed in hello file ~/.postgresql/root.crt in hello user's home directory.</span></span> <span data-ttu-id="30445-164">(På Microsoft Windows hello-filen heter % APPDATA%\postgresql\root.crt.).</span><span class="sxs-lookup"><span data-stu-id="30445-164">(On Microsoft Windows hello file is named %APPDATA%\postgresql\root.crt.).</span></span> <span data-ttu-id="30445-165">hello följande innehåller instruktioner för att ansluta tooAzure databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="30445-165">hello following provides instructions for connecting tooAzure Database for PostgreSQL.</span></span>

> [!NOTE]
> <span data-ttu-id="30445-166">För närvarande finns ett känt problem om du använder ”sslmode = Kontrollera fullständig” i toohello anslutningstjänst, hello anslutningen misslyckas med hello följande fel: _servercertifikat för ”&lt;region&gt;. Control.Database.Windows.NET ”(och 7 namn) inte matchar värdnamnet”&lt;servername&gt;. postgres.database.azure.com ”._</span><span class="sxs-lookup"><span data-stu-id="30445-166">Currently, there is a known issue if you use "sslmode=verify-full" in your connection toohello service, hello connection will fail with hello following error: _server certificate for "&lt;region&gt;.control.database.windows.net" (and 7 other names) does not match host name "&lt;servername&gt;.postgres.database.azure.com"._</span></span>
> <span data-ttu-id="30445-167">Om ”sslmode = Kontrollera fullständig” är krävs, Använd hello Namngivningskonventionen för servrar  **&lt;servername&gt;. database.windows.net** som din värdnamn för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="30445-167">If "sslmode=verify-full" is required, please use hello server naming convention **&lt;servername&gt;.database.windows.net** as your connection string host name.</span></span> <span data-ttu-id="30445-168">Vi planerar tooremove den här begränsningen i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="30445-168">We plan tooremove this limitation in hello future.</span></span> <span data-ttu-id="30445-169">Anslutningar med andra [SSL lägen](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) bör fortsätta toouse hello önskade värden namngivningskonvention  **&lt;servername&gt;. postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="30445-169">Connections using other [SSL modes](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) should continue toouse hello preferred host naming convention **&lt;servername&gt;.postgres.database.azure.com**.</span></span>

#### <a name="using-psql-command-line-utility"></a><span data-ttu-id="30445-170">Med hjälp av kommandoradsverktyget psql</span><span class="sxs-lookup"><span data-stu-id="30445-170">Using psql command-line utility</span></span>
<span data-ttu-id="30445-171">hello som följande exempel visar hur toosuccessfully ansluter tooyour PostgreSQL-servern med hjälp av kommandoradsverktyget för hello psql.</span><span class="sxs-lookup"><span data-stu-id="30445-171">hello following example shows you how toosuccessfully connect tooyour PostgreSQL server using hello psql command-line utility.</span></span> <span data-ttu-id="30445-172">Använd hello `root.crt` fil som har skapats och hello `sslmode=verify-ca` eller `sslmode=verify-full` alternativet.</span><span class="sxs-lookup"><span data-stu-id="30445-172">Use hello `root.crt` file created and hello `sslmode=verify-ca` or `sslmode=verify-full` option.</span></span>

<span data-ttu-id="30445-173">Med hello PostgreSQL kommandoradsgränssnitt, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="30445-173">Using hello PostgreSQL command-line interface, execute hello following command:</span></span>
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
<span data-ttu-id="30445-174">Om detta lyckas visas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="30445-174">If successful, you receive hello following output:</span></span>
```bash
Password for user mylogin@mypgserver-20170401:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a><span data-ttu-id="30445-175">Verktyget pgAdmin GUI</span><span class="sxs-lookup"><span data-stu-id="30445-175">Using pgAdmin GUI tool</span></span>
<span data-ttu-id="30445-176">Konfigurera pgAdmin 4 tooconnect på ett säkert sätt via SSL kräver tooset hello `SSL mode = Verify-CA` eller `SSL mode = Verify-Full` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="30445-176">Configuring pgAdmin 4 tooconnect securely over SSL requires you tooset hello `SSL mode = Verify-CA` or `SSL mode = Verify-Full` as follows:</span></span>

![Skärmbild av pgAdmin - anslutning – SSL-läge kräver](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a><span data-ttu-id="30445-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30445-178">Next steps</span></span>
<span data-ttu-id="30445-179">Granska olika anslutningsalternativ för programmet efter [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="30445-179">Review various application connectivity options following [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
