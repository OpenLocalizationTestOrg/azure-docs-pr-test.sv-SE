---
title: "aaaNever lagra känsliga data på anpassade avbildningar för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om hello riktlinjer för att lagra data i anpassade avbildningar i Azure RemoteApp"
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
ms.openlocfilehash: 86a0fea218f8826d6d25f50d3c4c36e368e26fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a><span data-ttu-id="c7cd4-103">Lagra inte känsliga data på anpassade avbildningar</span><span class="sxs-lookup"><span data-stu-id="c7cd4-103">Never store sensitive data on custom images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c7cd4-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="c7cd4-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="c7cd4-106">När du är värd för programmet i Azure RemoteApp, är hello första steget toocreate en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-106">When you host your own application in Azure RemoteApp, hello first step is toocreate a custom image.</span></span> <span data-ttu-id="c7cd4-107">Vi använder de anpassade bilden toocreate VM-instanser som hanterar dina appar tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-107">We use that custom image toocreate VM instances that serve your apps tooyour users.</span></span> <span data-ttu-id="c7cd4-108">hello anpassad avbildning bör innehålla endast program och aldrig känsliga data som kan gå förlorade, till exempel SQL-databaser, supportpersonal filer eller särskilda filer som QuickBooks företagets filer.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-108">hello custom image should contain ONLY applications and never sensitive data that can be lost, such as SQL databases, personnel files, or special data files like QuickBooks company files.</span></span> <span data-ttu-id="c7cd4-109">Alla känsliga data bör finnas externa tooAzure RemoteApp på en filserver och en annan Azure VM, eller i SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-109">All sensitive data should reside external tooAzure RemoteApp on a file server, another Azure VM, or in SQL Azure.</span></span> <span data-ttu-id="c7cd4-110">hello avbildningen ska bara hello värdprogrammet som ansluter toohello datakälla och visar hello data.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-110">hello image should just host hello application that connects toohello data source and presents hello data.</span></span> <span data-ttu-id="c7cd4-111">Granska [krav för Azure RemoteApp bilder](remoteapp-imagereqs.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-111">Review [Requirements for Azure RemoteApp images](remoteapp-imagereqs.md) for more information.</span></span> 

<span data-ttu-id="c7cd4-112">toounderstand varför inte bör du lagra känsliga data, behöver du toounderstand hur fungerar Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-112">toounderstand why you should not store sensitive data, you need toounderstand how Azure RemoteApp works.</span></span> <span data-ttu-id="c7cd4-113">När en samling skapas eller uppdateras, bakgrunden hello skapas flera kloner eller kopior av hello bild.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-113">When a collection is created or updated, behind hello scenes multiple clones or copies of hello image are created.</span></span> <span data-ttu-id="c7cd4-114">Alla dessa VM-instanser är exakt repliker av hello anpassade bilden. När användarna startar program är de anslutna tooone av dessa VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-114">All these VM instances are exact replicas of hello custom image; when users launch applications they are connected tooone of these VM instances.</span></span> <span data-ttu-id="c7cd4-115">Men hello samma instans är inte säkert och bör inte roll eftersom de är icke-beständig.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-115">But hello same instance is not guaranteed and should not matter because they are non-persistent.</span></span> <span data-ttu-id="c7cd4-116">Hej VM-instanser värd hello program är inte beständiga och kan vara raderats eller tagits bort baserat, till exempel under uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-116">hello VM instances hosting hello applications are non-persistent and can be destroyed or deleted based, for example, during collection update.</span></span> 

<span data-ttu-id="c7cd4-117">När hello samling etableras och startar användarna ansluter toohello VMs användardata är permanent och skyddas eftersom den har sparats i separata lagringsutrymme i en virtuell Hårddisk som vi kallar en [användarprofil-disk (UPD)](remoteapp-upd.md), vilket är hello användarprofil i c:\users\<userprofile >.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-117">Once hello collection is provisioned and users start connecting toohello VMs, user data is persistent and protected because it is saved on separate storage within a VHD that we call a [user profile disk (UPD)](remoteapp-upd.md), which is hello user profile in c:\users\<userprofile>.</span></span> <span data-ttu-id="c7cd4-118">När ett program börjar hello UPD monteras och behandlas precis som en lokal användarprofil hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-118">When an application starts, hello UPD is mounted and treated just like a local user profile by hello operating system.</span></span> <span data-ttu-id="c7cd4-119">Läs mer om hur [Azure RemoteApp sparar användardata och inställningar](remoteapp-upd.md).</span><span class="sxs-lookup"><span data-stu-id="c7cd4-119">Read more about how [Azure RemoteApp saves user data and settings](remoteapp-upd.md).</span></span>

<span data-ttu-id="c7cd4-120">Exempeldata som inte ska placeras i hello avbildningen:</span><span class="sxs-lookup"><span data-stu-id="c7cd4-120">Example data that should not reside in hello image:</span></span>

* <span data-ttu-id="c7cd4-121">Delade data för användare tooaccess</span><span class="sxs-lookup"><span data-stu-id="c7cd4-121">Shared data for users tooaccess</span></span>
* <span data-ttu-id="c7cd4-122">SQL-databas eller QuickBooks DB</span><span class="sxs-lookup"><span data-stu-id="c7cd4-122">SQL DB or QuickBooks DB</span></span>
* <span data-ttu-id="c7cd4-123">Inga data i D:\\</span><span class="sxs-lookup"><span data-stu-id="c7cd4-123">Any data in D:\\</span></span>

<span data-ttu-id="c7cd4-124">Exempeldata som kan finnas i hello standard profil toobe kopieras till varje användare UPD:</span><span class="sxs-lookup"><span data-stu-id="c7cd4-124">Example data that can reside in hello default profile toobe copied into every users’ UPD:</span></span>

* <span data-ttu-id="c7cd4-125">Konfigurationsfiler per användare</span><span class="sxs-lookup"><span data-stu-id="c7cd4-125">Configuration files per user</span></span>
* <span data-ttu-id="c7cd4-126">Skript som användarna behöver bevaras i sina UPD</span><span class="sxs-lookup"><span data-stu-id="c7cd4-126">Scripts that users would need preserved in their UPD</span></span>

<span data-ttu-id="c7cd4-127">Viktiga punkter:</span><span class="sxs-lookup"><span data-stu-id="c7cd4-127">Key points:</span></span>

* <span data-ttu-id="c7cd4-128">Lagra inte känsliga data som kan gå förlorade på hello avbildningen när du skapar en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-128">Never store sensitive data that can be lost on hello image when creating a custom image.</span></span>
* <span data-ttu-id="c7cd4-129">Känsliga data bör alltid finns på en separat filserver, avgränsa Azure VM, på hello moln och alltid externa toohello VM-instanser som värd för dina program i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="c7cd4-129">Sensitive data should always reside on a separate file server, separate Azure VM, on hello cloud, and always external toohello VM instances hosting your applications within Azure RemoteApp.</span></span> 
* <span data-ttu-id="c7cd4-130">Informationen sparas och kvarstår i hello användarprofil-disk (UPD)</span><span class="sxs-lookup"><span data-stu-id="c7cd4-130">User data is saved and persists in hello user profile disk (UPD)</span></span>

