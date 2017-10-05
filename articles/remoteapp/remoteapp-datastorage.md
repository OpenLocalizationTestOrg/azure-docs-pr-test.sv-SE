---
title: "Lagra inte känsliga data på anpassade avbildningar för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om riktlinjerna för att lagra data i anpassade avbildningar i Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 75d5415d33324d957617426e75909a6c6c58b1f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a><span data-ttu-id="8a235-103">Lagra inte känsliga data på anpassade avbildningar</span><span class="sxs-lookup"><span data-stu-id="8a235-103">Never store sensitive data on custom images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8a235-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="8a235-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8a235-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8a235-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8a235-106">När du är värd för programmet i Azure RemoteApp, är det första steget att skapa en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="8a235-106">When you host your own application in Azure RemoteApp, the first step is to create a custom image.</span></span> <span data-ttu-id="8a235-107">Vi använder den anpassade bilden för att skapa VM-instanser som hanterar dina appar till användarna.</span><span class="sxs-lookup"><span data-stu-id="8a235-107">We use that custom image to create VM instances that serve your apps to your users.</span></span> <span data-ttu-id="8a235-108">Den anpassade avbildningen ska innehålla endast program och aldrig känsliga data som kan gå förlorade, till exempel SQL-databaser, supportpersonal filer eller särskilda filer som QuickBooks företagets filer.</span><span class="sxs-lookup"><span data-stu-id="8a235-108">The custom image should contain ONLY applications and never sensitive data that can be lost, such as SQL databases, personnel files, or special data files like QuickBooks company files.</span></span> <span data-ttu-id="8a235-109">Alla känsliga data bör finnas utanför Azure RemoteApp på en filserver och en annan Azure VM, eller i SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="8a235-109">All sensitive data should reside external to Azure RemoteApp on a file server, another Azure VM, or in SQL Azure.</span></span> <span data-ttu-id="8a235-110">Bilden bör bara vara värd för det program som ansluter till datakällan och visar data.</span><span class="sxs-lookup"><span data-stu-id="8a235-110">The image should just host the application that connects to the data source and presents the data.</span></span> <span data-ttu-id="8a235-111">Granska [krav för Azure RemoteApp bilder](remoteapp-imagereqs.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8a235-111">Review [Requirements for Azure RemoteApp images](remoteapp-imagereqs.md) for more information.</span></span> 

<span data-ttu-id="8a235-112">För att förstå varför bör du inte lagra känsliga data, måste du förstå hur fungerar Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8a235-112">To understand why you should not store sensitive data, you need to understand how Azure RemoteApp works.</span></span> <span data-ttu-id="8a235-113">När en samling skapas eller uppdateras flera kloner eller kopior av bilden i bakgrunden skapas.</span><span class="sxs-lookup"><span data-stu-id="8a235-113">When a collection is created or updated, behind the scenes multiple clones or copies of the image are created.</span></span> <span data-ttu-id="8a235-114">Alla dessa VM-instanser är exakt repliker av den anpassade bilden. När användarna startar program som de är anslutna till något av dessa VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="8a235-114">All these VM instances are exact replicas of the custom image; when users launch applications they are connected to one of these VM instances.</span></span> <span data-ttu-id="8a235-115">Men samma instans är inte säkert och bör inte roll eftersom de är icke-beständig.</span><span class="sxs-lookup"><span data-stu-id="8a235-115">But the same instance is not guaranteed and should not matter because they are non-persistent.</span></span> <span data-ttu-id="8a235-116">VM-instanser som hyser program är inte beständiga och kan förstörs eller tas bort baserat, till exempel under uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="8a235-116">The VM instances hosting the applications are non-persistent and can be destroyed or deleted based, for example, during collection update.</span></span> 

<span data-ttu-id="8a235-117">När samlingen är etablerad och startar användarna ansluter till de virtuella datorerna kan informationen är permanent och skyddas eftersom den har sparats i separata lagringsutrymme i en virtuell Hårddisk som vi kallar en [användarprofil-disk (UPD)](remoteapp-upd.md), vilket är användarens profil i c:\users\<userprofile >.</span><span class="sxs-lookup"><span data-stu-id="8a235-117">Once the collection is provisioned and users start connecting to the VMs, user data is persistent and protected because it is saved on separate storage within a VHD that we call a [user profile disk (UPD)](remoteapp-upd.md), which is the user profile in c:\users\<userprofile>.</span></span> <span data-ttu-id="8a235-118">När ett program börjar UPD monteras och behandlas precis som en lokal användarprofil av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="8a235-118">When an application starts, the UPD is mounted and treated just like a local user profile by the operating system.</span></span> <span data-ttu-id="8a235-119">Läs mer om hur [Azure RemoteApp sparar användardata och inställningar](remoteapp-upd.md).</span><span class="sxs-lookup"><span data-stu-id="8a235-119">Read more about how [Azure RemoteApp saves user data and settings](remoteapp-upd.md).</span></span>

<span data-ttu-id="8a235-120">Exempeldata som inte ska placeras i avbildningen:</span><span class="sxs-lookup"><span data-stu-id="8a235-120">Example data that should not reside in the image:</span></span>

* <span data-ttu-id="8a235-121">Delade data för användare att komma åt</span><span class="sxs-lookup"><span data-stu-id="8a235-121">Shared data for users to access</span></span>
* <span data-ttu-id="8a235-122">SQL-databas eller QuickBooks DB</span><span class="sxs-lookup"><span data-stu-id="8a235-122">SQL DB or QuickBooks DB</span></span>
* <span data-ttu-id="8a235-123">Inga data i D:\\</span><span class="sxs-lookup"><span data-stu-id="8a235-123">Any data in D:\\</span></span>

<span data-ttu-id="8a235-124">Exempeldata som kan finnas i standardprofilen som ska kopieras till varje användare UPD:</span><span class="sxs-lookup"><span data-stu-id="8a235-124">Example data that can reside in the default profile to be copied into every users’ UPD:</span></span>

* <span data-ttu-id="8a235-125">Konfigurationsfiler per användare</span><span class="sxs-lookup"><span data-stu-id="8a235-125">Configuration files per user</span></span>
* <span data-ttu-id="8a235-126">Skript som användarna behöver bevaras i sina UPD</span><span class="sxs-lookup"><span data-stu-id="8a235-126">Scripts that users would need preserved in their UPD</span></span>

<span data-ttu-id="8a235-127">Viktiga punkter:</span><span class="sxs-lookup"><span data-stu-id="8a235-127">Key points:</span></span>

* <span data-ttu-id="8a235-128">Aldrig lagra känsliga data som kan gå förlorade på avbildningen när du skapar en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="8a235-128">Never store sensitive data that can be lost on the image when creating a custom image.</span></span>
* <span data-ttu-id="8a235-129">Känsliga data ska alltid placeras på en separat filserver separat Azure VM, i molnet, och alltid externa för VM-instanser som är värd för dina program i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8a235-129">Sensitive data should always reside on a separate file server, separate Azure VM, on the cloud, and always external to the VM instances hosting your applications within Azure RemoteApp.</span></span> 
* <span data-ttu-id="8a235-130">Informationen sparas och kvarstår i användarprofildisk (UPD)</span><span class="sxs-lookup"><span data-stu-id="8a235-130">User data is saved and persists in the user profile disk (UPD)</span></span>

