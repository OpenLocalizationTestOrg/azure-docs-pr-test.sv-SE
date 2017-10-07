---
title: "aaaConnect program tooAzure för MySQL-databas | Microsoft Docs"
description: "Det här dokumentet innehåller hello stöds för närvarande anslutningssträngar för tooconnect program med Azure-databas för MySQL, inklusive ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python eller Ruby."
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 06/12/2017
ms.openlocfilehash: bbcb2c0ddb4f8e5c225ebef53781e073f5c7b1a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-applications-tooazure-database-for-mysql"></a>Hur tooconnect program tooAzure databas för MySQL
Det här dokumentet innehåller hello sträng anslutningstyper som stöds av Azure-databas för MySQL, tillsammans med mallar och exempel. Du kan ha olika parametrar och andra inställningar i anslutningssträngen.

- tooobtain hello certifikat, se [hur tooconfigure SSL](./howto-configure-ssl.md).
- {your_host} = <servername>. mysql.database.azure.com
- {your_user}@{servername} = användar-ID-format för autentisering korrekt.  Hello autentisering toofail genereras om du använder bara hello användar-ID.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

I det här exemplet hello servernamnet är `myserver4demo`, databasnamnet är `wpdb`, användarnamnet är `WPAdmin`, och lösenordet är `mypassword!2`. Sedan ska hello anslutningssträngen vara:

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-hello-connection-string-details-from-hello-azure-portal"></a>Hämta hello anslutning sträng från hello Azure-portalen
I hello [Azure-portalen](https://portal.azure.com)gå tooyour Azure-databas för MySQL-server och klicka sedan på **anslutningssträngar** tooget i strängen lista för din instans: ![hello anslutning strängar rutan i hello Azure-portalen](./media/howto-connection-strings/connection-strings-on-portal.png)

hello-sträng innehåller information som hello drivrutin, server och andra parametrar för anslutningen. Ändra de här exemplen med dina egna parametrar, till exempel databasens namn, lösenord och så vidare. Du kan sedan använda den här strängen tooconnect toohello servern från din kod och program.

## <a name="next-steps"></a>Nästa steg
- Läs mer om anslutningsbibliotek [begrepp - anslutningsbibliotek](./concepts-connection-libraries.md).
