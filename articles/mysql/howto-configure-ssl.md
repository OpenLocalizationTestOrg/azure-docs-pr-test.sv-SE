---
title: "Konfigurera SSL-anslutning för att ansluta säkert till Azure-databas för MySQL | Microsoft Docs"
description: "Instruktioner för hur du ska konfigurera Azure-databas för MySQL och associerade program använder SSL-anslutningar"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 07/28/2017
ms.openlocfilehash: 77e1b6266a2cf47949fa06358ec003f6b6b38065
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Konfigurera SSL-anslutning i ditt program för att ansluta säkert till Azure-databas för MySQL
Azure-databas för MySQL stöder anslutning av din Azure-databas för MySQL-servern till klientprogram som använder Secure Sockets Layer (SSL). Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man i mitten” genom att kryptera dataströmmen mellan servern och ditt program.

## <a name="step-1-obtain-ssl-certificate"></a>Steg 1: Få SSL-certifikat
Hämta certifikat som behövs för att kommunicera via SSL med din Azure-databas för MySQL-servern från [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) och spara filen med certifikat till din lokala enhet (med den här självstudiekursen kommer vi använde c:\ssl).
**För Microsoft Internet Explorer och Microsoft Edge:** när hämtningen är slutförd, ändra certifikatet till BaltimoreCyberTrustRoot.crt.pem.

## <a name="step-2-bind-ssl"></a>Steg 2: Binda SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Ansluta till servern med hjälp av MySQL-arbetsstationen via SSL
Konfigurera MySQL-arbetsstationen för att ansluta på ett säkert sätt via SSL. Navigera till den **SSL** fliken i MySQL-arbetsstationen dialogen installera ny anslutning. Ange sökvägen till den **BaltimoreCyberTrustRoot.crt.pem** i den **SSL CA-fil:** fält.
![Spara anpassade sida vid sida](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Ansluter till servern med hjälp av MySQL-CLI via SSL
Genom att använda MySQL-kommandoradsgränssnitt, kör du följande kommando:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Steg 3: Framtvinga SSL-anslutningar i Azure 
### <a name="using-azure-portal"></a>Använda Azure Portal
Med hjälp av Azure portal finns din Azure-databas för MySQL-servern och klickar på **anslutningssäkerhet**. Använd växlingsknappen för att aktivera eller inaktivera den **framtvinga SSL-anslutning** inställningen. Klicka sedan på **Spara**. Microsoft rekommenderar att du alltid aktivera **framtvinga SSL-anslutning** för ökad säkerhet.
![Aktivera ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Använda Azure CLI
Du kan aktivera eller inaktivera den **ssl tvingande** parametern med aktiverad eller inaktiverad värden respektive i Azure CLI.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a>Steg 4: Verifiera SSL-anslutning
Köra mysql **status** kommando för att kontrollera att du har anslutit till MySQL-servern med hjälp av SSL:
```dos
mysql> status
```
Bekräfta anslutningen är krypterad genom att granska utdata. Du bör se: **SSL: Cipher används är AES256 SHA** 

## <a name="sample-code"></a>Exempelkod
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
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
