---
title: aaaHow toocreate en molnsamling av Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toocreate en distribution av Azure RemoteApp som sparar data i hello Azure-molnet."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 4d7c6956-7e4a-4a41-b7f2-7e5832bf01e3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a072ad19d8293016382831d48d0af8e0f5e0d458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-cloud-collection-of-azure-remoteapp"></a>Hur toocreate en molnsamling av Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Det finns två typer av [Azure RemoteApp](remoteapp-collections.md)-samlingar: moln- och hybridsamlingar. 

* Molnet: finns helt i Azure. Du kan välja toosave alla data i molnet hello (så en endast molnbaserad samling) eller tooconnect din samling tooa VNET och spara det data.   
* Hybrid: innehåller ett virtuellt nätverk för lokal åtkomst - kräver detta hello användning av Azure AD och lokala Active Directory-miljö.

Den här självstudiekursen vägleder dig genom hello processen att skapa en molnsamling. Det finns fyra steg: 

1. Skapa en Azure RemoteApp-samling.
2. Du kan också konfigurera katalogsynkronisering. Om du använder Azure AD + Active Directory, du har toosynchronize användare, kontakter och lösenord från din lokala Active Directory tooyour Azure AD-klient.
3. Publicera appar.
4. Konfigurera användaråtkomst.

**Innan du börjar**

Du behöver toodo hello följande innan du skapar hello samlingen:

