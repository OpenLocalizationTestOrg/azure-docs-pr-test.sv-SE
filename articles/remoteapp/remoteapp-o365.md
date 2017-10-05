---
title: "Använda Office med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur Office och Azure RemoteApp tillsammans"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: f1773baf-8aa1-423c-a2f9-e4679e0463d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a776d877c764109f15c1025db2be3114eea2130a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-office-with-azure-remoteapp"></a>Använda Office med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Har du två alternativ för Office-program i Azure RemoteApp: Office 365 ProPlus eller Office 2013 Professional Plus utvärderingsversion.

**Hej, Visste du att vi har ett nytt, bättre artikel som kommer snart att ersätta det? Checka ut [hur du använder Office 365-prenumeration med Azure RemoteApp](remoteapp-officesubscription.md). Den omfattar all information du behöver för att använda Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Du kan skapa en RemoteApp-samling med hjälp av Office 365 ProPlus mallavbildningen. Det här alternativet kan du utöka din Office 365-tjänst till RemoteApp. Du måste ha en befintlig plan för prenumeration och användarna måste ha licens för Office 365 ProPlus-tjänsten, antingen fristående eller via Office 365 tjänsten planer.

RemoteApp har stöd för Office 365 delade aktivering. När du aktiverar delade aktivering och använda den [distributionsverktyget för Office](http://www.microsoft.com/download/details.aspx?id=36778) för installation, Office 365 ProPlus installeras utan att aktiveras. När en användare loggar in en samling som innehåller Office 365 kontrollerar Office om användaren har etablerats för Office 365 ProPlus. Om så är fallet bör det finns Office för tillfälligt aktiverar Office 365 ProPlus - kvarstår den här aktiveringen tills att användare loggar ut från tjänsten.

Om du vill använda Office 365 delade aktivering, måste du skapa en [anpassad mall](remoteapp-create-custom-image.md) och installera Office 365 ProPlus, följa [de här anvisningarna](https://technet.microsoft.com/library/dn782858.aspx).

Du kan hantera användarnas Office 365 licenser på den [administrationsportalen för Office 365](https://portal.office365.com/). Läs mer om [Office 365-tjänstplaner](http://technet.microsoft.com/library/office-365-plan-options.aspx).  

## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plus utvärderingsversion
Du kan använda Office 2013 Professional Plus (test) mallavbildningen för att skapa en RemoteApp-samling under en 30-dagars utvärderingsversion av RemoteApp. Du kan tilldela användare till den här utvärderingsversionen samlingen med hjälp av sina Azure Active Directory arbetskonton eller Microsoft-konton. Det krävs ingen ytterligare prenumeration.

Det här är ett bra alternativ för att kolla startar och få en bra känsla för Office i RemoteApp. Men är det här alternativet avsedd för utvärdering och tester endast. RemoteApp-samlingar som skapats med hjälp av Office 2013 Professional Plus (test) mallavbildningen kan inte överföras till Produktionsläge och kommer att inaktiveras i slutet av utvärderingsperioden.

## <a name="switching-from-trial-to-production"></a>Växla från test till produktion
När du börjar din 30-dagars utvärderingsversion, en anteckning i avsnittet RemoteApp i portalen får du veta hur länge du har kvar i utvärderingsversionen innan du behöver övergång till ett betalt konto. Du kan aktivera ditt konto och växla till Produktionsläge med hjälp av länken i den här anteckningen.

När du aktiverar ditt konto kommer att påverka alla RemoteApp-samlingar i ditt konto.

* Samlingar som kör Windows Server 2012 R2 eller Office 365 ProPlus mallavbildningarna övergår sömlöst till produktionen. Alla användardata och inställningar, inklusive pågående sessioner, påverkas inte.
* Om du har överfört anpassad mall bilder, övergår också samlingar med hjälp av dessa avbildningar sömlöst.
* Office 2013 Professional Plus (test) mallavbildningen är avsedd för enbart. Samlingar som körs med den här mallavbildningen kan inte överföras till produktion. De placeras i ”inaktiverad” tillstånd.

Om du inte övergår till Produktionsläge av din utvärderingsversion upphör att gälla, kommer att inaktiveras RemoteApp-samlingar. Oroa dig inte – dina inställningar och användarnas data sparas för en annan 90 dagarna, så att du kan fortfarande aktivera tjänsten och växla till Produktionsläge utan att förlora data.

