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
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a><span data-ttu-id="6a2e1-103">Konfigurera SSL-anslutning i ditt program för att ansluta säkert till Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="6a2e1-103">Configure SSL connectivity in your application to securely connect to Azure Database for MySQL</span></span>
<span data-ttu-id="6a2e1-104">Azure-databas för MySQL stöder anslutning av din Azure-databas för MySQL-servern till klientprogram som använder Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="6a2e1-104">Azure Database for MySQL supports connecting your Azure Database for MySQL server to client applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="6a2e1-105">Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man i mitten” genom att kryptera dataströmmen mellan servern och ditt program.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

## <a name="step-1-obtain-ssl-certificate"></a><span data-ttu-id="6a2e1-106">Steg 1: Få SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="6a2e1-106">Step 1: Obtain SSL Certificate</span></span>
<span data-ttu-id="6a2e1-107">Hämta certifikat som behövs för att kommunicera via SSL med din Azure-databas för MySQL-servern från [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) och spara filen med certifikat till din lokala enhet (med den här självstudiekursen kommer vi använde c:\ssl).</span><span class="sxs-lookup"><span data-stu-id="6a2e1-107">Download the certificate needed to communicate over SSL with your Azure Database for MySQL server from [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) and save the certificate file to your local drive (with this tutorial, we used c:\ssl).</span></span>
<span data-ttu-id="6a2e1-108">**För Microsoft Internet Explorer och Microsoft Edge:** när hämtningen är slutförd, ändra certifikatet till BaltimoreCyberTrustRoot.crt.pem.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-108">**For Microsoft Internet Explorer and Microsoft Edge:** After the download has completed, rename the certificate to BaltimoreCyberTrustRoot.crt.pem.</span></span>

## <a name="step-2-bind-ssl"></a><span data-ttu-id="6a2e1-109">Steg 2: Binda SSL</span><span class="sxs-lookup"><span data-stu-id="6a2e1-109">Step 2: Bind SSL</span></span>
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a><span data-ttu-id="6a2e1-110">Ansluta till servern med hjälp av MySQL-arbetsstationen via SSL</span><span class="sxs-lookup"><span data-stu-id="6a2e1-110">Connecting to server using the MySQL Workbench over SSL</span></span>
<span data-ttu-id="6a2e1-111">Konfigurera MySQL-arbetsstationen för att ansluta på ett säkert sätt via SSL.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-111">Configure MySQL Workbench to connect securely over SSL.</span></span> <span data-ttu-id="6a2e1-112">Navigera till den **SSL** fliken i MySQL-arbetsstationen dialogen installera ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-112">Navigate to the **SSL** tab in the MySQL Workbench on the Setup New Connection dialogue.</span></span> <span data-ttu-id="6a2e1-113">Ange sökvägen till den **BaltimoreCyberTrustRoot.crt.pem** i den **SSL CA-fil:** fält.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-113">Enter the file location of the **BaltimoreCyberTrustRoot.crt.pem** in the **SSL CA File:** field.</span></span>
<span data-ttu-id="6a2e1-114">![Spara anpassade sida vid sida](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="6a2e1-114">![save customized tile](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span></span>

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a><span data-ttu-id="6a2e1-115">Ansluter till servern med hjälp av MySQL-CLI via SSL</span><span class="sxs-lookup"><span data-stu-id="6a2e1-115">Connecting to server using the MySQL CLI over SSL</span></span>
<span data-ttu-id="6a2e1-116">Genom att använda MySQL-kommandoradsgränssnitt, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6a2e1-116">Using the MySQL command-line interface, execute the following command:</span></span>
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a><span data-ttu-id="6a2e1-117">Steg 3: Framtvinga SSL-anslutningar i Azure</span><span class="sxs-lookup"><span data-stu-id="6a2e1-117">Step 3:  Enforcing SSL connections in Azure</span></span> 
### <a name="using-azure-portal"></a><span data-ttu-id="6a2e1-118">Använda Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6a2e1-118">Using Azure portal</span></span>
<span data-ttu-id="6a2e1-119">Med hjälp av Azure portal finns din Azure-databas för MySQL-servern och klickar på **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-119">Using the Azure portal, visit your Azure Database for MySQL server and click **Connection security**.</span></span> <span data-ttu-id="6a2e1-120">Använd växlingsknappen för att aktivera eller inaktivera den **framtvinga SSL-anslutning** inställningen.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-120">Use the toggle button to enable or disable the **Enforce SSL connection** setting.</span></span> <span data-ttu-id="6a2e1-121">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-121">Then click **Save**.</span></span> <span data-ttu-id="6a2e1-122">Microsoft rekommenderar att du alltid aktivera **framtvinga SSL-anslutning** för ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-122">Microsoft recommends to always enable **Enforce SSL connection** setting for enhanced security.</span></span>
<span data-ttu-id="6a2e1-123">![Aktivera ssl](./media/howto-configure-ssl/enable-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="6a2e1-123">![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="6a2e1-124">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6a2e1-124">Using Azure CLI</span></span>
<span data-ttu-id="6a2e1-125">Du kan aktivera eller inaktivera den **ssl tvingande** parametern med aktiverad eller inaktiverad värden respektive i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-125">You can enable or disable the **ssl-enforcement** parameter using Enabled or Disabled values respectively in Azure CLI.</span></span>
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a><span data-ttu-id="6a2e1-126">Steg 4: Verifiera SSL-anslutning</span><span class="sxs-lookup"><span data-stu-id="6a2e1-126">Step 4: Verify SSL Connection</span></span>
<span data-ttu-id="6a2e1-127">Köra mysql **status** kommando för att kontrollera att du har anslutit till MySQL-servern med hjälp av SSL:</span><span class="sxs-lookup"><span data-stu-id="6a2e1-127">Execute the mysql **status** command to verify that you have connected to your MySQL server using SSL:</span></span>
```dos
mysql> status
```
<span data-ttu-id="6a2e1-128">Bekräfta anslutningen är krypterad genom att granska utdata.</span><span class="sxs-lookup"><span data-stu-id="6a2e1-128">Confirm the connection is encrypted by reviewing the output.</span></span> <span data-ttu-id="6a2e1-129">Du bör se: **SSL: Cipher används är AES256 SHA**</span><span class="sxs-lookup"><span data-stu-id="6a2e1-129">It should show:  **SSL: Cipher in use is AES256-SHA**</span></span> 

## <a name="sample-code"></a><span data-ttu-id="6a2e1-130">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="6a2e1-130">Sample code</span></span>
### <a name="php"></a><span data-ttu-id="6a2e1-131">PHP</span><span class="sxs-lookup"><span data-stu-id="6a2e1-131">PHP</span></span>
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a><span data-ttu-id="6a2e1-132">Python</span><span class="sxs-lookup"><span data-stu-id="6a2e1-132">Python</span></span>
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a><span data-ttu-id="6a2e1-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a2e1-133">Next steps</span></span>
<span data-ttu-id="6a2e1-134">Granska olika anslutningsalternativ för programmet efter [anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="6a2e1-134">Review various application connectivity options following [Connection libraries for Azure Database for MySQL](concepts-connection-libraries.md)</span></span>
