---
title: "Databasen application utvecklingsöversikt för Azure-databas för MySQL | Microsoft Docs"
description: "Introducerar designöverväganden som utvecklare bör följa när du skriver programkod för att ansluta till Azure-databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 350dd775e172120d806d1193877a34d94f4d3f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a><span data-ttu-id="7c6af-103">Översikt över utveckling program för Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="7c6af-103">Application development overview for Azure Database for MySQL</span></span> 
<span data-ttu-id="7c6af-104">Den här artikeln beskrivs överväganden vid utformning av som utvecklare bör följa när du skriver programkod för att ansluta till Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="7c6af-104">This article discusses design considerations that a developer should follow when writing application code to connect to Azure Database for MySQL</span></span> 

> [!TIP]
> <span data-ttu-id="7c6af-105">En självstudiekurs visar hur du skapar du en server, skapa en serverbaserad brandvägg, Visa serveregenskaper, skapa databas, Anslut och fråga med arbetsstationen och mysql.exe finns [utforma din första Azure MySQL-databas](tutorial-design-database-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7c6af-105">For a tutorial showing you how to create a server, create a server-based firewall, view server properties, create database, connect and query using workbench and mysql.exe, see [Design your first Azure MySQL database](tutorial-design-database-using-portal.md)</span></span>

## <a name="language-and-platform"></a><span data-ttu-id="7c6af-106">Språk och plattform</span><span class="sxs-lookup"><span data-stu-id="7c6af-106">Language and platform</span></span>
<span data-ttu-id="7c6af-107">Det finns kodexempel för olika programmeringsspråk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="7c6af-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="7c6af-108">Du hittar länkar till kodexempel på: [anslutning bibliotek som används för att ansluta till Azure-databas för MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="7c6af-108">You can find links to the code samples at: [Connectivity libraries used to connect to Azure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="7c6af-109">Verktyg</span><span class="sxs-lookup"><span data-stu-id="7c6af-109">Tools</span></span>
<span data-ttu-id="7c6af-110">Azure MySQL-databas som använder MySQL community version, kompatibel med MySQL vanliga hanteringsverktyg, till exempel arbetsstationen eller MySQL verktyg, till exempel mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), med mera.</span><span class="sxs-lookup"><span data-stu-id="7c6af-110">Azure Database for MySQL uses the MySQL community version, compatible with MySQL common management tools such as Workbench or MySQL utilities such as mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), and others.</span></span> <span data-ttu-id="7c6af-111">Du kan också använda Azure-portalen, Azure CLI och REST API: er för att interagera med databastjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c6af-111">You can also use the Azure portal, Azure CLI, and REST APIs to interact with the database service.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="7c6af-112">Resursbegränsningar</span><span class="sxs-lookup"><span data-stu-id="7c6af-112">Resource limitations</span></span>
<span data-ttu-id="7c6af-113">Azure MySQL-databas hanterar resurser som är tillgängliga för en server med två olika metoder:</span><span class="sxs-lookup"><span data-stu-id="7c6af-113">Azure MySQL Database manages the resources available to a server using two different mechanisms:</span></span> 
- <span data-ttu-id="7c6af-114">Resurser-styrning</span><span class="sxs-lookup"><span data-stu-id="7c6af-114">Resources Governance</span></span> 
- <span data-ttu-id="7c6af-115">Tillämpning av gränser.</span><span class="sxs-lookup"><span data-stu-id="7c6af-115">Enforcement of Limits.</span></span>

## <a name="security"></a><span data-ttu-id="7c6af-116">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="7c6af-116">Security</span></span>
<span data-ttu-id="7c6af-117">Azure MySQL-databas innehåller resurser för att begränsa åtkomst, skyddar data, konfigurera användare och roll och övervakning av aktiviteter i en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7c6af-117">Azure MySQL Database provides resources for limiting access, protecting data, configuring users and role, and monitoring activities on a MySQL Database.</span></span>

## <a name="authentication"></a><span data-ttu-id="7c6af-118">Autentisering</span><span class="sxs-lookup"><span data-stu-id="7c6af-118">Authentication</span></span>
<span data-ttu-id="7c6af-119">Azure MySQL-databas har stöd för server-autentisering av användare och inloggningar.</span><span class="sxs-lookup"><span data-stu-id="7c6af-119">Azure MySQL Database supports server authentication of users and logins.</span></span>

## <a name="resiliency"></a><span data-ttu-id="7c6af-120">Återhämtning</span><span class="sxs-lookup"><span data-stu-id="7c6af-120">Resiliency</span></span>
<span data-ttu-id="7c6af-121">När ett tillfälligt fel uppstår vid anslutning till MySQL-databas ska koden gör anropet.</span><span class="sxs-lookup"><span data-stu-id="7c6af-121">When a transient error occurs while connecting to MySQL Database, your code should retry the call.</span></span> <span data-ttu-id="7c6af-122">Vi rekommenderar försök logik Använd tillbaka av logik, så att den inte överväldigande SQL-databas med flera klienter som du försöker samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7c6af-122">We recommend the retry logic use back off logic, so that it does not overwhelm the SQL Database with multiple clients retrying simultaneously.</span></span>

- <span data-ttu-id="7c6af-123">Kodexempel: kodexempel som visar försök logik, se exempel för språket i ditt val på: [anslutning bibliotek som används för att ansluta till Azure-databas för MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="7c6af-123">Code samples: For code samples that illustrate retry logic, see samples for the language of your choice at: [Connectivity libraries used to connect to Azure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="7c6af-124">Hantera anslutningar</span><span class="sxs-lookup"><span data-stu-id="7c6af-124">Managing Connections</span></span>
<span data-ttu-id="7c6af-125">Databasanslutningar är en begränsad resurs, så vi rekommenderar sensible användning av anslutningar vid åtkomst till din MySQL-databas för att få bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="7c6af-125">Database connections are a limited resource, so we recommend sensible use of connections when accessing your MySQL Database to achieve better performance.</span></span>
- <span data-ttu-id="7c6af-126">Åtkomst till databasen med hjälp av anslutningspooler eller beständiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7c6af-126">Access the database by using connection pooling or persistent connections.</span></span>
- <span data-ttu-id="7c6af-127">Åtkomst till databasen med hjälp av anslutning kort livslängd.</span><span class="sxs-lookup"><span data-stu-id="7c6af-127">Access the database by using short connection life span.</span></span> 
- <span data-ttu-id="7c6af-128">Använda logik i ditt program vid anslutningen, för att fånga fel på grund av samtidiga anslutningar har nått det högsta tillåtna.</span><span class="sxs-lookup"><span data-stu-id="7c6af-128">Use retry logic in your application at the point of the connection attempt, to catch failures due to concurrent connections have reached the maximum allowed.</span></span> <span data-ttu-id="7c6af-129">Ange en kort fördröjning i logik för omprövning och sedan vänta tills en slumpmässig tid innan ytterligare anslutningsförsök.</span><span class="sxs-lookup"><span data-stu-id="7c6af-129">In the retry logic, set a short delay, and then wait for a random time before the additional connection attempts.</span></span>