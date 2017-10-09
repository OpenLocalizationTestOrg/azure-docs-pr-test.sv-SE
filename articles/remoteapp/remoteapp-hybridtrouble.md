---
title: Skapa RemoteApp-hybridsamlingar aaaTroubleshoot | Microsoft Docs
description: "Lär dig hur tootroubleshoot RemoteApp hybrid samling fel vid skapande av"
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
ms.openlocfilehash: cc426f24bd0c349a8862d54acbafa9cf84446f4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a><span data-ttu-id="d31f6-103">Felsök att skapa hybridsamlingar i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="d31f6-103">Troubleshoot creating Azure RemoteApp hybrid collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d31f6-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="d31f6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d31f6-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="d31f6-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d31f6-106">En hybridsamling är värd för och lagrar data i hello Azure-molnet men kan användare komma åt data och resurser i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="d31f6-106">A hybrid collection is hosted in and stores data in hello Azure cloud but also lets users access data and resources stored on your local network.</span></span> <span data-ttu-id="d31f6-107">Användarna kan öppna programmen genom att logga in med sina inloggningsuppgifter till företaget , synkroniserat eller federerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d31f6-107">Users can access apps by logging in with their corporate credentials synchronized or federated with Azure Active Directory.</span></span> <span data-ttu-id="d31f6-108">Du kan distribuera en hybridsamling som använder ett befintligt virtuellt Azure-nätverk eller skapa ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d31f6-108">You can deploy a hybrid collection that uses an existing Azure Virtual Network, or you can create a new virtual network.</span></span> <span data-ttu-id="d31f6-109">Vi rekommenderar att du skapar eller använder ett undernät för virtuellt nätverk med ett CIDR-intervall som är tillräckligt stor för förväntade framtida tillväxt för Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="d31f6-109">We recommend that you create or use a virtual network subnet with a CIDR range large enough for expected future growth for Azure RemoteApp.</span></span>

<span data-ttu-id="d31f6-110">Samlingen ännu inte har skapats?</span><span class="sxs-lookup"><span data-stu-id="d31f6-110">Haven't created your collection yet?</span></span> <span data-ttu-id="d31f6-111">Se [skapa en hybridsamling](remoteapp-create-hybrid-deployment.md) hello anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d31f6-111">See [Create a hybrid collection](remoteapp-create-hybrid-deployment.md) for hello steps.</span></span>