* [Registrera dig](https://azure.microsoft.com/services/remoteapp/) för Azure RemoteApp. 
* Samla in information om hello-användare som du vill toogrant åtkomst till. Detta kan vara antingen Microsoft-kontoinformation eller Active Directory fungerar kontoinformationen för användare.
* Den här proceduren förutsätter att du kan antingen pågående toouse något av hello mallavbildningarna som ingår i prenumerationen eller att du har redan överförts hello-mallavbildningen som du vill toouse. Om du behöver tooupload en annan mall-avbildning, kan du göra det från hello Mallavbildningarna sida. Klicka bara på **överför en mallavbildningen** och följer anvisningarna hello i hello guiden. 
* Vill du toouse hello Office 365 ProPlus-avbildningen? Kolla in informationen [här](remoteapp-officesubscription.md).
* Vill tooprovide anpassade appar eller LOB-program. Skapa en ny [bilden](remoteapp-imageoptions.md) och använda i din molnsamling.
* Ta reda på om du behöver tooconnect tooa VNET. Om du väljer tooconnect tooa VNET, kontrollera att den uppfyller hello [storleksanpassa riktlinjer](remoteapp-vnetsizing.md) och att det [kan ansluta tooRemoteApp](remoteapp-vnet.md). Kolla in hello [VNET planering artikel ](remoteapp-planvnet.md)för mer information.
* Om du använder ett virtuellt nätverk kan du bestämma om toojoin den tooyour lokala Active Directory-domän.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Steg 1: Skapa en molnsamling - med eller utan ett virtuellt nätverk
Använd hello följande steg för**skapar en molnbaserad samling**:

1. Gå toohello RemoteApp-sidan i hello-hanteringsportalen.
2. Klicka på **nya > Snabbregistrering**.
3. Ange ett namn för samlingen och välj din region.
4. Välj hello plan som du vill toouse - standard eller grundläggande.
5. Välj hello mallen toouse för den här samlingen. 
   
    **Tips:** prenumerationen för RemoteApp medföljer [mallavbildningarna](remoteapp-images.md) som innehåller Office 365 eller Office 2013 (för Utvärderingsanvändning) program, en del publicerade (till exempel Word) och andra klar toopublish. Du kan också skapa en ny [bilden](remoteapp-imageoptions.md) och använda i din molnsamling.
6. Klicka på **skapa RemoteApp-samlingen**.
   
    **Viktigt:** kan det ta upp too30 minuter tooprovision din samling.

När Azure RemoteApp-samlingen har skapats, dubbelklickar du på hello namnet på hello-samling. Som kommer att få in hello **Snabbstart** sida - detta är där du har konfigurerat hello samling.

Använd hello följande steg toocreate en **moln + VNET samling**:

1. Gå toohello Azure RemoteApp-sidan i hello-hanteringsportalen.
2. Klicka på **nya** > **skapa med VNET**.
3. Ange ett namn för samlingen.
4. Välj hello plan som du vill toouse - standard eller grundläggande.
5. Välj hello VNET som du redan skapat. Vet inte hur toodo som? För tillfället hello stegen är i hello [hybrid](remoteapp-create-hybrid-deployment.md) avsnittet.
6. Bestäm om du vill toojoin din samling tooyour domän. Om Ja, behöver du toouse AD Connect toointegrate Azure AD och Active Directory-miljön. Som beskrivs i nedan i **steg 2**.
7. Klicka på **skapa RemoteApp-samlingen**.

## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Steg 2: Konfigurera Active Directory-katalogsynkronisering (valfritt)
Om du vill toouse Active Directory, kräver Azure RemoteApp katalogsynkronisering mellan Azure Active Directory och lokala Active Directory toosynchronize användare, kontakter och lösenord tooyour Azure Active Directory-klient. Se [konfigurera Active Directory för Azure RemoteApp](remoteapp-ad.md) planeringsinformation. Du kan också gå direkt för[AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) information.

## <a name="step-3-publish-apps"></a>Steg 3: Publicera appar
En app i Azure RemoteApp är hello app eller ett program som du anger tooyour användare. Det finns i hello-mallavbildningen som du har överfört för hello samling. När en användare öppnar en app, hello app visas toorun i sina lokala miljö, men den verkligen körs på en virtuell dator i Azure. 

Innan användarna kan komma åt appar, måste toopublish dem – publicering av appar kan användarna komma åt hello appar via hello fjärrskrivbordsklienten.

Du kan publicera flera appar tooyour Azure RemoteApp-samling. Hello publishing sidan klickar du på **publicera** tooadd ett program. Du kan antingen publicera från hello **starta** menyn på hello mallavbildningen eller genom att ange hello sökväg på hello mallavbildningen för hello app. Om du väljer tooadd från hello **starta** -menyn, Välj hello app toopublish. Om du väljer tooprovide hello sökvägen toohello app måste ange ett namn för hello appen och hello sökvägen toowhere som den är installerad på hello mallavbildningen.

## <a name="step-4-configure-user-access"></a>Steg 4: Konfigurera användaråtkomst
Nu när du har skapat din samling måste tooadd hello användare som du vill toobe kan toouse dina fjärresurser. Om du använder Active Directory hello användare att ange åtkomst tooneed tooexist i hello Active Directory-klient som är associerade med prenumerationen hello du har använt toocreate samlingen.

1. Hello Snabbstart sidan klickar du på **konfigurera användaråtkomst**. 
2. Ange hello arbetskonto (från Active Directory) eller Microsoft-konto som du vill toogrant åtkomst för.
   
   **Kommentarer:** 
   
   Kontrollera att du använder hello  *user@domain.com*  format.
   
   Om du använder Office 365 ProPlus i samlingen, använder du hello Active Directory identiteter för dina användare. På så sätt kan du verifiera licensiering. 
3. När användarna hello validerats, klickar du på **spara**.

## <a name="next-steps"></a>Nästa steg
Det - du har skapat och distribuerat din Azure RemoteApp-molnsamling. hello nästa steg är toohave användarna ladda ned och installera hello fjärrskrivbordsklienten. Hello-URL för hello klient hittar du på sidan för hello Azure RemoteApp-Snabbstart. Kan användare logga in på klienten hello och komma åt hello-appar som du har publicerat.

### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att i tillägg toorating den här artikeln och skriva kommentarer nedan, kan du göra ändringar toohello själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Rulla uppåt och klicka på **Redigera på GitHub** toomake ändringar - de kommer toous för granskning och sedan, när vi har godkänt dem, visas dina ändringar och förbättringar här.

