---
title: aaaWindows autentisering och Azure MFA-Server | Microsoft Docs
description: "Detta är hello Azure Multi-Factor authentication sida som är till hjälp vid distribution av Windows-autentisering och Azure Multi-Factor Authentication-servern."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 0fc38fd751966bf883d4eae7c48055988922af80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a><span data-ttu-id="89127-103">Windows-autentisering och Azure Multi-Factor Authentication Server</span><span class="sxs-lookup"><span data-stu-id="89127-103">Windows Authentication and Azure Multi-Factor Authentication Server</span></span>
<span data-ttu-id="89127-104">Använd hello avsnittet för Windows-autentisering av hello Azure Multi-Factor Authentication-servern tooenable och konfigurera Windows-autentisering för program.</span><span class="sxs-lookup"><span data-stu-id="89127-104">Use hello Windows Authentication section of hello Azure Multi-Factor Authentication Server tooenable and configure Windows authentication for applications.</span></span> <span data-ttu-id="89127-105">Innan du konfigurerar Windows-autentisering, hålla hello följande lista i åtanke:</span><span class="sxs-lookup"><span data-stu-id="89127-105">Before you set up Windows Authentication, keep hello following list in mind:</span></span>

* <span data-ttu-id="89127-106">Starta om hello Azure Multi-Factor Authentication för Terminal Services tootake effekt efter installationen.</span><span class="sxs-lookup"><span data-stu-id="89127-106">After setup, reboot hello Azure Multi-Factor Authentication for Terminal Services tootake effect.</span></span>
* <span data-ttu-id="89127-107">Om 'Azure Multi-Factor Authentication-användarmatchning är markerat och du inte är i hello användarlistan, kommer inte att kunna toolog till hello dator efter omstart.</span><span class="sxs-lookup"><span data-stu-id="89127-107">If ‘Require Azure Multi-Factor Authentication user match’ is checked and you are not in hello user list, you will not be able toolog into hello machine after reboot.</span></span>
* <span data-ttu-id="89127-108">Betrodda IP-adresser är beroende av att hello programmet kan uppge hello klientens IP-Adressen med hello-autentisering.</span><span class="sxs-lookup"><span data-stu-id="89127-108">Trusted IPs is dependent on whether hello application can provide hello client IP with hello authentication.</span></span> <span data-ttu-id="89127-109">För närvarande stöds endast Terminal Services.</span><span class="sxs-lookup"><span data-stu-id="89127-109">Currently only Terminal Services is supported.</span></span>  

> [!NOTE]
> <span data-ttu-id="89127-110">Den här funktionen är inte stöds toosecure Terminal Services i Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="89127-110">This feature is not supported toosecure Terminal Services on Windows Server 2012 R2.</span></span>

## <a name="toosecure-an-application-with-windows-authentication-use-hello-following-procedure"></a><span data-ttu-id="89127-111">toosecure ett program med Windows-autentisering, Använd hello nedan.</span><span class="sxs-lookup"><span data-stu-id="89127-111">toosecure an application with Windows Authentication, use hello following procedure.</span></span>
1. <span data-ttu-id="89127-112">Klicka på ikonen för Windows-autentisering av hello i hello Azure Multi-Factor Authentication-servern.</span><span class="sxs-lookup"><span data-stu-id="89127-112">In hello Azure Multi-Factor Authentication Server click hello Windows Authentication icon.</span></span>
   <span data-ttu-id="89127-113">![Windows-autentisering](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span><span class="sxs-lookup"><span data-stu-id="89127-113">![Windows Authentication](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span></span>
2. <span data-ttu-id="89127-114">Kontrollera hello **aktivera Windows-autentisering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="89127-114">Check hello **Enable Windows Authentication** checkbox.</span></span> <span data-ttu-id="89127-115">Den här rutan är avmarkerad som standard.</span><span class="sxs-lookup"><span data-stu-id="89127-115">By default, this box is unchecked.</span></span>
3. <span data-ttu-id="89127-116">hello på fliken program kan Hej administratör tooconfigure ett eller flera program för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="89127-116">hello Applications tab allows hello administrator tooconfigure one or more applications for Windows Authentication.</span></span>
4. <span data-ttu-id="89127-117">Välj en server eller ett program – ange om hello serverprogrammet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="89127-117">Select a server or application – specify whether hello server/application is enabled.</span></span> <span data-ttu-id="89127-118">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89127-118">Click **OK**.</span></span>
5. <span data-ttu-id="89127-119">Klicka på **Lägg till...**</span><span class="sxs-lookup"><span data-stu-id="89127-119">Click **Add…**</span></span>
6. <span data-ttu-id="89127-120">hello tillförlitliga IP-adresser fliken kan du tooskip Azure Multi-Factor Authentication för Windows-sessioner som kommer från särskilda IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="89127-120">hello Trusted IPs tab allows you tooskip Azure Multi-Factor Authentication for Windows sessions originating from specific IPs.</span></span> <span data-ttu-id="89127-121">Till exempel anställda använder programmet hello från hello office och hemmet, du kan då välja du inte vill att deras telefoner ska ringa för Azure Multi-Factor Authentication på hello office.</span><span class="sxs-lookup"><span data-stu-id="89127-121">For example, if employees use hello application from hello office and from home, you may decide you don't want their phones ringing for Azure Multi-Factor Authentication while at hello office.</span></span> <span data-ttu-id="89127-122">För att göra detta skulle du ange hello office undernät som betrodda IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="89127-122">For this, you would specify hello office subnet as Trusted IPs entry.</span></span>
7. <span data-ttu-id="89127-123">Klicka på **Lägg till...**</span><span class="sxs-lookup"><span data-stu-id="89127-123">Click **Add…**</span></span>
8. <span data-ttu-id="89127-124">Välj **enda IP** om du vill att tooskip en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="89127-124">Select **Single IP** if you would like tooskip a single IP address.</span></span>
9. <span data-ttu-id="89127-125">Välj **IP-intervall** om du vill att tooskip ett hela IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="89127-125">Select **IP Range** if you would like tooskip an entire IP range.</span></span> <span data-ttu-id="89127-126">Exempel: 10.63.193.1-10.63.193.100.</span><span class="sxs-lookup"><span data-stu-id="89127-126">Example 10.63.193.1-10.63.193.100.</span></span>
10. <span data-ttu-id="89127-127">Välj **undernät** om du vill att toospecify ett intervall av IP-adresser med hjälp av undernät.</span><span class="sxs-lookup"><span data-stu-id="89127-127">Select **Subnet** if you would like toospecify a range of IPs using subnet notation.</span></span> <span data-ttu-id="89127-128">Ange hello undernät Start IP och välj hello lämplig nätmask hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="89127-128">Enter hello subnet's starting IP and pick hello appropriate netmask from hello drop-down list.</span></span>
11. <span data-ttu-id="89127-129">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89127-129">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89127-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89127-130">Next steps</span></span>

- [<span data-ttu-id="89127-131">Konfigurera VPN-installationer från tredje part för Azure MFA Server</span><span class="sxs-lookup"><span data-stu-id="89127-131">Configure third-party VPN appliances for Azure MFA Server</span></span>](multi-factor-authentication-advanced-vpn-configurations.md)

- [<span data-ttu-id="89127-132">Utöka din befintliga infrastruktur för autentisering med hello NPS-tillägg för Azure MFA</span><span class="sxs-lookup"><span data-stu-id="89127-132">Augment your existing authentication infrastructure with hello NPS extension for Azure MFA</span></span>](multi-factor-authentication-nps-extension.md)
