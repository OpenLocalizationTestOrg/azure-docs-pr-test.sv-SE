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
# <a name="how-tooconnect-applications-tooazure-database-for-mysql"></a><span data-ttu-id="78e61-103">Hur tooconnect program tooAzure databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="78e61-103">How tooconnect applications tooAzure Database for MySQL</span></span>
<span data-ttu-id="78e61-104">Det här dokumentet innehåller hello sträng anslutningstyper som stöds av Azure-databas för MySQL, tillsammans med mallar och exempel.</span><span class="sxs-lookup"><span data-stu-id="78e61-104">This document lists hello connection string types that are supported by Azure Database for MySQL, together with templates and examples.</span></span> <span data-ttu-id="78e61-105">Du kan ha olika parametrar och andra inställningar i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="78e61-105">You might have different parameters and different settings in your connection string.</span></span>

- <span data-ttu-id="78e61-106">tooobtain hello certifikat, se [hur tooconfigure SSL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="78e61-106">tooobtain hello certificate, see [How tooconfigure SSL](./howto-configure-ssl.md).</span></span>
- <span data-ttu-id="78e61-107">{your_host} = <servername>. mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="78e61-107">{your_host} = <servername>.mysql.database.azure.com</span></span>
- <span data-ttu-id="78e61-108">{your_user}@{servername} = användar-ID-format för autentisering korrekt.</span><span class="sxs-lookup"><span data-stu-id="78e61-108">{your_user}@{servername} = userID format for authentication correctly.</span></span>  <span data-ttu-id="78e61-109">Hello autentisering toofail genereras om du använder bara hello användar-ID.</span><span class="sxs-lookup"><span data-stu-id="78e61-109">Using just hello userID will cause hello authentication toofail.</span></span>

## <a name="adonet"></a><span data-ttu-id="78e61-110">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="78e61-110">ADO.NET</span></span>
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

<span data-ttu-id="78e61-111">I det här exemplet hello servernamnet är `myserver4demo`, databasnamnet är `wpdb`, användarnamnet är `WPAdmin`, och lösenordet är `mypassword!2`.</span><span class="sxs-lookup"><span data-stu-id="78e61-111">In this example, hello server name is `myserver4demo`, database name is `wpdb`, user name is `WPAdmin`, and password is `mypassword!2`.</span></span> <span data-ttu-id="78e61-112">Sedan ska hello anslutningssträngen vara:</span><span class="sxs-lookup"><span data-stu-id="78e61-112">Then, hello connection string should be:</span></span>

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a><span data-ttu-id="78e61-113">JDBC</span><span class="sxs-lookup"><span data-stu-id="78e61-113">JDBC</span></span>
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a><span data-ttu-id="78e61-114">Node.js</span><span class="sxs-lookup"><span data-stu-id="78e61-114">Node.js</span></span>
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a><span data-ttu-id="78e61-115">ODBC</span><span class="sxs-lookup"><span data-stu-id="78e61-115">ODBC</span></span>
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a><span data-ttu-id="78e61-116">PHP</span><span class="sxs-lookup"><span data-stu-id="78e61-116">PHP</span></span>
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a><span data-ttu-id="78e61-117">Python</span><span class="sxs-lookup"><span data-stu-id="78e61-117">Python</span></span>
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a><span data-ttu-id="78e61-118">Ruby</span><span class="sxs-lookup"><span data-stu-id="78e61-118">Ruby</span></span>
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-hello-connection-string-details-from-hello-azure-portal"></a><span data-ttu-id="78e61-119">Hämta hello anslutning sträng från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="78e61-119">Get hello connection string details from hello Azure portal</span></span>
<span data-ttu-id="78e61-120">I hello [Azure-portalen](https://portal.azure.com)gå tooyour Azure-databas för MySQL-server och klicka sedan på **anslutningssträngar** tooget i strängen lista för din instans: ![hello anslutning strängar rutan i hello Azure-portalen](./media/howto-connection-strings/connection-strings-on-portal.png)</span><span class="sxs-lookup"><span data-stu-id="78e61-120">In hello [Azure portal](https://portal.azure.com), go tooyour Azure Database for MySQL server, and then click **Connection strings** tooget your string list for your instance: ![hello Connection strings pane in hello Azure portal](./media/howto-connection-strings/connection-strings-on-portal.png)</span></span>

<span data-ttu-id="78e61-121">hello-sträng innehåller information som hello drivrutin, server och andra parametrar för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="78e61-121">hello string provides details such as hello driver, server, and other database connection parameters.</span></span> <span data-ttu-id="78e61-122">Ändra de här exemplen med dina egna parametrar, till exempel databasens namn, lösenord och så vidare.</span><span class="sxs-lookup"><span data-stu-id="78e61-122">Modify these examples by using your own parameters, such as database name, password, and so on.</span></span> <span data-ttu-id="78e61-123">Du kan sedan använda den här strängen tooconnect toohello servern från din kod och program.</span><span class="sxs-lookup"><span data-stu-id="78e61-123">You can then use this string tooconnect toohello server from your code and applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78e61-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78e61-124">Next steps</span></span>
- <span data-ttu-id="78e61-125">Läs mer om anslutningsbibliotek [begrepp - anslutningsbibliotek](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="78e61-125">For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).</span></span>
