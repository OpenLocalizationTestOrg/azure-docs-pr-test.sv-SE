---
title: "Ansluta program till Azure-databas för MySQL | Microsoft Docs"
description: "Det här dokumentet innehåller för närvarande stöds anslutningssträngar att ansluta till Azure-databas för MySQL, inklusive ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python eller Ruby-program."
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 06/12/2017
ms.openlocfilehash: 2f40da41bcfda7e35f6fc63ead5d055246ab390c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a><span data-ttu-id="93e4e-103">Så här ansluter du program till Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="93e4e-103">How to connect applications to Azure Database for MySQL</span></span>
<span data-ttu-id="93e4e-104">Det här dokumentet innehåller strängen anslutningstyper som stöds av Azure-databas för MySQL, tillsammans med mallar och exempel.</span><span class="sxs-lookup"><span data-stu-id="93e4e-104">This document lists the connection string types that are supported by Azure Database for MySQL, together with templates and examples.</span></span> <span data-ttu-id="93e4e-105">Du kan ha olika parametrar och andra inställningar i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="93e4e-105">You might have different parameters and different settings in your connection string.</span></span>

- <span data-ttu-id="93e4e-106">Certifikatet finns på [hur du konfigurerar SSL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="93e4e-106">To obtain the certificate, see [How to configure SSL](./howto-configure-ssl.md).</span></span>
- <span data-ttu-id="93e4e-107">{your_host} = <servername>. mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="93e4e-107">{your_host} = <servername>.mysql.database.azure.com</span></span>
- <span data-ttu-id="93e4e-108">{your_user}@{servername} = användar-ID-format för autentisering korrekt.</span><span class="sxs-lookup"><span data-stu-id="93e4e-108">{your_user}@{servername} = userID format for authentication correctly.</span></span>  <span data-ttu-id="93e4e-109">Med bara userID kommer autentiseringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="93e4e-109">Using just the userID will cause the authentication to fail.</span></span>

## <a name="adonet"></a><span data-ttu-id="93e4e-110">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="93e4e-110">ADO.NET</span></span>
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

<span data-ttu-id="93e4e-111">I det här exemplet servernamnet är `myserver4demo`, databasnamnet är `wpdb`, användarnamnet är `WPAdmin`, och lösenordet är `mypassword!2`.</span><span class="sxs-lookup"><span data-stu-id="93e4e-111">In this example, the server name is `myserver4demo`, database name is `wpdb`, user name is `WPAdmin`, and password is `mypassword!2`.</span></span> <span data-ttu-id="93e4e-112">Sedan bör anslutningssträngen vara:</span><span class="sxs-lookup"><span data-stu-id="93e4e-112">Then, the connection string should be:</span></span>

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a><span data-ttu-id="93e4e-113">JDBC</span><span class="sxs-lookup"><span data-stu-id="93e4e-113">JDBC</span></span>
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a><span data-ttu-id="93e4e-114">Node.js</span><span class="sxs-lookup"><span data-stu-id="93e4e-114">Node.js</span></span>
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a><span data-ttu-id="93e4e-115">ODBC</span><span class="sxs-lookup"><span data-stu-id="93e4e-115">ODBC</span></span>
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a><span data-ttu-id="93e4e-116">PHP</span><span class="sxs-lookup"><span data-stu-id="93e4e-116">PHP</span></span>
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a><span data-ttu-id="93e4e-117">Python</span><span class="sxs-lookup"><span data-stu-id="93e4e-117">Python</span></span>
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a><span data-ttu-id="93e4e-118">Ruby</span><span class="sxs-lookup"><span data-stu-id="93e4e-118">Ruby</span></span>
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a><span data-ttu-id="93e4e-119">Hämta information om anslutningssträngen från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="93e4e-119">Get the connection string details from the Azure portal</span></span>
<span data-ttu-id="93e4e-120">I den [Azure-portalen](https://portal.azure.com), gå till din Azure-databas för MySQL-server och klicka sedan på **anslutningssträngar** att hämta en lista med strängar för din instans: ![Connection strings rutan i Azure Portal](./media/howto-connection-strings/connection-strings-on-portal.png)</span><span class="sxs-lookup"><span data-stu-id="93e4e-120">In the [Azure portal](https://portal.azure.com), go to your Azure Database for MySQL server, and then click **Connection strings** to get your string list for your instance: ![The Connection strings pane in the Azure portal](./media/howto-connection-strings/connection-strings-on-portal.png)</span></span>

<span data-ttu-id="93e4e-121">Strängen innehåller information som drivrutinen, server och andra databasen anslutningsparametrar.</span><span class="sxs-lookup"><span data-stu-id="93e4e-121">The string provides details such as the driver, server, and other database connection parameters.</span></span> <span data-ttu-id="93e4e-122">Ändra de här exemplen med dina egna parametrar, till exempel databasens namn, lösenord och så vidare.</span><span class="sxs-lookup"><span data-stu-id="93e4e-122">Modify these examples by using your own parameters, such as database name, password, and so on.</span></span> <span data-ttu-id="93e4e-123">Du kan sedan använda den här strängen för att ansluta till servern från din kod och program.</span><span class="sxs-lookup"><span data-stu-id="93e4e-123">You can then use this string to connect to the server from your code and applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93e4e-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93e4e-124">Next steps</span></span>
- <span data-ttu-id="93e4e-125">Läs mer om anslutningsbibliotek [begrepp - anslutningsbibliotek](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="93e4e-125">For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).</span></span>
