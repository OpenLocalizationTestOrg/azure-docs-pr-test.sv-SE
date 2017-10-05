---
title: "Ändra faktureringen för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om att stoppa faktureras för Azure RemoteApp."
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
ms.openlocfilehash: 32fc673eeef01e80c73375bf264206beea8cfbe5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="migrate-from-azure-remoteapp-to-mycloudit"></a>Migrera från Azure RemoteApp till MyCloudIT 

**För närvarande använder du Microsoft Azure RemoteApp?** : MyCloudIT skapat ett automatiskt verktyg för att migrera din Azure RemoteApp ARA ()-samling(ar) till hanteringsplattform MyCloudIT när fortsätter att köras på Microsoft Azure.

**Dra nytta av Azure Resource Manager-portal**: slutförd migrering i MyCloudIT-plattformen gör det möjligt för omedelbar åtkomst till Azures nya Azure Resource Manager-portalen. Den här portalen innehåller nya funktioner och innovationer som erbjuds av Microsoft Azure, inklusive tillgång till storlekar för virtuella datorer för distributionen är utformat för att stöd för ditt företag.

**Testa parallellt så den rätta lösningen för dina behov**: den MyCloudIT Migreringsverktyget är utformat för att påbörja migreringsprocessen och testa parallella tag aktuella ARA användarna fortsätta att använda ARA.  Användarna kommer att finnas kvar i ARA förrän migreringen och testningen har slutförts.  Migreringsverktyget är utformat för att hantera vanliga ARA-samlingen.  Om du anser att du har ett unikt, inte är standard scenario, kontaktar du oss på [ sales@conexlink.com ](mailto:sales@conexlink.com) så att vi kan ge en skräddarsydd plan för att hjälpa till med migreringen.

**Funktioner för Desktop as a Service**: Observera att MyCloudIT erbjuder inte bara RemoteApp-funktioner som du är van vid, men vi erbjuder även fullständig Desktop-As-A-Service funktioner för samma kostnad per månad utan någon minsta användare krav.

## <a name="what-we-will-do-for-you"></a>Vad gör vi för dig

MyCloudIT automatiserar migreringen av din Azure RemoteApp mall från den klassiska Azure-portalen för din prenumeration på Azure Resource Manager-portalen för din prenumeration med vår automatiserade Migreringsverktyget.  

> [!NOTE]
> Mallen för Azure RemoteApp måste vara i samma Azure-Region som din ursprungliga distributionen av Azure RemoteApp.  Om du behöver ändra Azure-regioner eller Azure-prenumerationer under migreringen kan kontakta oss för ytterligare hjälp vid [ sales@conexlink.com ](mailto:sales@conexlink.com).

Läs nedan för detaljerad information om automatisk migreringsprocessen med Migreringsverktyget MyCloudIT:

1. Migreringsverktyget söker igenom dina aktuella abonnemang för alla befintliga ARA-distributioner.  
2. Välj en ARA samling för att migrera i taget.  Om du har flera samlingar kan köra flera gånger i vårt verktyg.
3. Du har möjlighet att kopiera användarens profil diskar (UPD) till din nya distribution så att du kan hämta äldre data, eller manuellt mappa dina UPD: ar till den nya distributionen. Om du väljer att kopiera ditt UPD: ar, kommer att spara de UPD: ar och inkludera en textfil som mappar UPD namn till varje användares namn.  De UPD: ar som ska kopieras till en resurs på servern RDSMGMT `F:\Shares\LegacyUPD` och ska visas via resursen `\\RDSmgmt\LegacyUPD`. 
4. Migreringen kräver utan avbrott för din aktuella ARA-distribution.  Men om några ändringar görs i UPD: ar (från ARA) efter kopieringen, dessa ändringar visas inte direkt i UPD: ar som lagras i Azure Resource Manager-portalen. 
5. Om du har ytterligare virtuella datorer som domänkontrollanter och filservrar i din klassiska Azure-nätverk upprättar vi VNet-peering mellan ditt befintliga klassiska Azure-nätverk och ett nytt virtuellt nätverk som vi skapa för dig, i den nya Azure-resursen Manager-portalen.
6. Vår automatisk lösning endast upprättar VNet-peering mellan ditt befintliga klassiska Azure-nätverk och ett nytt virtuellt nätverk om din befintliga ARA-distribution är en Hybriddistribution; d.v.s. autentiseras du med en Windows Server Active Directory-domänkontrollant i befintliga klassiska virtuella nätverk. Om vi inte upprätta VNet-peering för samlingen, men du behöver VNet-peering, kontaktar du oss som [ sales@conexlink.com ](mailto:sales@conexlink.com) och vi är glada över att konfigurera VNet-peering utan extra kostnad.
7. Vår automatisk lösning säkerställer Azure DNS-konfiguration har uppdaterats med de nya inställningarna för virtuellt nätverk att kontrollera distributionen av nya kan ansluta till den befintliga domänkontrollanten i klassiska virtuella nätverk.
8. Vår automatisk lösning säkerställer att det inte finns några IP-adresskonflikter när vi skapar den här nytt virtuellt nätverk och upprätta VNet-peering för distributioner som har en befintlig Windows Server Active Directory-Server.
9. Om du bara använder Azure AD för autentisering, MyCloudIT skapar en ny Windows Server Active Directory-domän och använda Azure AD Connect för att synkronisera användare mellan den befintliga instansen av Azure AD och den nya Windows Server Active Directory Domain skapas av MyCloudIT.
10. Om du använder en Windows Server Active Directory-domän för att autentisera användare ARA ansluter vår automatisk lösning distributionen MyCloudIT till din befintliga Windows Server Active Directory-domänkontrollant via VNet-peering.
11. Om du använder Azure Active Directory Domain Services för autentisering, kan vi migrerar du men kontaktar du oss så att vi kan skapa en anpassad migreringsplan för dig.  Skicka ett e-postmeddelande till [ sales@conexlink.com ](mailto:sales@conexlink.com). 
12. När en samling som ska migreras bekräftas sitta tillbaka och slappna av medan våra automatisk lösning migrerar din ARA samlingen och (valfritt) användarprofil-diskar till nya MyCloudIT drivs fjärrprogram lösning.
13. När installationen är klar, kommer vi Återpublicera samma program som publicerats i ARA och efter distributionen som du kommer att kunna publicera fler program.

