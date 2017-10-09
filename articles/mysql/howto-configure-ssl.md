---
title: "toosecurely för aaaConfigure SSL-anslutning ansluta tooAzure databas för MySQL | Microsoft Docs"
description: "Instruktioner för hur tooproperly konfigurera Azure-databas för MySQL och associerade program toocorrectly använda SSL-anslutningar"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 07/28/2017
ms.openlocfilehash: 8c37c19d4c101abfb730f429a19441e94e52fc85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ssl-connectivity-in-your-application-toosecurely-connect-tooazure-database-for-mysql"></a>Konfigurera SSL anslutningen i ditt program toosecurely connect tooAzure databas för MySQL
Azure-databas för MySQL stöder anslutning av din Azure-databas för MySQL tooclient serverprogram med hjälp av Secure Sockets Layer (SSL). Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.

## <a name="step-1-obtain-ssl-certificate"></a>Steg 1: Få SSL-certifikat
Hämta hello certifikat som behövs toocommunicate via SSL med din Azure-databas för MySQL-servern från [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) och spara hello certifikat filen tooyour lokala enhet (med den här kursen används vi c:\ssl).
**För Microsoft Internet Explorer och Microsoft Edge:** när hello har hämtats, byta namn på hello certifikat tooBaltimoreCyberTrustRoot.crt.pem.

## <a name="step-2-bind-ssl"></a>Steg 2: Binda SSL
### <a name="connecting-tooserver-using-hello-mysql-workbench-over-ssl"></a>Ansluta tooserver med hello MySQL arbetsstationen via SSL
Konfigurera MySQL-arbetsstationen tooconnect på ett säkert sätt via SSL. Navigera toohello **SSL** fliken i hello MySQL-arbetsstationen på hello konfigurera nya Connection dialog. Ange hello filplatsen för hello **BaltimoreCyberTrustRoot.crt.pem** i hello **SSL CA-fil:** fält.
![Spara anpassade sida vid sida](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-tooserver-using-hello-mysql-cli-over-ssl"></a>Ansluta tooserver med hello MySQL CLI via SSL
Med hello MySQL kommandoradsgränssnitt, kör hello följande kommando:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Steg 3: Framtvinga SSL-anslutningar i Azure 
### <a name="using-azure-portal"></a>Använda Azure Portal
Med hjälp av hello Azure-portalen finns din Azure-databas för MySQL-servern och klickar på **anslutningssäkerhet**. Använd hello växla knappen tooenable eller inaktivera hello **framtvinga SSL-anslutning** inställningen. Klicka sedan på **Spara**. Microsoft rekommenderar tooalways aktiverar **framtvinga SSL-anslutning** för ökad säkerhet.
![Aktivera ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Använda Azure CLI
Du kan aktivera eller inaktivera hello **ssl tvingande** parametern med aktiverad eller inaktiverad värden respektive i Azure CLI.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a>Steg 4: Verifiera SSL-anslutning
Köra hello mysql **status** kommandot tooverify att du har anslutit tooyour MySQL-servern med SSL:
```dos
mysql> status
```
Bekräfta hello anslutningen är krypterad genom att granska hello utdata. Du bör se: **SSL: Cipher används är AES256 SHA** 

## <a name="sample-code"></a>Exempelkod
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a>Python
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a>Nästa steg
Granska olika anslutningsalternativ för programmet efter [anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)
