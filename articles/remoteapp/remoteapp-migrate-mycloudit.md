---
title: "aaaChange hello fakturering för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur toostop att debiteras för Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 8f94da9a-7848-4ddc-b7b7-d9c280ccf4d7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mbaldwin
ms.openlocfilehash: fe3841a88978ec56829932621489e75d5dd7e673
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-azure-remoteapp-toomycloudit"></a>Migrera från Azure RemoteApp tooMyCloudIT 

**För närvarande använder du Microsoft Azure RemoteApp?** : MyCloudIT skapat ett automatiskt verktyg toomigrate din Azure RemoteApp (ARA) samling(ar) toohello MyCloudIT plattform medan toorun på Microsoft Azure.

**Dra nytta av hello Azure Resource Manager portal**: slutförd migrering i hello MyCloudIT-plattformen gör det möjligt för direktåtkomst tooAzure nya Azure Resource Manager-portalen. Den här portalen innehåller alla hello nya funktioner och innovationer som erbjuds av Microsoft Azure, inklusive åtkomst tooVirtual datorn storlekar tooensure distributionen bygger toosupport hello behov.

**Testa i parallella tooensure hello rätta lösningen för dina behov**: hello MyCloudIT Migreringsverktyget bygger tooinitiate hello migreringsprocessen och testa parallella tag aktuella ARA användarna fortsätta toouse ARA.  Användarna kommer att finnas kvar i ARA förrän migreringen och testningen har slutförts.  hello Migreringsverktyget bygger toohandle hello vanliga ARA samling.  Om du anser att du har ett unikt, inte är standard scenario, kontaktar du oss på [ sales@conexlink.com ](mailto:sales@conexlink.com) så att vi kan ge en skräddarsydd plan tooassist migreringen.

**Funktioner för Desktop as a Service**: Observera att MyCloudIT erbjuder inte bara hello RemoteApp-funktioner som du är van vid, men vi erbjuder även fullständig Desktop-As-A-Service funktioner för hello samma kostnad per månad utan någon minsta användare krav.

## <a name="what-we-will-do-for-you"></a>Vad gör vi för dig

MyCloudIT automatiserar hello migreringen av din Azure RemoteApp-mall från hello Azure klassiska portal för din prenumeration toohello Azure Resource Manager-portalen för din prenumeration med vår automatiserade Migreringsverktyget.  

> [!NOTE]
> hello Azure RemoteApp mall måste finnas i hello samma Azure-Region som din ursprungliga distributionen av Azure RemoteApp.  Om du behöver toochange Azure-regioner eller Azure-prenumerationer under hello migreringen, kontakta oss för ytterligare hjälp vid [ sales@conexlink.com ](mailto:sales@conexlink.com).

Läs nedan för detaljerad information om hello automated migreringsprocessen med hello MyCloudIT Migreringsverktyg för användartillstånd:

1. hello Migreringsverktyget söker igenom dina aktuella abonnemang för alla befintliga ARA-distributioner.  
2. Välj en ARA samling toomigrate i taget.  Om du har flera samlingar kan köra flera gånger i vårt verktyg.
3. Du har hello alternativet toocopy hello diskar av profilen (UPD) tooyour ny distribution så att du kan hämta äldre data, eller mappa UPD: ar toohello nya distributionen manuellt. Om du väljer toocopy din UPD: ar kan vi spara hello UPD: ar och inkludera en textfil som mappar hello UPD namn tooeach användarnas namn.  hello UPD: ar blir kopierade tooa resurs på hello RDSMGMT servern `F:\Shares\LegacyUPD` och ska visas via hello resurs `\\RDSmgmt\LegacyUPD`. 
4. Migreringen kräver utan avbrott för din aktuella ARA-distribution.  Men om några ändringar görs toohello UPD: ar (från ARA) efter hello kopiering kan ändringarna återspeglas inte i hello UPD: ar lagras i hello Azure Resource Manager-portalen. 
5. Om du har ytterligare virtuella datorer som domänkontrollanter och filservrar i din klassiska Azure-nätverk vi upprättar VNet-peering mellan ditt befintliga klassiska Azure-nätverk och hello nytt virtuellt nätverk vi skapar du, i hello ny Azure-resurs Manager-portalen.
6. Vår automatisk lösning endast kan upprätta VNet-peering mellan ditt befintliga klassiska Azure-nätverk och hello nytt virtuellt nätverk om din befintliga ARA-distribution är en Hybriddistribution; d.v.s. autentiseras du med en Windows Server Active Directory-domänkontrollant i hello befintliga klassiska virtuella nätverk. Om vi inte upprätta VNet-peering för samlingen, men du behöver VNet-peering, kontaktar du oss som [ sales@conexlink.com ](mailto:sales@conexlink.com) och vi är glada tooconfigure VNet-peering utan extra kostnad.
7. Vår automatisk lösning säkerställer Azure DNS-konfiguration har uppdaterats med hello nytt virtuellt nätverk inställningar tooensure distributionen av nya ansluta tooyour befintlig domänkontrollant i hello klassiska virtuella nätverk.
8. Vår automatisk lösning säkerställer att det inte finns några IP-adresskonflikter när vi skapar den här nytt virtuellt nätverk och upprätta hello VNet-peering för distributioner som har en befintlig Windows Server Active Directory-Server.
9. Om du bara använder Azure AD för autentisering, MyCloudIT skapar en ny Windows Server Active Directory-domän och använda Azure AD Connect toosynchronize användare mellan hello befintliga Azure AD-instans och hello Skapa ny Windows Server Active Directory-domän av MyCloudIT.
10. Om du använder en Windows Server Active Directory-domän tooauthenticate ARA användare ansluter vår automatisk lösning för din nya MyCloudIT distribution tooyour befintlig Windows Server Active Directory-domänkontrollant via VNet-peering.
11. Om du använder Azure Active Directory Domain Services för autentisering, kan vi migrerar du men kontaktar du oss så att vi kan skapa en anpassad migreringsplan för dig.  Skicka ett e-postmeddelande för[sales@conexlink.com](mailto:sales@conexlink.com). 
12. När hello samling toobe migreras bekräftat, sitta tillbaka och slappna av medan våra automatisk lösning migrerar din ARA samlingen och användarprofil-diskar (valfritt) toohello nya MyCloudIT drivs fjärrprogram lösningen.
13. När hello distributionen är klar, vi ska publicera hello samma program som publicerats i ARA och efter distributionen som du kommer att kunna toopublish fler program.

