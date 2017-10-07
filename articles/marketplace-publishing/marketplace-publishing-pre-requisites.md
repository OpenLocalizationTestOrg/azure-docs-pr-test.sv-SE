---
title: "aaaNon tekniska krav för att skapa ett erbjudande om hello Azure Marketplace | Microsoft Docs"
description: "Förstå hello kraven för att skapa och distribuera ett erbjudande toohello Azure Marketplace för andra toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ms.openlocfilehash: 472096863084cc58dc921313419ab60b1a08a3bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="general-prerequisites-for-creating-an-offer-for-hello-azure-marketplace"></a>Allmänna krav för att skapa ett erbjudande om hello Azure Marketplace
Förstå hello Allmänt, business-process-elementbaserade krav som är nödvändiga toomove med hello erbjuder skapandeprocessen.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Se till att du är registrerad som en säljare med Microsoft
Detaljerade instruktioner om hur du registrerar ett säljare-konto med Microsoft gå för[skapande av konton och registrering](marketplace-publishing-accounts-creation-registration.md).

* **Om ditt företag har redan registrerats som en säljare i hello Dev Center och du vill toocreate ett nytt erbjudande** sedan inloggningen toohello publicering portal med hello samma e-id med vilka Dev Center har registrerats. Det här steget krävs så att hello Dev Center och publicering portal är kopplade till varandra.
* **Om ditt företag har redan registrerats som en säljare i hello Dev Center och du vill tooedit ett befintligt erbjudande** sedan antingen inloggningen toohello publicering portal hello administratörskonto eller med ett konto som har lagts till som medadministratör i hello publicering av portalen . Steg tooadd konto för en medadministratör anges nedan.

## <a name="steps-tooadd-a-co-admin-in-hello-publishing-portal"></a>Tooadd steg för en medadministratör i hello Publishing Portal
Administratörer av hello publicering portal kan lägga till andra medlemmar i hello företag som arbetar på hello program som medadministratör i hello hello publicering av portalen. **Förutsatt att du är Hej administratör** anges nedan är hello steg tooadd co-administratör.

> [!NOTE]
> För nya användare innan du lägger till en medadministratör hello publicering portal, se till att du har skapat minst ett program i hello publicering av portalen. Detta krävs som hello **UTGIVARE** fliken visas endast när du har skapat minst ett program i hello publicering av portalen.
> 
> 

1. Se till att hello medadministratör e-post-id är ett Microsoft-account(MSA). Om inte, registrera den som en MSA med detta [länk](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Kontrollera att det finns minst ett program under hello administratörskonto innan du försöker tooadd co-administratör.
3. Hello senare steg när du är klar, logga in toohello publicera portal med hello medadministratör e-id och sedan logga ut.
4. Nu inloggningen toohello publicering portal med ID: t för hello admin-e-post.
5. Navigera tooPublishers -> ditt konto -> Välj administratörer -> Lägg till hello medadministratör (skärmbilden nedan)
   
    ![Rita](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Kontrollera som e-ID: n anges vid hello olika stegen i hello publiceringsprocessen (t.ex. Dev Center, publicera portal) som ska övervakas för all kommunikation från Microsoft.
7. För registrering av Dev Center Undvik att använda ett konto som är associerade med en enskild person. Detta rekommenderas för beroendet från en person.
8. Om du står inför eventuella problem med registreringen Dev Center sedan Utlös en biljett med den här [länk](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-toodelete-a-co-admin-in-hello-publishing-portal"></a>Toodelete steg för en medadministratör i hello publicering av portalen
**Förutsatt att du är Hej administratör** anges nedan är hello steg toodelete co-administratör.

1. Inloggningen toohello publicering portal med ID: t för hello admin-e-post.
2. Navigera för**utgivare** -> Välj ditt konto -> **administratörer** -> **Medadministratörer**.
3. Klicka på hello **X** knappen Nästa toohello medadministratör du vill ta bort totalvärde (skärmbilden nedan).
   
    ![Rita](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Du har inte toocomplete skatt och bank företagsinformation om du planerar toopublish endast kostnadsfria erbjudanden (eller använda din egen licens).
> 
> hello företagets registreringen måste vara slutförd tooget igång. Men när ditt företag fungerar på hello skatt och bank informationen i hello Microsoft Developer konto, hello utvecklare kan börja jobba skapar hello avbildning av virtuell dator i hello [Publiceringsportal](https://publish.windowsazure.com), tas den certifierade, och testning i hello Azure mellanlagringsmiljön. Hello fullständig säljare konto godkännande behöver du bara för hello sista steget i att publicera ditt erbjudande toohello Azure Marketplace.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Skaffa en Azure-prenumeration ”betalning per användning”
Detta är hello prenumeration som du vill använda toocreate VM-avbildningar och överlämna hello bilder toohello [Azure Marketplace](https://azure.microsoft.com/marketplace/). Om du inte har en befintlig prenumeration sedan logga på https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>”Säljer-från-länder
> [!WARNING]
> I ordning toosell dina tjänster på hello Azure Marketplace, du måste se till att registrerade entiteten är från en hello godkända ”säljer-från-länder. Den här begränsningen är beloppet och skatt skäl. Vi söker aktivt tooexpand denna lista över länder i hello nära framtiden, så Håll ögonen öppna. Hello fullständig lista finns i avsnittet 1b av hello [Azure Marketplace deltagande principer](http://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Nästa steg
När hello icke-tekniska krav är uppfyllda, nästa är hello erbjudande specifika tekniska krav. Klicka på hello länken toohello artikel för hello respektive erbjudande som du vill att toocreate för hello Azure Marketplace.

* [Tekniska krav för VM](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Lösning mallen tekniska krav](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Se även
* [Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)

