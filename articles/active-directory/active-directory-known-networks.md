---
title: "Kända nätverk i den klassiska Azure-portalen | Microsoft Docs"
description: "Genom att konfigurera kända nätverk kan undvika du IP-adresser som ägs av din organisation som ingår i inloggning-moduler från flera områden och logga moduler från IP-adresser med misstänkt aktivitetsrapporter."
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
ms.openlocfilehash: e4d51d1d2f09fca34d749879e21d49f785eac35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="known-networks"></a><span data-ttu-id="85337-103">Kända nätverk</span><span class="sxs-lookup"><span data-stu-id="85337-103">Known networks</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="85337-104">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="85337-104">Azure classic portal</span></span>](active-directory-known-networks.md)
> * [<span data-ttu-id="85337-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="85337-105">Azure portal</span></span>](active-directory-known-networks-azure-portal.md)
> 
> 


<span data-ttu-id="85337-106">Du kan använda Azure Active Directory-åtkomst och användningsrapporter insyn i integritet och säkerheten för din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="85337-106">You can use Azure Active Directory's access and usage reports to gain visibility into the integrity and security of your organization’s directory.</span></span> <span data-ttu-id="85337-107">Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan på lämpligt sätt att minska riskerna.</span><span class="sxs-lookup"><span data-stu-id="85337-107">With this information, a directory admin can better determine where possible security risks may lie so that they can adequately plan to mitigate those risks.</span></span>

<span data-ttu-id="85337-108">Det är möjligt som den ”*inloggningar från flera områden*” och ”*inloggningar från IP-adresser med misstänkt aktivitet*” rapporter flaggan felaktigt IP-adresser som faktiskt ägs av organisationen.</span><span class="sxs-lookup"><span data-stu-id="85337-108">It is possible that the “*Sign ins from multiple geographies*” and “*Sign ins from IP addresses with suspicious activity*” reports incorrectly flag IP addresses that are actually owned by your organization.</span></span> 

<span data-ttu-id="85337-109">Detta kan till exempel hända när:</span><span class="sxs-lookup"><span data-stu-id="85337-109">This can, for example, happen when:</span></span> 

* <span data-ttu-id="85337-110">En användare i din Boston office har loggat in via fjärranslutning på ditt datacenter i San Francisco utlöser ”inloggningar från flera områden”-rapport</span><span class="sxs-lookup"><span data-stu-id="85337-110">A user in your Boston office has signed in remotely to your data center in San Francisco triggers the “Sign ins from multiple geographies” report</span></span> 
* <span data-ttu-id="85337-111">En användare i din organisation försöker logga in flera gånger med ett felaktigt lösenord utlösare rapporten ”inloggningar från IP-adresser med misstänkt aktivitet”</span><span class="sxs-lookup"><span data-stu-id="85337-111">A user of your organization tries to sign-on several times with an incorrect password triggers the “Sign ins from IP addresses with suspicious activity” report</span></span> 

<span data-ttu-id="85337-112">För att förhindra att dessa fall genererar vilseledande säkerhetsrapporter bör du lägga till kända IP-adressintervall i listan över organisationens offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="85337-112">To prevent these cases from generating misleading security reports, you should add known IP address ranges to the list of your organization's public IP address.</span></span>    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a><span data-ttu-id="85337-113">Utför följande steg för att lägga till din organisations offentliga IP-adressintervall:</span><span class="sxs-lookup"><span data-stu-id="85337-113">To add your organization’s public IP address ranges, perform the following steps:</span></span>

1. <span data-ttu-id="85337-114">Inloggning till den [Azure-hanteringsportalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="85337-114">Sign-on to the [Azure management portal](https://manage.windowsazure.com).</span></span>

2. <span data-ttu-id="85337-115">I den vänstra rutan klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="85337-115">In the left pane, click **Active Directory**.</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-01.png)

3. <span data-ttu-id="85337-117">I den **Directory** , Välj din katalog.</span><span class="sxs-lookup"><span data-stu-id="85337-117">In the **Directory** tab, select your directory.</span></span>

4. <span data-ttu-id="85337-118">Klicka på menyn högst upp **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="85337-118">In the menu on the top, click **Configure**.</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-02.png)

5. <span data-ttu-id="85337-120">Gå till på fliken Konfigurera **din organisationer offentliga IP-adressintervall**</span><span class="sxs-lookup"><span data-stu-id="85337-120">On the Configure tab, go to **your organizations public IP address ranges**</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-03.png)

6. <span data-ttu-id="85337-122">Klicka på **lägga till kända IP-adressintervall**.</span><span class="sxs-lookup"><span data-stu-id="85337-122">Click **Add Known IP Address Ranges**.</span></span>

7. <span data-ttu-id="85337-123">Lägg till-adressintervall i dialogrutan som visas och klicka sedan på knappen Kontrollera när du är klar.</span><span class="sxs-lookup"><span data-stu-id="85337-123">Add your address ranges in the dialog box that appears, and then click the check button  when you are done.</span></span> 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-04.png)

<span data-ttu-id="85337-125">**Ytterligare resurser:**</span><span class="sxs-lookup"><span data-stu-id="85337-125">**Additional resources:**</span></span>

* [<span data-ttu-id="85337-126">Visa åtkomst- och användningsrapporterna</span><span class="sxs-lookup"><span data-stu-id="85337-126">View your access and usage reports</span></span>](active-directory-view-access-usage-reports.md)
* [<span data-ttu-id="85337-127">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="85337-127">Sign ins from IP addresses with suspicious activity</span></span>](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [<span data-ttu-id="85337-128">Inloggningar från flera geografiska områden</span><span class="sxs-lookup"><span data-stu-id="85337-128">Sign ins from multiple geographies</span></span>](active-directory-reporting-sign-ins-from-multiple-geographies.md)

