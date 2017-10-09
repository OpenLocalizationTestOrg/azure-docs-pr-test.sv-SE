---
title: "utvecklingsöversikt för aaaDatabase program för Azure-databas för MySQL | Microsoft Docs"
description: "Introducerar designöverväganden som utvecklare bör följa när du skriver programmet kod tooconnect tooAzure databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: f08df605eba21b4ba4b43565c0a7ded95779a171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a><span data-ttu-id="eebda-103">Översikt över utveckling program för Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="eebda-103">Application development overview for Azure Database for MySQL</span></span> 
<span data-ttu-id="eebda-104">Den här artikeln beskrivs överväganden vid utformning av som utvecklare bör följa när du skriver programmet kod tooconnect tooAzure databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="eebda-104">This article discusses design considerations that a developer should follow when writing application code tooconnect tooAzure Database for MySQL</span></span> 

> [!TIP]
> <span data-ttu-id="eebda-105">En självstudiekurs visar du hur toocreate en server, skapa en serverbaserad brandvägg, Visa serveregenskaper, skapa databas, Anslut och fråga med arbetsstationen och mysql.exe finns [utforma din första Azure MySQL-databas](tutorial-design-database-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="eebda-105">For a tutorial showing you how toocreate a server, create a server-based firewall, view server properties, create database, connect and query using workbench and mysql.exe, see [Design your first Azure MySQL database](tutorial-design-database-using-portal.md)</span></span>

## <a name="language-and-platform"></a><span data-ttu-id="eebda-106">Språk och plattform</span><span class="sxs-lookup"><span data-stu-id="eebda-106">Language and platform</span></span>
<span data-ttu-id="eebda-107">Det finns kodexempel för olika programmeringsspråk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="eebda-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="eebda-108">Du hittar länkar toohello kodexempel på: [anslutning bibliotek används tooconnect tooAzure databas för MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="eebda-108">You can find links toohello code samples at: [Connectivity libraries used tooconnect tooAzure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="eebda-109">Verktyg</span><span class="sxs-lookup"><span data-stu-id="eebda-109">Tools</span></span>
<span data-ttu-id="eebda-110">Azure-databas för MySQL använder hello MySQL community-versionen, kompatibel med MySQL vanliga hanteringsverktyg, till exempel arbetsstationen eller MySQL verktyg, till exempel mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), med mera.</span><span class="sxs-lookup"><span data-stu-id="eebda-110">Azure Database for MySQL uses hello MySQL community version, compatible with MySQL common management tools such as Workbench or MySQL utilities such as mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), and others.</span></span> <span data-ttu-id="eebda-111">Du kan också använda hello Azure-portalen, Azure CLI och REST API: er toointeract med hello database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eebda-111">You can also use hello Azure portal, Azure CLI, and REST APIs toointeract with hello database service.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="eebda-112">Resursbegränsningar</span><span class="sxs-lookup"><span data-stu-id="eebda-112">Resource limitations</span></span>
<span data-ttu-id="eebda-113">Azure MySQL-databas hanterar hello resurser tillgängliga tooa server på två olika sätt:</span><span class="sxs-lookup"><span data-stu-id="eebda-113">Azure MySQL Database manages hello resources available tooa server using two different mechanisms:</span></span> 
- <span data-ttu-id="eebda-114">Resurser-styrning</span><span class="sxs-lookup"><span data-stu-id="eebda-114">Resources Governance</span></span> 
- <span data-ttu-id="eebda-115">Tillämpning av gränser.</span><span class="sxs-lookup"><span data-stu-id="eebda-115">Enforcement of Limits.</span></span>

## <a name="security"></a><span data-ttu-id="eebda-116">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="eebda-116">Security</span></span>
<span data-ttu-id="eebda-117">Azure MySQL-databas innehåller resurser för att begränsa åtkomst, skyddar data, konfigurera användare och roll och övervakning av aktiviteter i en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="eebda-117">Azure MySQL Database provides resources for limiting access, protecting data, configuring users and role, and monitoring activities on a MySQL Database.</span></span>

## <a name="authentication"></a><span data-ttu-id="eebda-118">Autentisering</span><span class="sxs-lookup"><span data-stu-id="eebda-118">Authentication</span></span>
<span data-ttu-id="eebda-119">Azure MySQL-databas har stöd för server-autentisering av användare och inloggningar.</span><span class="sxs-lookup"><span data-stu-id="eebda-119">Azure MySQL Database supports server authentication of users and logins.</span></span>

## <a name="resiliency"></a><span data-ttu-id="eebda-120">Återhämtning</span><span class="sxs-lookup"><span data-stu-id="eebda-120">Resiliency</span></span>
<span data-ttu-id="eebda-121">När ett tillfälligt fel uppstår vid anslutning tooMySQL databasen bör koden försöka hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="eebda-121">When a transient error occurs while connecting tooMySQL Database, your code should retry hello call.</span></span> <span data-ttu-id="eebda-122">Vi rekommenderar hello försök logik användning av en logik, så att den inte överväldigande hello SQL-databas med flera klienter som du försöker samtidigt.</span><span class="sxs-lookup"><span data-stu-id="eebda-122">We recommend hello retry logic use back off logic, so that it does not overwhelm hello SQL Database with multiple clients retrying simultaneously.</span></span>

- <span data-ttu-id="eebda-123">Kodexempel: kodexempel som visar försök logik, se exempel för hello språk på: [anslutning bibliotek används tooconnect tooAzure databas för MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="eebda-123">Code samples: For code samples that illustrate retry logic, see samples for hello language of your choice at: [Connectivity libraries used tooconnect tooAzure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="eebda-124">Hantera anslutningar</span><span class="sxs-lookup"><span data-stu-id="eebda-124">Managing Connections</span></span>
<span data-ttu-id="eebda-125">Databasanslutningar är en begränsad resurs, så vi rekommenderar sensible användning av anslutningar vid åtkomst till din MySQL-databas tooachieve bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="eebda-125">Database connections are a limited resource, so we recommend sensible use of connections when accessing your MySQL Database tooachieve better performance.</span></span>
- <span data-ttu-id="eebda-126">Access hello-databas med hjälp av anslutningspooler eller beständiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="eebda-126">Access hello database by using connection pooling or persistent connections.</span></span>
- <span data-ttu-id="eebda-127">Access hello-databas med hjälp av anslutning kort livslängd.</span><span class="sxs-lookup"><span data-stu-id="eebda-127">Access hello database by using short connection life span.</span></span> 
- <span data-ttu-id="eebda-128">Använd logik i programmet hello gång hello anslutningsförsök, toocatch fel på grund av tooconcurrent anslutningar har uppnåtts hello tillåtna.</span><span class="sxs-lookup"><span data-stu-id="eebda-128">Use retry logic in your application at hello point of hello connection attempt, toocatch failures due tooconcurrent connections have reached hello maximum allowed.</span></span> <span data-ttu-id="eebda-129">Försök logik i hello, ange en kort fördröjning och sedan vänta tills en slumpmässig tid innan hello ytterligare anslutningsförsök.</span><span class="sxs-lookup"><span data-stu-id="eebda-129">In hello retry logic, set a short delay, and then wait for a random time before hello additional connection attempts.</span></span>
