---
title: aaaNamed platser i Azure Active Directory | Microsoft Docs
description: "Genom att konfigurera med namnet platser kan du undvika med IP-adresser som ägs av organisationen generera falska positiva identifieringar för hello omöjligt att resa tooatypical platser riskerar händelsetyp."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 591e4b94b2ec9d45e20c01711e922f9972e047e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Sökvägarna i Azure Active Directory

Du kan använda hello med namnet platser funktion i Azure Active Directory, för att sätta etiketter tillförlitliga IP-adressintervall i ditt företag. I din miljö kan du använda sökvägarna i hello kontexten hello upptäcka [riskerar händelser](active-directory-reporting-risk-events.md). hello funktionen minskar hello antalet rapporterade falska positiva identifieringar för hello *omöjligt att resa tooatypical platser* riskerar händelsetyp. 

## <a name="configuration"></a>Konfiguration

tooconfigure en namngiven plats:

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som global administratör.

2. Hello vänster klickar du på **Azure Active Directory**.

    ![hello Azure Active Directory-länken i hello till vänster](./media/active-directory-named-locations/01.png)

3. På hello **Azure Active Directory** bladet i hello **säkerhet** klickar du på **villkorlig åtkomst**.

    ![hello kommandot för villkorlig åtkomst](./media/active-directory-named-locations/05.png)


4. På hello **villkorlig åtkomst** bladet i hello **hantera** klickar du på **med namnet platser**.

    ![hello namngivna platser kommando](./media/active-directory-named-locations/06.png)


5. På hello **med namnet platser** bladet, klickar du på **ny plats**.

    ![hello kommandot ny plats](./media/active-directory-named-locations/07.png)


6. På hello **ny** bladet hello följande:

    ![hello nytt blad](./media/active-directory-named-locations/08.png)

    a. I hello **namn** Skriv ett namn för din namngiven plats.

    b. I hello **IP-adressintervall** skriver ett IP-adressintervall. hello IP-intervallet måste toobe i hello *Classless Inter-Domain Routing* (CIDR)-format.  

    c. Klicka på **Skapa**.



## <a name="what-you-should-know"></a>Vad du bör känna till

**Massuppdateringar**: när du skapar eller uppdaterar sökvägarna för massuppdateringar, du kan överföra eller hämta en CSV-fil med hello IP-adressintervall. Överföra lägger till hello IP-adressintervall i hello toohello listan över filer i stället för att skriva över hello-listan.

![hello ladda upp och hämta länkar](./media/active-directory-named-locations/09.png)


**Begränsningar**: du kan definiera högst 60 sökvägarna med en IP-adressintervall som tilldelats tooeach av dem. Om du har en namngiven plats som har konfigurerats kan du definiera too500 IP-adressintervall för den.


## <a name="next-steps"></a>Nästa steg

toolearn mer om riskhändelser, se [Azure Active Directory riskhändelser](active-directory-reporting-risk-events.md).

