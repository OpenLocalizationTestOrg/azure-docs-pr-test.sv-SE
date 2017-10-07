---
title: aaaUsing Office med Azure RemoteApp | Microsoft Docs
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
ms.openlocfilehash: d065c1a0a2191c9e2e405264a835cecf791d6ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-office-with-azure-remoteapp"></a>Använda Office med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Har du två alternativ för Office-program i Azure RemoteApp: Office 365 ProPlus eller Office 2013 Professional Plus utvärderingsversion.

**Hej, Visste du att vi har ett nytt, bättre artikel som kommer snart att ersätta det? Checka ut [hur toouse din Office 365-prenumeration med Azure RemoteApp](remoteapp-officesubscription.md). Den omfattar alla hello information du behöver för att använda Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Du kan skapa en RemoteApp-samling med hjälp av Office 365 ProPlus hello-mallavbildningen. Det här alternativet kan du tooextend tooRemoteApp din Office 365-tjänsten. Du måste ha en befintlig plan för prenumeration och användarna måste ha licens för hello Office 365 ProPlus-tjänsten, antingen fristående eller via hello Office 365-tjänstplaner.

RemoteApp har stöd för Office 365 delade aktivering. När du aktiverar delade aktivering och använda hello [distributionsverktyget för Office](http://www.microsoft.com/download/details.aspx?id=36778) för installation, Office 365 ProPlus installeras utan att aktiveras. När en användare loggar in en samling som innehåller Office 365 kontrollerar Office toosee om hello användaren har etablerats för Office 365 ProPlus. Om så är fallet bör det finns Office för tillfälligt aktiverar Office 365 ProPlus - kvarstår den här aktiveringen tills att användare loggar ut från hello-tjänsten.

toouse Office 365 delade aktivering behöver du toocreate en [anpassad mall](remoteapp-create-custom-image.md) och installera Office 365 ProPlus, följa [de här anvisningarna](https://technet.microsoft.com/library/dn782858.aspx).

Du kan hantera användarnas Office 365 licenser på hello [administrationsportalen för Office 365](https://portal.office365.com/). Läs mer om [Office 365-tjänstplaner](http://technet.microsoft.com/library/office-365-plan-options.aspx).  

## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plus utvärderingsversion
Du kan använda hello Office 2013 Professional Plus (test) mallen avbildningen toocreate en RemoteApp-samling under en 30-dagars utvärderingsversion av RemoteApp. Du kan tilldela användare toothis utvärderingsversion samlingen med hjälp av sina Azure Active Directory arbetskonton eller Microsoft-konton. Det krävs ingen ytterligare prenumeration.

Detta är ett bra alternativ tookick hello kolla och få en bra känsla för Office i RemoteApp. Men är det här alternativet avsedd för utvärdering och tester endast. RemoteApp-samlingar som skapats med hjälp av hello Office 2013 Professional Plus (test) mallavbildningen kan inte övergick tooproduction läge och kommer att inaktiveras hello slutet av utvärderingsperioden hello.

## <a name="switching-from-trial-tooproduction"></a>Växla från utvärderingsversionen tooproduction
När du börjar din 30-dagars utvärderingsversion, anger en anteckning i hello RemoteApp-avsnittet i hello portal hur länge du har kvar i hello utvärderingsversionen innan du behöver tootransition tooa betald konto. Du kan aktivera ditt konto och växel tooproduction läge via hello länk i den här anteckningen.

När du aktiverar ditt konto kommer att påverka alla hello RemoteApp-samlingar i ditt konto.

* Samlingar med hello Windows Server 2012 R2 eller hello Office 365 ProPlus mallavbildningarna övergår tooproduction sömlöst. Alla användardata och inställningar, inklusive pågående sessioner, påverkas inte.
* Om du har överfört anpassad mall bilder, övergår också samlingar med hjälp av dessa avbildningar sömlöst.
* hello Office 2013 Professional Plus (test)-mallavbildningen är avsedd för enbart. Samlingar som körs med den här mallavbildningen kan inte vara övergick tooproduction. De placeras i ”inaktiverad” tillstånd.

Om du inte övergår tooproduction läge av hello giltighetstid för din utvärderingsversion, kommer RemoteApp-samlingar att inaktiveras. Oroa dig inte – dina inställningar och användarnas data sparas för en annan 90 dagarna, så du kan aktivera tjänsten och växel tooproduction läget utan att förlora data.

