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
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>Konfigurera SSL-anslutning i Azure-databas för PostgreSQL
Azure-databas för PostgreSQL föredrar ansluta din klient program toohello PostgreSQL-tjänsten med hjälp av Secure Sockets Layer (SSL). Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.

Hej PostgreSQL database-tjänsten är som standard konfigurerade toorequire SSL-anslutning. Alternativt kan inaktivera du kräver SSL för att ansluta tooyour databastjänsten om klientprogrammet inte har stöd för SSL-anslutning. 

## <a name="enforcing-ssl-connections"></a>Att framtvinga SSL-anslutningar
Tillämpning av SSL-anslutningar är aktiverad som standard för alla Azure-databas för PostgreSQL-servrar som etablerats via hello Azure-portalen och CLI. 

På samma sätt kan inkludera anslutningssträngar som redan har definierats i hello ”anslutningssträngar” inställningar under din server i hello Azure-portalen hello krävs parametrar för vanliga språk tooconnect tooyour databasservern med SSL. hello SSL parametern varierar beroende på hello koppling, till exempel ”ssl = true” eller ”sslmode = kräver” eller ”sslmode = krävs” eller andra ändringar.

## <a name="configure-enforcement-of-ssl"></a>Konfigurera tvingande av SSL
Alternativt kan du inaktivera att framtvinga SSL-anslutning. Microsoft Azure rekommenderar tooalways aktiverar **framtvinga SSL-anslutning** för ökad säkerhet.

### <a name="using-hello-azure-portal"></a>Med hjälp av hello Azure-portalen
Besök din Azure-databas för PostgreSQL-servern och på **anslutningssäkerhet**. Använd hello växla knappen tooenable eller inaktivera hello **framtvinga SSL-anslutning** inställningen. Klicka sedan på **Spara**. 

