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
# <a name="configure-ssl-connectivity-in-your-application-toosecurely-connect-tooazure-database-for-mysql"></a><span data-ttu-id="56f74-103">Konfigurera SSL anslutningen i ditt program toosecurely connect tooAzure databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="56f74-103">Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL</span></span>
<span data-ttu-id="56f74-104">Azure-databas för MySQL stöder anslutning av din Azure-databas för MySQL tooclient serverprogram med hjälp av Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="56f74-104">Azure Database for MySQL supports connecting your Azure Database for MySQL server tooclient applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="56f74-105">Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.</span><span class="sxs-lookup"><span data-stu-id="56f74-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

## <a name="step-1-obtain-ssl-certificate"></a><span data-ttu-id="56f74-106">Steg 1: Få SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="56f74-106">Step 1: Obtain SSL Certificate</span></span>
<span data-ttu-id="56f74-107">Hämta hello certifikat som behövs toocommunicate via SSL med din Azure-databas för MySQL-servern från [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) och spara hello certifikat filen tooyour lokala enhet (med den här kursen används vi c:\ssl).</span><span class="sxs-lookup"><span data-stu-id="56f74-107">Download hello certificate needed toocommunicate over SSL with your Azure Database for MySQL server from [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) and save hello certificate file tooyour local drive (with this tutorial, we used c:\ssl).</span></span>
<span data-ttu-id="56f74-108">**För Microsoft Internet Explorer och Microsoft Edge:** när hello har hämtats, byta namn på hello certifikat tooBaltimoreCyberTrustRoot.crt.pem.</span><span class="sxs-lookup"><span data-stu-id="56f74-108">**For Microsoft Internet Explorer and Microsoft Edge:** After hello download has completed, rename hello certificate tooBaltimoreCyberTrustRoot.crt.pem.</span></span>

