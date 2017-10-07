---
title: aaaManage Azure Automation-konto | Microsoft Docs
description: "Den här artikeln beskriver hur toomanage hello konfigurationen av ditt Automation-konto som certifikatförnyelse, borttagning och felaktig konfiguration."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 75e41f601a138d4e8853aa79dcbab6696a5e9fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-automation-account"></a>Hantera Azure Automation-konto
Någon gång innan det Automation-kontot upphör att gälla måste toorenew hello certifikat. Om du tror att hello kör som-konto har komprometterats kan du ta bort och skapa den igen. Det här avsnittet beskrivs hur tooperform dessa åtgärder.

## <a name="self-signed-certificate-renewal"></a>Förnya självsignerade certifikat
hello självsignerat certifikat som du skapade för hello kör som-konto går ut ett år från hello dag skapas. Du kan förnya det när som helst innan det upphör att gälla. När du förnyar det är hello aktuella giltiga certifikat kvarhållna tooensure att alla runbooks som lagts i kö upp eller aktivt körs och som autentiserar med hello kör som-konto inte påverkas negativt. hello intyg är giltigt fram till dess förfallodatum.

> [!NOTE]
> Om du har konfigurerat ditt Automation kör som-konto toouse ett certifikat utfärdat av en enterprise-certifikatutfärdare och du använder det här alternativet, ersätts hello företagscertifikat av ett självsignerat certifikat.

toorenew hello certifikat, hello följande:

1. Öppna hello Automation-konto i hello Azure-portalen.

2. På hello **Automation-konto** bladet i hello **konto egenskaper** rutan under **kontoinställningar**väljer **kör som-konton**.

    ![Egenskapsrutan för Automation-konto](media/automation-manage-account/automation-account-properties-pane.png)
3. På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller hello klassiska kör som-konto som du vill ha toorenew hello certifikat för.

4. På hello **egenskaper** bladet för hello valt konto, klickar du på **förnya certifikat**.

    ![Förnya certifikat för Kör som-konto](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. Medan hello-certifikat förnyas du kan följa förloppet för hello under **meddelanden** hello-menyn.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Ta bort ett Kör som-konto eller ett klassiskt Kör som-konto
Det här avsnittet beskrivs hur toodelete och sedan skapa ett kör som eller klassiska kör som-konto. När du utför den här åtgärden sparas hello Automation-konto. När du har tagit bort ett kör som eller klassiska kör som-konto kan skapa du den igen hello Azure-portalen.

1. Öppna hello Automation-konto i hello Azure-portalen.

2. På hello **automatiseringskontot** bladet i hello konto egenskapsrutan, Välj **kör som-konton**.

3. På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller klassiska kör som-konto som du vill toodelete. Klicka sedan på hello **egenskaper** bladet för hello valt konto, klickar du på **ta bort**.

 ![Ta bort Kör som-konto](media/automation-manage-account/automation-account-delete-runas.png)

4. Medan hello konto raderas, du kan följa förloppet hello under **meddelanden** hello-menyn.

5. När hello-konto har tagits bort, du kan skapa den igen på hello **kör som-konton** egenskapsbladet genom att välja hello skapa alternativet **Azure kör som-konto**.

 ![Skapa nytt hello Automation kör som-konto](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Felaktig konfiguration
Vissa konfigurationsobjekt som är nödvändiga för hello kör som eller klassiska kör som-konto toofunction kanske korrekt har tagits bort eller skapats felaktigt under installationen. hello-objekt är:

* Certifikattillgång
* Anslutningstillgång
* Kör som-konto har tagits bort från hello deltagarrollen
* Huvudnamn för tjänsten eller program i Azure AD

I föregående hello och andra instanser av felaktig konfiguration hello Automation-konto identifierar hello ändras och visar statusen *ofullständigt* på hello **kör som-konton** egenskapsbladet för hello konto.

![Konfigurationsstatusen Ofullständig för Kör som-konto](media/automation-manage-account/automation-account-runas-incomplete-config.png)

När du väljer hello-kör som-konto hello konto **egenskaper** hello följande felmeddelande visas:

![Varningsmeddelande om ofullständig konfiguration av Kör som-konto](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

Du kan snabbt lösa problemen kör som-konto genom att ta bort och återskapa hello-konto.

## <a name="next-steps"></a>Nästa steg
* Mer information om tjänstens huvudnamn finns för[program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md).
* Mer information om rollbaserad åtkomstkontroll i Azure Automation finns för[rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).
* Mer information om certifikat och Azure-tjänster finns för[certifikat översikt för Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