## <a name="post-migration-benefits"></a>Efter migreringen förmåner

1. Vi ger hanteringskonsolen som hjälper dig att hantera hela livscykeln för fjärrprogram distributionen.
2. Du kommer att kunna hantera dina virtuella datorer från vår portal.  Starta / Stoppa och ändra storlek på enskilda virtuella datorer om det behövs.
3. Hanteringskonsolen ger möjlighet att skapa och hantera användare / grupper från våra management-portalen.
4. Hanteringskonsolen ger möjlighet att synkronisera användare med Office 365 för att skapa en samma inloggning.
5. Hanteringskonsolen ger möjlighet att skapa ytterligare fjärrprogram och fjärrskrivbord samlingar utan kostnader för dubblettanvändare eller minsta användarkrav. 
6. Hanteringskonsolen ger möjlighet att publicera den nya fjärrprogram program.
7. Hanteringskonsolen ger möjlighet att schemalägga start och stopp av distributionen av fjärråtkomst appar om du bara behöver din lösning under tider.
8. Hanteringskonsolen ger möjlighet att automatisera installationen och konfigurationen av Azure Backup-agenten som innehåller en historik för kvarhållning av dokument för kundens data.
9. Hanteringskonsolen ger åtkomst till prestandamått för distributionen.  Detta ger dig möjlighet att identifiera alla potentiella flaskhalsar utan att installera ytterligare prestandaverktyg.
10. Om du har flera session värdar kommer du att kunna aktivera automatisk skalning så att endast sessionen värdar som behöver köras körs.
11. MyCloudIT ger åtkomst till RDWeb gateway-servern via ett MyCloudIT domännamn.  Vi ger också möjlighet att mappa URL-Adressen till en anpassad URL för slutanvändaren anpassning.

## <a name="prerequisites-for-migration"></a>Krav för migrering

1. Du måste ha åtkomst till Azure-prenumerationen som är värd för din aktuella Azure RemoteApp-lösning.
2. Du måste ge vår portal behörigheter i din prenumeration för att migrera din mall och för att skapa / ändra distributionen av nya MyCloudIT.
3. Observera att på grund av en begränsning i Azure RemoteApp, varje samling kan endast migreras tre gånger.  Om du behöver migrera en samling med fler än tre gånger, kan du antingen öka en biljett till Azure för att öka din export antal eller kontakta oss och vi hjälper ARA begäran om att öka antalet export.

## <a name="mycloudit-billing"></a>MyCloudIT fakturering

Se [MyCloudIT prissättningen för RemoteApp-lösningar](https://mcitdocuments.blob.core.windows.net/terms/MyCloudIT_Pricing_Overview.pdf) (PDF) för information om hur du förutsäga och hantera dina Azure övergripande kostnader.

Om du fortfarande har frågor kan du kontakta oss på [ sales@conexlink.com ](mailto:sales@conexlink.com) eller titta på fullständig demonstrationsvideon [Migreringsverktyget för Azure RemoteApp - MyCloudIT](https://www.youtube.com/watch?v=YQ_1F-JeeLM&t=482s). 