<span data-ttu-id="d31f6-112">Om du har problem med att skapa din samling, eller om hello samlingen inte fungerar hello sätt som du tror att det ska, se hello följande information.</span><span class="sxs-lookup"><span data-stu-id="d31f6-112">If you are having trouble creating your collection, or if hello collection isn't working hello way you think it should, check out hello following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="d31f6-113">Bilden är ogiltig</span><span class="sxs-lookup"><span data-stu-id="d31f6-113">Your image is invalid</span></span>
<span data-ttu-id="d31f6-114">Om du ser ett meddelande som ”GoldImageInvalid” när du väntar Azure tooprovision din samling, innebär det att mallavbildningen inte uppfyller hello [definierat avbildningen krav](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="d31f6-114">If you see a message like, "GoldImageInvalid" when you are waiting for Azure tooprovision your collection, it means that your template image doesn't meet hello [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="d31f6-115">Därför gå läsa de [krav](remoteapp-imagereqs.md), åtgärda avbildningen och försök toocreate samlingen igen.</span><span class="sxs-lookup"><span data-stu-id="d31f6-115">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try toocreate your collection again.</span></span>

## <a name="does-your-vnet-have-network-security-groups-defined"></a><span data-ttu-id="d31f6-116">Har ditt VNET nätverkssäkerhetsgrupper definierats?</span><span class="sxs-lookup"><span data-stu-id="d31f6-116">Does your VNET have network security groups defined?</span></span>
<span data-ttu-id="d31f6-117">Om du har nätverkssäkerhetsgrupper som definierats i hello undernät som du använder för samlingen, se till att dessa [URL: er och portar](remoteapp-ports.md) är tillgänglig från undernätet.</span><span class="sxs-lookup"><span data-stu-id="d31f6-117">If you have network security groups defined on hello subnet you are using for your collection, make sure these [URLs and ports](remoteapp-ports.md) are accessible from within your subnet.</span></span>

<span data-ttu-id="d31f6-118">Du kan lägga till ytterligare nätverk Säkerhet grupper toohello virtuella datorer distribueras av du hello undernät för bättre kontroll.</span><span class="sxs-lookup"><span data-stu-id="d31f6-118">You can add additional network security groups toohello VMs deployed by you in hello subnet for tighter control.</span></span>

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a><span data-ttu-id="d31f6-119">Använder du DNS-servrar?</span><span class="sxs-lookup"><span data-stu-id="d31f6-119">Are you using your own DNS servers?</span></span> <span data-ttu-id="d31f6-120">Och de som är tillgängliga från ditt VNET undernät?</span><span class="sxs-lookup"><span data-stu-id="d31f6-120">And are they accessible from your VNET subnet?</span></span>
> [!NOTE]
> <span data-ttu-id="d31f6-121">Du har toomake att hello DNS-servrar i ditt VNET alltid är igång och alltid kan tooresolve hello virtuella datorer som finns i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d31f6-121">You have toomake sure hello DNS servers in your VNET are always up and always able tooresolve hello virtual machines hosted in hello VNET.</span></span> <span data-ttu-id="d31f6-122">Använd inte Google DNS för den här.</span><span class="sxs-lookup"><span data-stu-id="d31f6-122">Don't use Google DNS for this.</span></span>
> 
> 

<span data-ttu-id="d31f6-123">För hybridsamlingar använder du DNS-servrarna.</span><span class="sxs-lookup"><span data-stu-id="d31f6-123">For hybrid collections you use your own DNS servers.</span></span> <span data-ttu-id="d31f6-124">Anger du dem i ditt nätverk Konfigurationsschemat eller via hello management portal när du skapar ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d31f6-124">You specify them in your network configuration schema or through hello management portal when you create your virtual network.</span></span> <span data-ttu-id="d31f6-125">DNS-servrar som används i hello ordning som de anges i ett failover-sätt (som skillnad från tooround robin).</span><span class="sxs-lookup"><span data-stu-id="d31f6-125">DNS servers are used in hello order that they are specified in a failover manner (as opposed tooround robin).</span></span>  
<span data-ttu-id="d31f6-126">Se för[namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake till DNS-servrarna är konfigurerade correcly.</span><span class="sxs-lookup"><span data-stu-id="d31f6-126">Please refer too[Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake sure your DNS servers are configured correcly.</span></span>

<span data-ttu-id="d31f6-127">Kontrollera att hello DNS-servrar för samlingen är tillgängliga och från hello VNET undernät som du angav för den här samlingen.</span><span class="sxs-lookup"><span data-stu-id="d31f6-127">Make sure hello DNS servers for your collection are accessible and available from hello VNET subnet you specified for this collection.</span></span>

<span data-ttu-id="d31f6-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d31f6-128">For example:</span></span>

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definiera din DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a><span data-ttu-id="d31f6-130">Använder du en Active Directory-domänkontrollant i samlingen?</span><span class="sxs-lookup"><span data-stu-id="d31f6-130">Are you using an Active Directory domain controller in your collection?</span></span>
<span data-ttu-id="d31f6-131">För närvarande bara en Active Directory-domän kan associeras med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="d31f6-131">Currently only one Active Directory domain can be associated with Azure RemoteApp.</span></span> <span data-ttu-id="d31f6-132">Hej hybridsamlingen stöder enbart Azure Active Directory-konton som har synkroniserats med DirSync-verktyget från en Windows Server Active Directory-distribution mer specifikt antingen synkroniserats med alternativet för hello Lösenordssynkronisering eller synkroniseras med Active Directory Federation Services (AD FS) konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="d31f6-132">hello hybrid collection supports only Azure Active Directory accounts that have been synced using DirSync tool from a Windows Server Active Directory deployment; specifically, either synced with hello Password Synchronization option or synced with Active Directory Federation Services (AD FS) federation configured.</span></span> <span data-ttu-id="d31f6-133">Du behöver toocreate anpassade domäner som matchar hello UPN domänsuffixet för din lokala domän och konfigurera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="d31f6-133">You need toocreate a custom domain that matches hello UPN domain suffix for your on-premises domain and set up directory integration.</span></span>

<span data-ttu-id="d31f6-134">Se [konfigurera Active Directory för Azure RemoteApp](remoteapp-ad.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d31f6-134">See [Configuring Active Directory for Azure RemoteApp](remoteapp-ad.md) for more information.</span></span>

<span data-ttu-id="d31f6-135">Kontrollera att hello domän informationen är giltiga och hello domänkontrollanten är nåbar från hello skapas den virtuella datorn i hello undernät som används för Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="d31f6-135">Make sure hello domain details provided are valid and hello domain controller is reachable from hello VM created in hello subnet used for Azure Remote App.</span></span> <span data-ttu-id="d31f6-136">Kontrollera också att hello-tjänstkontot autentiseringsuppgifterna har behörigheter tooadd datorer toohello tillhandahållit domän och som hello Annonsnamn angivna kan matchas från hello DNS i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d31f6-136">Also make sure hello service account credentials supplied have permissions tooadd computers toohello provided domain and that hello AD name provided can be resolved from hello DNS provided in hello VNET.</span></span>

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a><span data-ttu-id="d31f6-137">Vilket domännamn som du angav när du skapade din samling?</span><span class="sxs-lookup"><span data-stu-id="d31f6-137">What domain name did you specify when you created your collection?</span></span>
<span data-ttu-id="d31f6-138">hello domännamnet du har skapat eller lagts till måste vara ett internt domännamn (inte dina Azure AD domain name) och måste vara i DNS-format som kan lösas (contoso.local).</span><span class="sxs-lookup"><span data-stu-id="d31f6-138">hello domain name you created or added must be an internal domain name (not your Azure AD domain name) and must be in resolvable DNS format (contoso.local).</span></span> <span data-ttu-id="d31f6-139">Till exempel du har ett internt namn för Active Directory (contoso.local) och en Active Directory-UPN (contoso.com) – du har toouse hello interna namn när du skapar din samling.</span><span class="sxs-lookup"><span data-stu-id="d31f6-139">For example, you have an Active Directory internal name (contoso.local) and an Active Directory UPN (contoso.com) - you have toouse hello internal name when you create your collection.</span></span>

