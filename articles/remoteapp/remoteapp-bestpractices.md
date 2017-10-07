---
title: "Metodtips för aaaAzure RemoteApp | Microsoft Docs"
description: "Metodtips för att konfigurera och använda Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b851865b-bec4-4f29-82c0-7b9770c1a520
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f4d09ef30816eaebb74b69f26f3242c69ea27591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a><span data-ttu-id="823b5-103">Metodtips för att konfigurera och använda Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="823b5-103">Best practices for configuring and using Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="823b5-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="823b5-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="823b5-105">Läs hello [meddelande](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) mer information.</span><span class="sxs-lookup"><span data-stu-id="823b5-105">Read hello [announcement](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) for details.</span></span>
> 
> 

<span data-ttu-id="823b5-106">hello följande information kan hjälpa dig att konfigurera och använda Azure RemoteApp effektivt.</span><span class="sxs-lookup"><span data-stu-id="823b5-106">hello following information can help you configure and use Azure RemoteApp productively.</span></span>

## <a name="connectivity"></a><span data-ttu-id="823b5-107">Anslutning</span><span class="sxs-lookup"><span data-stu-id="823b5-107">Connectivity</span></span>
* <span data-ttu-id="823b5-108">Använd alltid hello senaste klientversion.</span><span class="sxs-lookup"><span data-stu-id="823b5-108">Always use hello latest client version.</span></span> <span data-ttu-id="823b5-109">Med hjälp av äldre klienter leda till problem med nätverksanslutningen och andra försämrad upplevelser.</span><span class="sxs-lookup"><span data-stu-id="823b5-109">Using older clients might result in connectivity issues and other degraded experiences.</span></span> <span data-ttu-id="823b5-110">Aktivera automatisk tillämpning uppdateringar för din enhet säkerställer installeras alltid den senaste hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="823b5-110">Enabling automatic application updates for your device will ensure that hello latest client is always installed.</span></span>
* <span data-ttu-id="823b5-111">Använd alltid hello stabila och mest tillförlitliga internet-anslutning tillgängliga tooyou.</span><span class="sxs-lookup"><span data-stu-id="823b5-111">Always use hello most stable and reliable internet connection available tooyou.</span></span>  
* <span data-ttu-id="823b5-112">Endast stöd för att använda proxy-anslutningar för optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="823b5-112">Use only supported proxy connections for optimal connectivity performance.</span></span>  <span data-ttu-id="823b5-113">hello SOCKS-proxy stöds inte.</span><span class="sxs-lookup"><span data-stu-id="823b5-113">hello SOCKS proxy is not supported.</span></span>

## <a name="applications"></a><span data-ttu-id="823b5-114">Program</span><span class="sxs-lookup"><span data-stu-id="823b5-114">Applications</span></span>
* <span data-ttu-id="823b5-115">Spara och stänga RemoteApp-program när du är klar med hello program.</span><span class="sxs-lookup"><span data-stu-id="823b5-115">Save and close RemoteApp applications when you are done with hello application.</span></span> <span data-ttu-id="823b5-116">Inte stänga programmet hello kan resultera i förlust av data.</span><span class="sxs-lookup"><span data-stu-id="823b5-116">Not closing hello application might result in data loss.</span></span>
* <span data-ttu-id="823b5-117">Verifiera anpassade program innan du använder dem i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="823b5-117">Validate custom applications before using them in Azure RemoteApp.</span></span> <span data-ttu-id="823b5-118">Detta inkluderar att säkerställa de fungerar på en plattform för flera sessioner och inte använda onödiga resurser, till exempel minne och processor som kan strypa en annan användare i hello samma samling.</span><span class="sxs-lookup"><span data-stu-id="823b5-118">This includes ensuring they work on a multi-session platform and don’t consume unnecessary resources such as memory and CPU that might starve another user in hello same collection.</span></span> <span data-ttu-id="823b5-119">Hämta information och granska hello [Application Compatibility bästa praxis för Fjärrskrivbordstjänster](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span><span class="sxs-lookup"><span data-stu-id="823b5-119">For information, download and review hello [Application Compatibility Best Practices for Remote Desktop Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span></span>

## <a name="configuration-and-management"></a><span data-ttu-id="823b5-120">Konfiguration och hantering</span><span class="sxs-lookup"><span data-stu-id="823b5-120">Configuration and management</span></span>
* <span data-ttu-id="823b5-121">Behåll din mallavbildningarna in toodate, programuppdateringar och andra kritiska korrigeringar installeras vid behov.</span><span class="sxs-lookup"><span data-stu-id="823b5-121">Keep your template images up toodate, installing software updates and other critical fixes as needed.</span></span> <span data-ttu-id="823b5-122">Detta säkerställer att som Azure RemoteApp auto-skalor toomeet korrigeras kapacitet varje instans.</span><span class="sxs-lookup"><span data-stu-id="823b5-122">This ensures that as Azure RemoteApp auto-scales toomeet your capacity, each instance is patched.</span></span>  
* <span data-ttu-id="823b5-123">Kontrollera distributionen av Active Directory Federation Services (AD FS) är säkert och tillförlitligt.</span><span class="sxs-lookup"><span data-stu-id="823b5-123">Make sure your Active Directory Federation Services (AD FS) deployment is secure and reliable.</span></span> <span data-ttu-id="823b5-124">Annars misslyckas klienten autentiseringar, hindrar användare från att komma åt Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="823b5-124">Otherwise client authentications might fail, preventing users from accessing Azure RemoteApp.</span></span>
* <span data-ttu-id="823b5-125">Konfigurera mallavbildningarna med installerade program, roller eller funktioner som finns tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="823b5-125">Configure template images with installed applications, roles, or features such that they are stateless.</span></span> <span data-ttu-id="823b5-126">De bör inte lita på alla instanser av hello virtuella datorer i en RemoteApp-tjänsten är i ett beständigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="823b5-126">They should not rely on any instances of hello virtual machines in a RemoteApp service being in a persistent state.</span></span>
  * <span data-ttu-id="823b5-127">Lagra alla användardata i användarprofiler eller andra platser externa toohello lagringstjänsten, t.ex. lokala filen resurser eller OneDrive.</span><span class="sxs-lookup"><span data-stu-id="823b5-127">Store all user data in user profiles or other storage locations external toohello service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="823b5-128">Lagra delade data i platser externa toohello lagringstjänsten, t.ex. lokala filen resurser eller OneDrive.</span><span class="sxs-lookup"><span data-stu-id="823b5-128">Store shared data in storage locations external toohello service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="823b5-129">Konfigurera inställningar för hela systemet i hello mallavbildningen i stället för på enskilda virtuella datorer i en tjänst.</span><span class="sxs-lookup"><span data-stu-id="823b5-129">Configure any system-wide settings in hello template image rather than on individual virtual machines in a service.</span></span>
  * <span data-ttu-id="823b5-130">Inaktivera automatiska uppdateringar av publicerade program – i stället använda dem manuellt toohello mallavbildningen och testa dem innan du distribuerar från hello mall.</span><span class="sxs-lookup"><span data-stu-id="823b5-130">Disable automatic software updates for published applications - instead apply them manually toohello template image and test them before you deploy  from hello template.</span></span>

