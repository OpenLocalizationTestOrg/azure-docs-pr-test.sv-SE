---
title: "Metodtips för Azure RemoteApp | Microsoft Docs"
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
ms.openlocfilehash: ab9c2dafc622e9b6f3bcd2767218cdc03e844847
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a><span data-ttu-id="aad33-103">Metodtips för att konfigurera och använda Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="aad33-103">Best practices for configuring and using Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="aad33-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="aad33-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="aad33-105">Läs [meddelandet](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="aad33-105">Read the [announcement](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) for details.</span></span>
> 
> 

<span data-ttu-id="aad33-106">Följande information kan hjälpa dig att konfigurera och använda Azure RemoteApp effektivt.</span><span class="sxs-lookup"><span data-stu-id="aad33-106">The following information can help you configure and use Azure RemoteApp productively.</span></span>

## <a name="connectivity"></a><span data-ttu-id="aad33-107">Anslutning</span><span class="sxs-lookup"><span data-stu-id="aad33-107">Connectivity</span></span>
* <span data-ttu-id="aad33-108">Använd alltid den senaste klientversionen.</span><span class="sxs-lookup"><span data-stu-id="aad33-108">Always use the latest client version.</span></span> <span data-ttu-id="aad33-109">Med hjälp av äldre klienter leda till problem med nätverksanslutningen och andra försämrad upplevelser.</span><span class="sxs-lookup"><span data-stu-id="aad33-109">Using older clients might result in connectivity issues and other degraded experiences.</span></span> <span data-ttu-id="aad33-110">Aktivera automatisk tillämpning uppdateringar för din enhet säkerställer att installeras alltid den senaste klienten.</span><span class="sxs-lookup"><span data-stu-id="aad33-110">Enabling automatic application updates for your device will ensure that the latest client is always installed.</span></span>
* <span data-ttu-id="aad33-111">Använd alltid stabila och mest tillförlitliga Internetanslutningen tillgängliga för dig.</span><span class="sxs-lookup"><span data-stu-id="aad33-111">Always use the most stable and reliable internet connection available to you.</span></span>  
* <span data-ttu-id="aad33-112">Endast stöd för att använda proxy-anslutningar för optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="aad33-112">Use only supported proxy connections for optimal connectivity performance.</span></span>  <span data-ttu-id="aad33-113">SOCKS-proxy stöds inte.</span><span class="sxs-lookup"><span data-stu-id="aad33-113">The SOCKS proxy is not supported.</span></span>

## <a name="applications"></a><span data-ttu-id="aad33-114">Program</span><span class="sxs-lookup"><span data-stu-id="aad33-114">Applications</span></span>
* <span data-ttu-id="aad33-115">Spara och stänga RemoteApp-program när du är klar med programmet.</span><span class="sxs-lookup"><span data-stu-id="aad33-115">Save and close RemoteApp applications when you are done with the application.</span></span> <span data-ttu-id="aad33-116">Inte stänga programmet kan resultera i förlust av data.</span><span class="sxs-lookup"><span data-stu-id="aad33-116">Not closing the application might result in data loss.</span></span>
* <span data-ttu-id="aad33-117">Verifiera anpassade program innan du använder dem i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="aad33-117">Validate custom applications before using them in Azure RemoteApp.</span></span> <span data-ttu-id="aad33-118">Detta inkluderar att säkerställa de fungerar på en plattform för flera sessioner och inte använda onödiga resurser, till exempel minne och processor som kan strypa en annan användare i samma samling.</span><span class="sxs-lookup"><span data-stu-id="aad33-118">This includes ensuring they work on a multi-session platform and don’t consume unnecessary resources such as memory and CPU that might starve another user in the same collection.</span></span> <span data-ttu-id="aad33-119">Information, hämta och granska de [Application Compatibility bästa praxis för Fjärrskrivbordstjänster](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span><span class="sxs-lookup"><span data-stu-id="aad33-119">For information, download and review the [Application Compatibility Best Practices for Remote Desktop Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span></span>

## <a name="configuration-and-management"></a><span data-ttu-id="aad33-120">Konfiguration och hantering</span><span class="sxs-lookup"><span data-stu-id="aad33-120">Configuration and management</span></span>
* <span data-ttu-id="aad33-121">Kontinuerligt mallavbildningar, programuppdateringar och andra kritiska korrigeringar installeras vid behov.</span><span class="sxs-lookup"><span data-stu-id="aad33-121">Keep your template images up to date, installing software updates and other critical fixes as needed.</span></span> <span data-ttu-id="aad33-122">Detta säkerställer att som automatiskt Azure RemoteApp-skalor för att uppfylla dina kapacitet, korrigeras varje instans.</span><span class="sxs-lookup"><span data-stu-id="aad33-122">This ensures that as Azure RemoteApp auto-scales to meet your capacity, each instance is patched.</span></span>  
* <span data-ttu-id="aad33-123">Kontrollera distributionen av Active Directory Federation Services (AD FS) är säkert och tillförlitligt.</span><span class="sxs-lookup"><span data-stu-id="aad33-123">Make sure your Active Directory Federation Services (AD FS) deployment is secure and reliable.</span></span> <span data-ttu-id="aad33-124">Annars misslyckas klienten autentiseringar, hindrar användare från att komma åt Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="aad33-124">Otherwise client authentications might fail, preventing users from accessing Azure RemoteApp.</span></span>
* <span data-ttu-id="aad33-125">Konfigurera mallavbildningarna med installerade program, roller eller funktioner som finns tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="aad33-125">Configure template images with installed applications, roles, or features such that they are stateless.</span></span> <span data-ttu-id="aad33-126">De bör inte lita på alla instanser av de virtuella datorerna i en RemoteApp-tjänsten är i ett beständigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="aad33-126">They should not rely on any instances of the virtual machines in a RemoteApp service being in a persistent state.</span></span>
  * <span data-ttu-id="aad33-127">Lagra alla användardata i användarprofiler eller andra lagringsplatser som är externa för tjänsten, t.ex. lokala filen resurser eller OneDrive.</span><span class="sxs-lookup"><span data-stu-id="aad33-127">Store all user data in user profiles or other storage locations external to the service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="aad33-128">Lagra delade data i lagringsplatser som är externa för tjänsten, t.ex. lokala filen resurser eller OneDrive.</span><span class="sxs-lookup"><span data-stu-id="aad33-128">Store shared data in storage locations external to the service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="aad33-129">Konfigurera inställningar för hela systemet i mallavbildningen i stället för på enskilda virtuella datorer i en tjänst.</span><span class="sxs-lookup"><span data-stu-id="aad33-129">Configure any system-wide settings in the template image rather than on individual virtual machines in a service.</span></span>
  * <span data-ttu-id="aad33-130">Inaktivera automatiska uppdateringar av publicerade program – i stället använda dem manuellt på mallavbildningen och testa dem innan du distribuerar från mallen.</span><span class="sxs-lookup"><span data-stu-id="aad33-130">Disable automatic software updates for published applications - instead apply them manually to the template image and test them before you deploy  from the template.</span></span>

