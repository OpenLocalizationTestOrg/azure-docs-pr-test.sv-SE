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
# <a name="known-networks"></a>Kända nätverk

> [!div class="op_single_selector"]
> * [Klassisk Azure-portal](active-directory-known-networks.md)
> * [Azure Portal](active-directory-known-networks-azure-portal.md)
> 
> 


Du kan använda Azure Active Directory-åtkomst och användningsrapporter insyn i integritet och säkerheten för din organisations katalog. Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan på lämpligt sätt att minska riskerna.

Det är möjligt som den ”*inloggningar från flera områden*” och ”*inloggningar från IP-adresser med misstänkt aktivitet*” rapporter flaggan felaktigt IP-adresser som faktiskt ägs av organisationen. 

Detta kan till exempel hända när: 

* En användare i din Boston office har loggat in via fjärranslutning på ditt datacenter i San Francisco utlöser ”inloggningar från flera områden”-rapport 
* En användare i din organisation försöker logga in flera gånger med ett felaktigt lösenord utlösare rapporten ”inloggningar från IP-adresser med misstänkt aktivitet” 

För att förhindra att dessa fall genererar vilseledande säkerhetsrapporter bör du lägga till kända IP-adressintervall i listan över organisationens offentlig IP-adress.    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Utför följande steg för att lägga till din organisations offentliga IP-adressintervall:

1. Inloggning till den [Azure-hanteringsportalen](https://manage.windowsazure.com).

2. I den vänstra rutan klickar du på **Active Directory**. 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-01.png)

3. I den **Directory** , Välj din katalog.

4. Klicka på menyn högst upp **konfigurera**. 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-02.png)

5. Gå till på fliken Konfigurera **din organisationer offentliga IP-adressintervall** 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-03.png)

6. Klicka på **lägga till kända IP-adressintervall**.

7. Lägg till-adressintervall i dialogrutan som visas och klicka sedan på knappen Kontrollera när du är klar. 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-04.png)

**Ytterligare resurser:**

* [Visa åtkomst- och användningsrapporterna](active-directory-view-access-usage-reports.md)
* [Inloggningar från IP-adresser med misstänkt aktivitet](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Inloggningar från flera geografiska områden](active-directory-reporting-sign-ins-from-multiple-geographies.md)

