---
title: problem med aaaTroubleshoot cloud service-distribution | Microsoft Docs
description: "Det finns några vanliga problem som kan uppstå när du distribuerar en cloud service tooAzure. Den här artikeln innehåller lösningar toosome av dem."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 15aea4f2b2913d95f3378b2e9762b232531f3c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Felsökning av problem med cloud service-distribution
När du distribuerar en cloud service programmet paketet tooAzure får du information om distributionen av hello från hello **egenskaper** rutan i hello Azure-portalen. Du kan använda hello information i det här fönstret toohelp felsökning av problem med hello-Molntjänsten och du kan ange den här informationen tooAzure stöd när du öppnar en ny supportförfrågan.

Du kan hitta hello **egenskaper** rutan på följande sätt:

* I hello Azure-portalen, klicka på hello distribution av Molntjänsten, **alla inställningar**, och klicka sedan på **egenskaper**.
* I hello klassiska Azure-portalen, klicka på hello distribution av Molntjänsten, **INSTRUMENTPANELEN**på hello nedre högra hörnet på sidan hello (under **snabböversikten**). Tänk på att det finns ingen ”egenskaper” etikett på det här fönstret.

> [!NOTE]
> Du kan kopiera hello innehållet i hello **egenskaper** fönstret toohello Urklipp genom att klicka på hello-ikonen i hello övre högra hörnet i hello-fönstret.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problem: Jag kan inte komma åt min webbplats, men installationen startas och alla rollinstanser är klara
hello webbplats URL-länk som visas i hello portal innehåller inte hello port. hello-standardporten för webbplatser är 80. Om ditt program är konfigurerade toorun i en annan port, måste du lägga till hello rätt port number toohello URL vid åtkomst till hello webbplats.

1. Klicka på hello distribution av Molntjänsten i hello Azure-portalen.
2. I hello **egenskaper** rutan hello Azure-portalen, kontrollera hello portar för hello rollinstanser (under **indata slutpunkter**).
3. Lägg till hello rätt port Värdet toohello URL när du har åtkomst till programmet hello om inte hello port 80 till. toospecify en annan port än standardporten, Skriv hello URL, följt av ett kolon (:) följt av hello portnummer, utan blanksteg.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problem: Min rollinstanser återvinns utan mig gör någonting
Tjänsten återställning sker automatiskt när Azure identifierar problemet noder och därför flyttar rollen instanser toonew noder. När detta inträffar kan du se dina rollinstanser återvinning automatiskt. toofind reda på om tjänsten återställning uppstod:

1. Klicka på hello distribution av Molntjänsten i hello Azure-portalen.
2. I hello **egenskaper** rutan hello Azure-portalen, granska hello information och kontrollera om tjänsten återställning uppstod under hello tid som observeras av hello roller återvinning.

Roller kommer också att återanvändas ungefär en gång i månaden under värd-OS och Gäst-OS-uppdateringar.  
Mer information finns i blogginlägget hello [roll-instansen startas om på grund av tooOS uppgraderingar](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problem: Det går inte att jag göra en VIP-växling och får ett felmeddelande
En VIP-växling är inte tillåten om en av distributionsuppdatering pågår. Distribution av uppdateringar kan ske automatiskt när:

* En ny gästoperativsystemet är tillgänglig och du har konfigurerats för automatiska uppdateringar.
* Tjänsten återställning inträffar.

toofind ut hindrar om en automatisk uppdatering dig från att utföra en VIP-växling:

1. Klicka på hello distribution av Molntjänsten i hello Azure-portalen.
2. I hello **egenskaper** rutan hello Azure-portalen, titta på hello värdet för **Status**. Om det är **klar**, kontrollera **senaste åtgärden** toosee om en nyligen uppstått som kan förhindra att hello VIP-växlingen.
3. Upprepa steg 1 och 2 för hello Produktionsdistribution.
4. Om en automatisk uppdatering pågår, vänta tills den toofinish innan du försöker toodo hello VIP-växlingen.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problem: En rollinstans slingor mellan igång, initierar, upptagen och Stoppad
Det här tillståndet kan tyda på ett problem med programkoden, paketet eller konfigurationsfilen. I så fall bör du kunna toosee hello status ändra några minuters mellanrum och hello Azure-portalen kan stå ungefär **återvinning**, **upptagen**, eller **initierar**. Detta anger att det är något fel med hello-program som hindrar hälsningspaket rollinstansen från att köras.

Mer information om hur tootroubleshoot för det här problemet finns i blogginlägget hello [Compute diagnostikdata i Azure PaaS](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) och [vanliga problem som orsakar roller toorecycle](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problem: Mitt program stoppats
1. Klicka på hello rollinstans i hello Azure-portalen.
2. I hello **egenskaper** rutan hello Azure-portalen, Överväg hello följande villkor tooresolve problemet:
   * Om hälsningspaket rollinstansen nyligen har stoppats (du kan kontrollera hello värdet för **antal avbrott**), hello distribution kunde uppdateras. Vänta toosee om hello rollinstans återställs fungerar på sin egen.
   * Om hälsningspaket rollinstansen **upptagen**, kontrollera din kod toosee för programmet hello [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) händelsen hanterats. Du kanske behöver tooadd eller åtgärda kod som hanterar den här händelsen.
   * Gå igenom hello diagnostikdata och felsökning i hello blogginlägget [Compute diagnostikdata i Azure PaaS](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Om du återanvänder Molntjänsten återställa du hello egenskaper för distribution av hello effektivt raderar hello information för hello ursprungliga problem.
>
>

## <a name="next-steps"></a>Nästa steg
Visa mer [felsökning artiklar](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) för molntjänster.

toolearn hur tootroubleshoot moln rolltjänst problem med hjälp av Azure PaaS diagnostikdata för datorn, se [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