![Anslutningssäkerhet - inaktivera framtvinga SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Du kan bekräfta hello inställningen genom att visa hello **översikt** sidan toosee hello **SSL genomdriva status** indikatorn.

### <a name="using-azure-cli"></a>Använda Azure CLI
Du kan aktivera eller inaktivera hello **ssl tvingande** parameter med hjälp av `Enabled` eller `Disabled` värden i Azure CLI.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Se till att ditt program eller framework stöder SSL-anslutningar
Många vanliga ramverk för programmet som använder PostgreSQL för databastjänster, till exempel Drupal och Django, aktiverar inte SSL som standard under installationen. Aktivera SSL-anslutning måste göras efter installationen eller via CLI-kommandon specifika toohello program. Om servern PostgreSQL tvingar SSL-anslutningar och hello associerade program är inte korrekt konfigurerad, fungerar hello program inte tooconnect tooyour databasserver. Kontakta ditt programs dokumentationen toolearn hur tooenable SSL-anslutningar.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Program som kräver certifikatverifiering för SSL-anslutning
I vissa fall kan kräver program en lokal certifikatfil som genereras från en betrodd certifikatutfärdare (CA) certifikat filen (.cer) tooconnect på ett säkert sätt. Se hello följande steg tooobtain hello .cer-fil, avkoda hello certifikat och bind det tooyour program.

### <a name="download-hello-certificate-file-from-hello-certificate-authority-ca"></a>Hämta hello certifikatfilen från hello certifikatutfärdare (CA) 
hello certifikat behövs toocommunicate via SSL med din Azure-databas för PostgreSQL-servern är belägen [här](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Hämta hello certifikatfilen lokalt.

### <a name="download-and-install-openssl-on-your-machine"></a>Hämta och installera OpenSSL på din dator 
toodecode hello certifikatfil krävs för ditt program tooconnect på ett säkert sätt tooyour databasservern måste tooinstall OpenSSL på den lokala datorn.

#### <a name="for-linux-os-x-or-unix"></a>För Linux, OS X eller Unix
Hej OpenSSL-bibliotek finns i källkoden direkt från hello [OpenSSL Software Foundation](http://www.openssl.org). hello följande instruktionerna leder dig igenom hello steg nödvändiga tooinstall OpenSSL på Linux-datorn. Den här artikeln använder kommandon kända toowork på Ubuntu 12.04 och högre.

Öppna en terminalsession och installera OpenSSL
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Extrahera hello filer från hello-paketet
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Ange hello katalogen där hello filerna har extraherats. Som standard ska det vara på följande sätt.

```bash
cd openssl-1.1.0e
```
Konfigurera OpenSSL genom att köra följande kommando hello. Om du vill hello filer i en mapp skiljer sig /usr/local/openssl, kontrollera att toochange hello följande efter behov.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Nu när OpenSSL är korrekt konfigurerad, måste toocompile den tooconvert ditt certifikat. Kör följande kommando hello toocompile:

```bash
make
```
När kompilering är klar kan är du klar tooinstall OpenSSL som en körbar fil genom att köra följande kommando hello:
```bash
make install
```
tooconfirm som du har installerat OpenSSL på datorn som kör hello följande kommando och kontrollera att du får hello toomake samma utdata.

```bash
/usr/local/openssl/bin/openssl version
```
Du bör se hello efter meddelande om det lyckas.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>För Windows
Installera OpenSSL på en Windows-dator kan göras i hello följande sätt:
1. **(Rekommenderas)**  Använder hello inbyggda Bash för Windows-funktioner i Windows 10 och senare, OpenSSL installeras som standard. Anvisningar om hur du kan hitta tooenable Bash för Windows-funktioner i Windows 10 [här](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. Genom att hämta ett Win32/64-program som tillhandahålls av hello community. Medan hello OpenSSL Software Foundation inte stöder eller stödja en specifik Windows-installationsprogram, ger en lista över tillgängliga installationsprogram [här](https://wiki.openssl.org/index.php/Binaries)

### <a name="decode-your-certificate-file"></a>Avkoda certifikatfil
hello ned rot-CA som är i krypterat format. Använda OpenSSL toodecode hello-certifikatfil. toodo kör så OpenSSL kommandot:

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-tooazure-database-for-postgresql-with-ssl-certificate-authentication"></a>Anslutande tooAzure för PostgreSQL-databas med SSL-certifikat-autentisering
Nu när du har har avkodas certifikatet, kan du nu ansluta tooyour databasserver på ett säkert sätt via SSL. tooallow för certifikatet serververifiering hello certifikatet måste placeras i hello filen ~/.postgresql/root.crt i hello användarens hemkatalog. (På Microsoft Windows hello-filen heter % APPDATA%\postgresql\root.crt.). hello följande innehåller instruktioner för att ansluta tooAzure databas för PostgreSQL.

> [!NOTE]
> För närvarande finns ett känt problem om du använder ”sslmode = Kontrollera fullständig” i toohello anslutningstjänst, hello anslutningen misslyckas med hello följande fel: _servercertifikat för ”&lt;region&gt;. Control.Database.Windows.NET ”(och 7 namn) inte matchar värdnamnet”&lt;servername&gt;. postgres.database.azure.com ”._
> Om ”sslmode = Kontrollera fullständig” är krävs, Använd hello Namngivningskonventionen för servrar  **&lt;servername&gt;. database.windows.net** som din värdnamn för anslutningssträngen. Vi planerar tooremove den här begränsningen i hello framtida. Anslutningar med andra [SSL lägen](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) bör fortsätta toouse hello önskade värden namngivningskonvention  **&lt;servername&gt;. postgres.database.azure.com**.

#### <a name="using-psql-command-line-utility"></a>Med hjälp av kommandoradsverktyget psql
hello som följande exempel visar hur toosuccessfully ansluter tooyour PostgreSQL-servern med hjälp av kommandoradsverktyget för hello psql. Använd hello `root.crt` fil som har skapats och hello `sslmode=verify-ca` eller `sslmode=verify-full` alternativet.

Med hello PostgreSQL kommandoradsgränssnitt, kör hello följande kommando:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Om detta lyckas visas hello följande utdata:
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

#### <a name="using-pgadmin-gui-tool"></a>Verktyget pgAdmin GUI
Konfigurera pgAdmin 4 tooconnect på ett säkert sätt via SSL kräver tooset hello `SSL mode = Verify-CA` eller `SSL mode = Verify-Full` på följande sätt:

![Skärmbild av pgAdmin - anslutning – SSL-läge kräver](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Nästa steg
Granska olika anslutningsalternativ för programmet efter [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)
