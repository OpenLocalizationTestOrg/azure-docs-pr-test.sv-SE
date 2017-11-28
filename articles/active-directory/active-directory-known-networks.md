---
title: "aaaKnown nätverk i hello klassiska Azure-portalen | Microsoft Docs"
description: "Genom att konfigurera kända nätverk kan undvika du IP-adresser som ägs av din organisation som ingår i hello logga moduler från flera områden och logga moduler från IP-adresser med misstänkt aktivitetsrapporter."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: ec636cdda172cd3baeb1e606dd8d6e1949fbc63b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="known-networks"></a><span data-ttu-id="5de8b-103">Kända nätverk</span><span class="sxs-lookup"><span data-stu-id="5de8b-103">Known networks</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5de8b-104">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="5de8b-104">Azure classic portal</span></span>](active-directory-known-networks.md)
> * [<span data-ttu-id="5de8b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5de8b-105">Azure portal</span></span>](active-directory-known-networks-azure-portal.md)
> 
> 


<span data-ttu-id="5de8b-106">Du kan använda Azure Active Directory-åtkomst och användning rapporter toogain insyn i hello integritet och säkerhet för din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="5de8b-106">You can use Azure Active Directory's access and usage reports toogain visibility into hello integrity and security of your organization’s directory.</span></span> <span data-ttu-id="5de8b-107">Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan rätt toomitigate riskerna.</span><span class="sxs-lookup"><span data-stu-id="5de8b-107">With this information, a directory admin can better determine where possible security risks may lie so that they can adequately plan toomitigate those risks.</span></span>

<span data-ttu-id="5de8b-108">Det är möjligt att hello ”*inloggningar från flera områden*” och ”*inloggningar från IP-adresser med misstänkt aktivitet*” rapporter flaggan felaktigt IP-adresser som faktiskt ägs av ditt organisation.</span><span class="sxs-lookup"><span data-stu-id="5de8b-108">It is possible that hello “*Sign ins from multiple geographies*” and “*Sign ins from IP addresses with suspicious activity*” reports incorrectly flag IP addresses that are actually owned by your organization.</span></span> 

<span data-ttu-id="5de8b-109">Detta kan till exempel hända när:</span><span class="sxs-lookup"><span data-stu-id="5de8b-109">This can, for example, happen when:</span></span> 

* <span data-ttu-id="5de8b-110">En användare på kontoret Boston har loggat in via fjärranslutning tooyour datacenter i San Francisco utlösare hello ”logga moduler från flera områden” rapport</span><span class="sxs-lookup"><span data-stu-id="5de8b-110">A user in your Boston office has signed in remotely tooyour data center in San Francisco triggers hello “Sign ins from multiple geographies” report</span></span> 
* <span data-ttu-id="5de8b-111">En användare i din organisation försöker toosign på flera gånger med ett felaktigt lösenord utlösare hello ”logga moduler från IP-adresser med misstänkt aktivitet” rapport</span><span class="sxs-lookup"><span data-stu-id="5de8b-111">A user of your organization tries toosign-on several times with an incorrect password triggers hello “Sign ins from IP addresses with suspicious activity” report</span></span> 

<span data-ttu-id="5de8b-112">rapporter om tooprevent dessa fall genererar vilseledande säkerheten, bör du lägga till kända IP-adressintervall toohello lista över organisationens offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5de8b-112">tooprevent these cases from generating misleading security reports, you should add known IP address ranges toohello list of your organization's public IP address.</span></span>    

### <a name="tooadd-your-organizations-public-ip-address-ranges-perform-hello-following-steps"></a><span data-ttu-id="5de8b-113">tooadd din organisations offentliga IP-adressintervall, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5de8b-113">tooadd your organization’s public IP address ranges, perform hello following steps:</span></span>

1. <span data-ttu-id="5de8b-114">Inloggning toohello [Azure-hanteringsportalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5de8b-114">Sign-on toohello [Azure management portal](https://manage.windowsazure.com).</span></span>

2. <span data-ttu-id="5de8b-115">Hello vänster klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5de8b-115">In hello left pane, click **Active Directory**.</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-01.png)

3. <span data-ttu-id="5de8b-117">I hello **Directory** , Välj din katalog.</span><span class="sxs-lookup"><span data-stu-id="5de8b-117">In hello **Directory** tab, select your directory.</span></span>

4. <span data-ttu-id="5de8b-118">Hello-menyn överst hello **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="5de8b-118">In hello menu on hello top, click **Configure**.</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-02.png)

5. <span data-ttu-id="5de8b-120">På fliken Konfigurera hello gå för**din organisationer offentliga IP-adressintervall**</span><span class="sxs-lookup"><span data-stu-id="5de8b-120">On hello Configure tab, go too**your organizations public IP address ranges**</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-03.png)

6. <span data-ttu-id="5de8b-122">Klicka på **lägga till kända IP-adressintervall**.</span><span class="sxs-lookup"><span data-stu-id="5de8b-122">Click **Add Known IP Address Ranges**.</span></span>

7. <span data-ttu-id="5de8b-123">Lägg till din adressintervall hello i dialogrutan som visas och klicka sedan på knappen för hello när du är klar.</span><span class="sxs-lookup"><span data-stu-id="5de8b-123">Add your address ranges in hello dialog box that appears, and then click hello check button  when you are done.</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-04.png)

<span data-ttu-id="5de8b-125">**Ytterligare resurser:**</span><span class="sxs-lookup"><span data-stu-id="5de8b-125">**Additional resources:**</span></span>

* [<span data-ttu-id="5de8b-126">Visa åtkomst- och användningsrapporterna</span><span class="sxs-lookup"><span data-stu-id="5de8b-126">View your access and usage reports</span></span>](active-directory-view-access-usage-reports.md)
* [<span data-ttu-id="5de8b-127">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="5de8b-127">Sign ins from IP addresses with suspicious activity</span></span>](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [<span data-ttu-id="5de8b-128">Inloggningar från flera geografiska områden</span><span class="sxs-lookup"><span data-stu-id="5de8b-128">Sign ins from multiple geographies</span></span>](active-directory-reporting-sign-ins-from-multiple-geographies.md)

