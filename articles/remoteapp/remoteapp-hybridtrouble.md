---
title: "Felsöka skapa RemoteApp-hybridsamlingar | Microsoft Docs"
description: "Lär dig hur du felsöker fel för RemoteApp-hybrid samling skapas"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: b32033ee-8d52-4e74-bb78-86ca873c34e2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: a486dcb3f994cd78311ee86521a6792a4d57438e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a><span data-ttu-id="a57fe-103">Felsök att skapa hybridsamlingar i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="a57fe-103">Troubleshoot creating Azure RemoteApp hybrid collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a57fe-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="a57fe-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a57fe-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a57fe-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="a57fe-106">En hybridsamling är värd för och lagrar data i azuremolnet, men låter användare komma åt data och resurser i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="a57fe-106">A hybrid collection is hosted in and stores data in the Azure cloud but also lets users access data and resources stored on your local network.</span></span> <span data-ttu-id="a57fe-107">Användarna kan öppna programmen genom att logga in med sina inloggningsuppgifter till företaget , synkroniserat eller federerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a57fe-107">Users can access apps by logging in with their corporate credentials synchronized or federated with Azure Active Directory.</span></span> <span data-ttu-id="a57fe-108">Du kan distribuera en hybridsamling som använder ett befintligt virtuellt Azure-nätverk eller skapa ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a57fe-108">You can deploy a hybrid collection that uses an existing Azure Virtual Network, or you can create a new virtual network.</span></span> <span data-ttu-id="a57fe-109">Vi rekommenderar att du skapar eller använder ett undernät för virtuellt nätverk med ett CIDR-intervall som är tillräckligt stor för förväntade framtida tillväxt för Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a57fe-109">We recommend that you create or use a virtual network subnet with a CIDR range large enough for expected future growth for Azure RemoteApp.</span></span>

<span data-ttu-id="a57fe-110">Samlingen ännu inte har skapats?</span><span class="sxs-lookup"><span data-stu-id="a57fe-110">Haven't created your collection yet?</span></span> <span data-ttu-id="a57fe-111">Se [skapa en hybridsamling](remoteapp-create-hybrid-deployment.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="a57fe-111">See [Create a hybrid collection](remoteapp-create-hybrid-deployment.md) for the steps.</span></span>