## <a name="post-migration-benefits"></a>Efter migreringen förmåner

1. Vi ger hello-hanteringskonsolen kan toomanage hello hela livscykeln för fjärrprogram distributionen.
2. Du kommer att kunna toomanage dina virtuella datorer från vår portal.  Starta / Stoppa och ändra storlek på enskilda virtuella datorer om det behövs.
3. hello-hanteringskonsolen tillhandahåller hello möjlighet toocreate och hantera användare / grupper från våra management-portalen.
4. hello-hanteringskonsolen innehåller hello möjlighet toosynchronize användare med Office 365 toocreate en samma inloggning.
5. hello-hanteringskonsolen tillhandahåller hello möjlighet toocreate ytterligare fjärrprogram och fjärrskrivbord samlingar utan kostnader för dubblettanvändare eller minsta användarkrav. 
6. hello-hanteringskonsolen innehåller hello möjlighet toopublish nya fjärrprogram program.
7. hello-hanteringskonsolen innehåller hello möjlighet tooschedule hello start och stopp av distributionen av fjärråtkomst appar om du bara behöver din lösning under tider.
8. hello-hanteringskonsolen innehåller hello möjlighet tooautomate hello installationen och konfigurationen av hello Azure Backup-agenten som innehåller en historik för kvarhållning av dokument för kundens data.
9. hello-hanteringskonsolen innehåller åtkomst tooperformance mätvärden för din distribution.  Det här ger du hello möjlighet tooidentify alla potentiella flaskhalsar utan att installera ytterligare prestandaverktyg.
10. Om du har flera session värdar blir kan tooenable automatisk skalning så att endast hello session värdar som behöver toobe kör körs.
11. MyCloudIT ger åtkomst toohello RDWeb gateway-servern via ett MyCloudIT domännamn.  Vi också ange hello möjlighet toore kartan hello URL tooa anpassad URL för slutanvändaren anpassning.

## <a name="prerequisites-for-migration"></a>Krav för migrering

1. Du måste ha åtkomst toohello Azure-prenumeration som är värd för din aktuella Azure RemoteApp-lösning.
2. Du måste ge vår portal behörigheter i din prenumeration toomigrate din mall och toocreate / ändra distributionen av nya MyCloudIT.
3. Observera att på grund av tooa begränsning i Azure RemoteApp, varje samling kan endast migreras tre gånger.  Om du behöver toomigrate en samling mer än tre gånger, kan du antingen öka en biljett tooAzure tooincrease export-antal eller kontakta oss och vi hjälper hello ARA begäran tooincrease hello export count.

## <a name="mycloudit-billing"></a>MyCloudIT fakturering

Se [MyCloudIT prissättningen för RemoteApp-lösningar](https://mcitdocuments.blob.core.windows.net/terms/MyCloudIT_Pricing_Overview.pdf) (PDF) mer information om hur toopredict och hantera dina Azure övergripande kostnader.

Om du fortfarande har frågor kan du kontakta oss på [ sales@conexlink.com ](mailto:sales@conexlink.com) eller titta på hello fullständig demonstrationsvideon [Migreringsverktyget för Azure RemoteApp - MyCloudIT](https://www.youtube.com/watch?v=YQ_1F-JeeLM&t=482s). 

