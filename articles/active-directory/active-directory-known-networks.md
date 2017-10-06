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
# <a name="known-networks"></a>Kända nätverk

> [!div class="op_single_selector"]
> * [Klassisk Azure-portal](active-directory-known-networks.md)
> * [Azure Portal](active-directory-known-networks-azure-portal.md)
> 
> 


Du kan använda Azure Active Directory-åtkomst och användning rapporter toogain insyn i hello integritet och säkerhet för din organisations katalog. Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan rätt toomitigate riskerna.

Det är möjligt att hello ”*inloggningar från flera områden*” och ”*inloggningar från IP-adresser med misstänkt aktivitet*” rapporter flaggan felaktigt IP-adresser som faktiskt ägs av ditt organisation. 

Detta kan till exempel hända när: 

* En användare på kontoret Boston har loggat in via fjärranslutning tooyour datacenter i San Francisco utlösare hello ”logga moduler från flera områden” rapport 
* En användare i din organisation försöker toosign på flera gånger med ett felaktigt lösenord utlösare hello ”logga moduler från IP-adresser med misstänkt aktivitet” rapport 

rapporter om tooprevent dessa fall genererar vilseledande säkerheten, bör du lägga till kända IP-adressintervall toohello lista över organisationens offentlig IP-adress.    

### <a name="tooadd-your-organizations-public-ip-address-ranges-perform-hello-following-steps"></a>tooadd din organisations offentliga IP-adressintervall, utföra hello följande steg:

1. Inloggning toohello [Azure-hanteringsportalen](https://manage.windowsazure.com).

2. Hello vänster klickar du på **Active Directory**. 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-01.png)

3. I hello **Directory** , Välj din katalog.

4. Hello-menyn överst hello **konfigurera**. 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-02.png)

5. På fliken Konfigurera hello gå för**din organisationer offentliga IP-adressintervall** 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-03.png)

6. Klicka på **lägga till kända IP-adressintervall**.

7. Lägg till din adressintervall hello i dialogrutan som visas och klicka sedan på knappen för hello när du är klar. 

    ![Kända nätverk](./media/active-directory-known-networks/known-netwoks-04.png)

**Ytterligare resurser:**

* [Visa åtkomst- och användningsrapporterna](active-directory-view-access-usage-reports.md)
* [Inloggningar från IP-adresser med misstänkt aktivitet](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Inloggningar från flera geografiska områden](active-directory-reporting-sign-ins-from-multiple-geographies.md)