<span data-ttu-id="a57fe-112">Om du har problem med att skapa din samling, eller om samlingen inte fungerar som du har tänkt, se följande information.</span><span class="sxs-lookup"><span data-stu-id="a57fe-112">If you are having trouble creating your collection, or if the collection isn't working the way you think it should, check out the following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="a57fe-113">Bilden är ogiltig</span><span class="sxs-lookup"><span data-stu-id="a57fe-113">Your image is invalid</span></span>
<span data-ttu-id="a57fe-114">Om du ser ett meddelande som ”GoldImageInvalid” när du väntar på att Azure för att etablera samlingen, innebär det att mallavbildningen inte uppfyller den [definierat avbildningen krav](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="a57fe-114">If you see a message like, "GoldImageInvalid" when you are waiting for Azure to provision your collection, it means that your template image doesn't meet the [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="a57fe-115">Därför gå läsa de [krav](remoteapp-imagereqs.md), åtgärda avbildningen och försök att skapa samlingen igen.</span><span class="sxs-lookup"><span data-stu-id="a57fe-115">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try to create your collection again.</span></span>

## <a name="does-your-vnet-have-network-security-groups-defined"></a><span data-ttu-id="a57fe-116">Har ditt VNET nätverkssäkerhetsgrupper definierats?</span><span class="sxs-lookup"><span data-stu-id="a57fe-116">Does your VNET have network security groups defined?</span></span>
<span data-ttu-id="a57fe-117">Om du har nätverkssäkerhetsgrupper som definierats i undernät som du använder för samlingen, se till att dessa [URL: er och portar](remoteapp-ports.md) är tillgänglig från undernätet.</span><span class="sxs-lookup"><span data-stu-id="a57fe-117">If you have network security groups defined on the subnet you are using for your collection, make sure these [URLs and ports](remoteapp-ports.md) are accessible from within your subnet.</span></span>

<span data-ttu-id="a57fe-118">Du kan lägga till ytterligare nätverkssäkerhetsgrupper på virtuella datorer som distribueras av du i undernät för bättre kontroll.</span><span class="sxs-lookup"><span data-stu-id="a57fe-118">You can add additional network security groups to the VMs deployed by you in the subnet for tighter control.</span></span>

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a><span data-ttu-id="a57fe-119">Använder du DNS-servrar?</span><span class="sxs-lookup"><span data-stu-id="a57fe-119">Are you using your own DNS servers?</span></span> <span data-ttu-id="a57fe-120">Och de som är tillgängliga från ditt VNET undernät?</span><span class="sxs-lookup"><span data-stu-id="a57fe-120">And are they accessible from your VNET subnet?</span></span>
> [!NOTE]
> <span data-ttu-id="a57fe-121">Du måste se till att DNS-servrar i ditt VNET alltid är igång och alltid kunna matcha de virtuella datorerna i virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="a57fe-121">You have to make sure the DNS servers in your VNET are always up and always able to resolve the virtual machines hosted in the VNET.</span></span> <span data-ttu-id="a57fe-122">Använd inte Google DNS för den här.</span><span class="sxs-lookup"><span data-stu-id="a57fe-122">Don't use Google DNS for this.</span></span>
> 
> 

<span data-ttu-id="a57fe-123">För hybridsamlingar använder du DNS-servrarna.</span><span class="sxs-lookup"><span data-stu-id="a57fe-123">For hybrid collections you use your own DNS servers.</span></span> <span data-ttu-id="a57fe-124">Anger du dem i ditt nätverk Konfigurationsschemat eller via hanteringsportalen när du skapar ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="a57fe-124">You specify them in your network configuration schema or through the management portal when you create your virtual network.</span></span> <span data-ttu-id="a57fe-125">DNS-servrar som används i den ordning som de anges på ett sätt för växling vid fel (i stället för resursallokering).</span><span class="sxs-lookup"><span data-stu-id="a57fe-125">DNS servers are used in the order that they are specified in a failover manner (as opposed to round robin).</span></span>  
<span data-ttu-id="a57fe-126">Se [namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) Kontrollera DNS-servrar är konfigurerade correcly.</span><span class="sxs-lookup"><span data-stu-id="a57fe-126">Please refer to [Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) to make sure your DNS servers are configured correcly.</span></span>

<span data-ttu-id="a57fe-127">Kontrollera att DNS-servrar för samlingen är tillgängliga och tillgänglig från undernätet som virtuella nätverk som du angav för den här samlingen.</span><span class="sxs-lookup"><span data-stu-id="a57fe-127">Make sure the DNS servers for your collection are accessible and available from the VNET subnet you specified for this collection.</span></span>

<span data-ttu-id="a57fe-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a57fe-128">For example:</span></span>

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definiera din DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a><span data-ttu-id="a57fe-130">Använder du en Active Directory-domänkontrollant i samlingen?</span><span class="sxs-lookup"><span data-stu-id="a57fe-130">Are you using an Active Directory domain controller in your collection?</span></span>
<span data-ttu-id="a57fe-131">För närvarande bara en Active Directory-domän kan associeras med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a57fe-131">Currently only one Active Directory domain can be associated with Azure RemoteApp.</span></span> <span data-ttu-id="a57fe-132">Hybridsamlingen stöder enbart Azure Active Directory-konton som har synkroniserats med DirSync-verktyget från en Windows Server Active Directory-distribution mer specifikt antingen synkroniserats med alternativet för Lösenordssynkronisering eller synkroniseras med Active Directory Federation Services (AD FS) konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="a57fe-132">The hybrid collection supports only Azure Active Directory accounts that have been synced using DirSync tool from a Windows Server Active Directory deployment; specifically, either synced with the Password Synchronization option or synced with Active Directory Federation Services (AD FS) federation configured.</span></span> <span data-ttu-id="a57fe-133">Du måste skapa en anpassad domän som matchar UPN-suffix för domän för din lokala domän och konfigurera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="a57fe-133">You need to create a custom domain that matches the UPN domain suffix for your on-premises domain and set up directory integration.</span></span>

<span data-ttu-id="a57fe-134">Se [konfigurera Active Directory för Azure RemoteApp](remoteapp-ad.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a57fe-134">See [Configuring Active Directory for Azure RemoteApp](remoteapp-ad.md) for more information.</span></span>

<span data-ttu-id="a57fe-135">Kontrollera att domänen informationen är giltiga och domänkontrollanten kan nås från den virtuella datorn skapas i det undernät som används för Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a57fe-135">Make sure the domain details provided are valid and the domain controller is reachable from the VM created in the subnet used for Azure Remote App.</span></span> <span data-ttu-id="a57fe-136">Kontrollera också att angett autentiseringsuppgifterna för tjänstkontot har behörighet att lägga till datorer i den angivna domänen och att det angivna AD-namnet kan matchas från DNS i VNET.</span><span class="sxs-lookup"><span data-stu-id="a57fe-136">Also make sure the service account credentials supplied have permissions to add computers to the provided domain and that the AD name provided can be resolved from the DNS provided in the VNET.</span></span>

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a><span data-ttu-id="a57fe-137">Vilket domännamn som du angav när du skapade din samling?</span><span class="sxs-lookup"><span data-stu-id="a57fe-137">What domain name did you specify when you created your collection?</span></span>
<span data-ttu-id="a57fe-138">Domännamnet du har skapat eller lagts till måste vara ett internt domännamn (inte dina Azure AD domain name) och måste vara i DNS-format som kan lösas (contoso.local).</span><span class="sxs-lookup"><span data-stu-id="a57fe-138">The domain name you created or added must be an internal domain name (not your Azure AD domain name) and must be in resolvable DNS format (contoso.local).</span></span> <span data-ttu-id="a57fe-139">Till exempel du har ett internt namn för Active Directory (contoso.local) och en Active Directory-UPN (contoso.com) – du måste använda det interna namnet när du skapar din samling.</span><span class="sxs-lookup"><span data-stu-id="a57fe-139">For example, you have an Active Directory internal name (contoso.local) and an Active Directory UPN (contoso.com) - you have to use the internal name when you create your collection.</span></span>