## <a name="step-2-bind-ssl"></a><span data-ttu-id="56f74-109">Steg 2: Binda SSL</span><span class="sxs-lookup"><span data-stu-id="56f74-109">Step 2: Bind SSL</span></span>
### <a name="connecting-tooserver-using-hello-mysql-workbench-over-ssl"></a><span data-ttu-id="56f74-110">Ansluta tooserver med hello MySQL arbetsstationen via SSL</span><span class="sxs-lookup"><span data-stu-id="56f74-110">Connecting tooserver using hello MySQL Workbench over SSL</span></span>
<span data-ttu-id="56f74-111">Konfigurera MySQL-arbetsstationen tooconnect på ett säkert sätt via SSL.</span><span class="sxs-lookup"><span data-stu-id="56f74-111">Configure MySQL Workbench tooconnect securely over SSL.</span></span> <span data-ttu-id="56f74-112">Navigera toohello **SSL** fliken i hello MySQL-arbetsstationen på hello konfigurera nya Connection dialog.</span><span class="sxs-lookup"><span data-stu-id="56f74-112">Navigate toohello **SSL** tab in hello MySQL Workbench on hello Setup New Connection dialogue.</span></span> <span data-ttu-id="56f74-113">Ange hello filplatsen för hello **BaltimoreCyberTrustRoot.crt.pem** i hello **SSL CA-fil:** fält.</span><span class="sxs-lookup"><span data-stu-id="56f74-113">Enter hello file location of hello **BaltimoreCyberTrustRoot.crt.pem** in hello **SSL CA File:** field.</span></span>
<span data-ttu-id="56f74-114">![Spara anpassade sida vid sida](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="56f74-114">![save customized tile](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span></span>

### <a name="connecting-tooserver-using-hello-mysql-cli-over-ssl"></a><span data-ttu-id="56f74-115">Ansluta tooserver med hello MySQL CLI via SSL</span><span class="sxs-lookup"><span data-stu-id="56f74-115">Connecting tooserver using hello MySQL CLI over SSL</span></span>
<span data-ttu-id="56f74-116">Med hello MySQL kommandoradsgränssnitt, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="56f74-116">Using hello MySQL command-line interface, execute hello following command:</span></span>
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a><span data-ttu-id="56f74-117">Steg 3: Framtvinga SSL-anslutningar i Azure</span><span class="sxs-lookup"><span data-stu-id="56f74-117">Step 3:  Enforcing SSL connections in Azure</span></span> 
### <a name="using-azure-portal"></a><span data-ttu-id="56f74-118">Använda Azure Portal</span><span class="sxs-lookup"><span data-stu-id="56f74-118">Using Azure portal</span></span>
<span data-ttu-id="56f74-119">Med hjälp av hello Azure-portalen finns din Azure-databas för MySQL-servern och klickar på **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="56f74-119">Using hello Azure portal, visit your Azure Database for MySQL server and click **Connection security**.</span></span> <span data-ttu-id="56f74-120">Använd hello växla knappen tooenable eller inaktivera hello **framtvinga SSL-anslutning** inställningen.</span><span class="sxs-lookup"><span data-stu-id="56f74-120">Use hello toggle button tooenable or disable hello **Enforce SSL connection** setting.</span></span> <span data-ttu-id="56f74-121">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="56f74-121">Then click **Save**.</span></span> <span data-ttu-id="56f74-122">Microsoft rekommenderar tooalways aktiverar **framtvinga SSL-anslutning** för ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="56f74-122">Microsoft recommends tooalways enable **Enforce SSL connection** setting for enhanced security.</span></span>
<span data-ttu-id="56f74-123">![Aktivera ssl](./media/howto-configure-ssl/enable-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="56f74-123">![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="56f74-124">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="56f74-124">Using Azure CLI</span></span>
<span data-ttu-id="56f74-125">Du kan aktivera eller inaktivera hello **ssl tvingande** parametern med aktiverad eller inaktiverad värden respektive i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="56f74-125">You can enable or disable hello **ssl-enforcement** parameter using Enabled or Disabled values respectively in Azure CLI.</span></span>
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a><span data-ttu-id="56f74-126">Steg 4: Verifiera SSL-anslutning</span><span class="sxs-lookup"><span data-stu-id="56f74-126">Step 4: Verify SSL Connection</span></span>
<span data-ttu-id="56f74-127">Köra hello mysql **status** kommandot tooverify att du har anslutit tooyour MySQL-servern med SSL:</span><span class="sxs-lookup"><span data-stu-id="56f74-127">Execute hello mysql **status** command tooverify that you have connected tooyour MySQL server using SSL:</span></span>
```dos
mysql> status
```
<span data-ttu-id="56f74-128">Bekräfta hello anslutningen är krypterad genom att granska hello utdata.</span><span class="sxs-lookup"><span data-stu-id="56f74-128">Confirm hello connection is encrypted by reviewing hello output.</span></span> <span data-ttu-id="56f74-129">Du bör se: **SSL: Cipher används är AES256 SHA**</span><span class="sxs-lookup"><span data-stu-id="56f74-129">It should show:  **SSL: Cipher in use is AES256-SHA**</span></span> 

## <a name="sample-code"></a><span data-ttu-id="56f74-130">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="56f74-130">Sample code</span></span>
### <a name="php"></a><span data-ttu-id="56f74-131">PHP</span><span class="sxs-lookup"><span data-stu-id="56f74-131">PHP</span></span>
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a><span data-ttu-id="56f74-132">Python</span><span class="sxs-lookup"><span data-stu-id="56f74-132">Python</span></span>
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a><span data-ttu-id="56f74-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56f74-133">Next steps</span></span>
<span data-ttu-id="56f74-134">Granska olika anslutningsalternativ för programmet efter [anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="56f74-134">Review various application connectivity options following [Connection libraries for Azure Database for MySQL](concepts-connection-libraries.md)</span></span>
