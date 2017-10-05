---
title: Windows-autentisering och Azure MFA Server | Microsoft Docs
description: "Det här är sidan om Azure Multi-Factor Authentication som hjälper dig att distribuera Windows-autentisering och Azure Multi-Factor Authentication Server."
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
ms.openlocfilehash: 7e6384ea8fea686b5cad1a3bc3007252b9cfcd65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a><span data-ttu-id="61654-103">Windows-autentisering och Azure Multi-Factor Authentication Server</span><span class="sxs-lookup"><span data-stu-id="61654-103">Windows Authentication and Azure Multi-Factor Authentication Server</span></span>
<span data-ttu-id="61654-104">Använd avsnittet Windows-autentisering för Azure Multi-Factor Authentication Server för att aktivera och konfigurera Windows-autentisering för program.</span><span class="sxs-lookup"><span data-stu-id="61654-104">Use the Windows Authentication section of the Azure Multi-Factor Authentication Server to enable and configure Windows authentication for applications.</span></span> <span data-ttu-id="61654-105">Ta följande lista i beaktande innan du konfigurerar Windows-autentisering:</span><span class="sxs-lookup"><span data-stu-id="61654-105">Before you set up Windows Authentication, keep the following list in mind:</span></span>

* <span data-ttu-id="61654-106">Efter konfigurationen måste du starta om Azure Multi-Factor Authentication för att Terminal Services ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="61654-106">After setup, reboot the Azure Multi-Factor Authentication for Terminal Services to take effect.</span></span>
* <span data-ttu-id="61654-107">Om ”Kräv Azure Multi-Factor Authentication-användarmatchning” är markerat och du inte finns med i användarlistan kan du inte logga in på datorn efter omstarten.</span><span class="sxs-lookup"><span data-stu-id="61654-107">If ‘Require Azure Multi-Factor Authentication user match’ is checked and you are not in the user list, you will not be able to log into the machine after reboot.</span></span>
* <span data-ttu-id="61654-108">Användningen av tillförlitliga IP-adresser beror på om programmet kan tillhandahålla klient-IP-adressen med autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="61654-108">Trusted IPs is dependent on whether the application can provide the client IP with the authentication.</span></span> <span data-ttu-id="61654-109">För närvarande stöds endast Terminal Services.</span><span class="sxs-lookup"><span data-stu-id="61654-109">Currently only Terminal Services is supported.</span></span>  

> [!NOTE]
> <span data-ttu-id="61654-110">Den här funktionen stöds inte om du vill skydda Terminal Services i Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="61654-110">This feature is not supported to secure Terminal Services on Windows Server 2012 R2.</span></span>

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a><span data-ttu-id="61654-111">Följ stegen nedan om du vill skydda ett program med Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="61654-111">To secure an application with Windows Authentication, use the following procedure.</span></span>
1. <span data-ttu-id="61654-112">Klicka på ikonen för Windows-autentisering i Azure Multi-Factor Authentication Server.</span><span class="sxs-lookup"><span data-stu-id="61654-112">In the Azure Multi-Factor Authentication Server click the Windows Authentication icon.</span></span>
   <span data-ttu-id="61654-113">![Windows-autentisering](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span><span class="sxs-lookup"><span data-stu-id="61654-113">![Windows Authentication](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span></span>
2. <span data-ttu-id="61654-114">Markera kryssrutan **Aktivera Windows-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="61654-114">Check the **Enable Windows Authentication** checkbox.</span></span> <span data-ttu-id="61654-115">Den här rutan är avmarkerad som standard.</span><span class="sxs-lookup"><span data-stu-id="61654-115">By default, this box is unchecked.</span></span>
3. <span data-ttu-id="61654-116">På fliken Program kan administratören konfigurera ett eller flera program för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="61654-116">The Applications tab allows the administrator to configure one or more applications for Windows Authentication.</span></span>
4. <span data-ttu-id="61654-117">Välj en server eller ett program – ange om servern/programmet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="61654-117">Select a server or application – specify whether the server/application is enabled.</span></span> <span data-ttu-id="61654-118">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61654-118">Click **OK**.</span></span>
5. <span data-ttu-id="61654-119">Klicka på **Lägg till...**</span><span class="sxs-lookup"><span data-stu-id="61654-119">Click **Add…**</span></span>
6. <span data-ttu-id="61654-120">På fliken Tillförlitliga IP-adresser kan du välja att hoppa över Azure Multi-Factor Authentication för Windows-sessioner som kommer från specifika IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="61654-120">The Trusted IPs tab allows you to skip Azure Multi-Factor Authentication for Windows sessions originating from specific IPs.</span></span> <span data-ttu-id="61654-121">Om anställda till exempel använder programmet från kontoret och hemifrån kan du välja att du inte vill att deras telefoner ska ringa för Azure Multi-Factor Authentication när de är på kontoret.</span><span class="sxs-lookup"><span data-stu-id="61654-121">For example, if employees use the application from the office and from home, you may decide you don't want their phones ringing for Azure Multi-Factor Authentication while at the office.</span></span> <span data-ttu-id="61654-122">För att göra det anger du kontorets undernät som en tillförlitlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="61654-122">For this, you would specify the office subnet as Trusted IPs entry.</span></span>
7. <span data-ttu-id="61654-123">Klicka på **Lägg till...**</span><span class="sxs-lookup"><span data-stu-id="61654-123">Click **Add…**</span></span>
8. <span data-ttu-id="61654-124">Välj **Enkel IP** om du vill hoppa över en enstaka IP-adress.</span><span class="sxs-lookup"><span data-stu-id="61654-124">Select **Single IP** if you would like to skip a single IP address.</span></span>
9. <span data-ttu-id="61654-125">Välj **IP-intervall** om du vill hoppa över ett helt IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="61654-125">Select **IP Range** if you would like to skip an entire IP range.</span></span> <span data-ttu-id="61654-126">Exempel: 10.63.193.1-10.63.193.100.</span><span class="sxs-lookup"><span data-stu-id="61654-126">Example 10.63.193.1-10.63.193.100.</span></span>
10. <span data-ttu-id="61654-127">Välj **Undernät** om du vill ange ett intervall med IP-adresser med hjälp av undernätsnotation.</span><span class="sxs-lookup"><span data-stu-id="61654-127">Select **Subnet** if you would like to specify a range of IPs using subnet notation.</span></span> <span data-ttu-id="61654-128">Ange undernätets start-IP och välj lämplig nätmask i listrutan.</span><span class="sxs-lookup"><span data-stu-id="61654-128">Enter the subnet's starting IP and pick the appropriate netmask from the drop-down list.</span></span>
11. <span data-ttu-id="61654-129">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61654-129">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61654-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61654-130">Next steps</span></span>

- [<span data-ttu-id="61654-131">Konfigurera VPN-installationer från tredje part för Azure MFA Server</span><span class="sxs-lookup"><span data-stu-id="61654-131">Configure third-party VPN appliances for Azure MFA Server</span></span>](multi-factor-authentication-advanced-vpn-configurations.md)

- [<span data-ttu-id="61654-132">Utöka din befintliga infrastruktur för autentisering med NPS-tillägget för Azure MFA</span><span class="sxs-lookup"><span data-stu-id="61654-132">Augment your existing authentication infrastructure with the NPS extension for Azure MFA</span></span>](multi-factor-authentication-nps-extension.md)
