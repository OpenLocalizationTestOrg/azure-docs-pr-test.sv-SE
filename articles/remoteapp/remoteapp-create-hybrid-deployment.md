---
title: "aaaHow toocreate en hybridsamling för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur toocreate en distribution av RemoteApp som ansluter tooyour interna nätverket."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 08ea0ce3-3a2c-4ddf-9394-6d75c8030cb1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 3fba29acc676e0af48e995da406f889c532c44c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-hybrid-collection-for-azure-remoteapp"></a>Hur toocreate en hybridsamling för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Det finns två typer av Azure RemoteApp-samlingar:

* Molnet: finns helt i Azure. Du kan välja toosave alla data i molnet hello (så en endast molnbaserad samling) eller tooconnect din samling tooa VNET och spara det data.   
* Hybrid: innehåller ett virtuellt nätverk för lokal åtkomst - kräver detta hello användning av Azure AD och lokala Active Directory-miljö.

Inte vet vilken behöver du? Checka ut [vilken typ av samling behöver för Azure RemoteApp](remoteapp-collections.md).

Den här självstudiekursen vägleder dig genom hello processen att skapa en hybridsamling. Det finns 8 steg:

1. Bestäm vad [bild](remoteapp-imageoptions.md) toouse för samlingen. Du kan skapa en anpassad avbildning eller använda en av hello Microsoft avbildningarna som ingår i din prenumeration.
2. Ställ in det virtuella nätverket. Kolla in hello [VNET planera](remoteapp-planvnet.md) och [sizing](remoteapp-vnetsizing.md) information.
3. Skapa en samling.
4. Anslut din samling tooyour lokala domän.
5. Lägg till en mall avbildningen tooyour samling.
6. Konfigurera katalogsynkronisering. Azure RemoteApp kräver att du har integrerat med Azure Active Directory genom att antingen 1) Konfigurera Azure Active Directory-synkronisering med hello alternativet för Lösenordssynkronisering eller 2) Konfigurera Azure Active Directory Sync utan hello Lösenordssynkronisering alternativet, men med en domän som är Federerad tooAD FS. Kolla in hello [konfigurationsinformation för Active Directory med RemoteApp](remoteapp-ad.md).
7. Publicera RemoteApp-appar.
8. Konfigurera användaråtkomst.

**Innan du börjar**

Du behöver toodo hello följande innan du skapar hello samlingen:

* [Registrera dig](https://azure.microsoft.com/services/remoteapp/) för Azure RemoteApp.
* Skapa ett användarkonto i Active Directory toouse som hello Azure RemoteApp-tjänstkontot. Begränsa hello behörigheter för det här kontot så att den kan bara ansluta till datorer toohello domän.
* Samla in information om ditt lokala nätverk: IP-adressen information och information om VPN-enhet.
* Installera hello [Azure PowerShell](/powershell/azure/overview) modul.
* Samla in information om hello-användare som du vill toogrant åtkomst till. Du behöver hello Azure Active Directory-användarens huvudnamn (till exempel name@contoso.com) för varje användare. Kontrollera att hello UPN matchar mellan Azure AD och Active Directory.
* Välj mallbild. En Azure RemoteApp-mallavbildningen innehåller hello appar och program som du vill toopublish för dina användare. Se [Azure RemoteApp-avbildningsalternativen](remoteapp-imageoptions.md) för mer information.
* Vill du toouse hello Office 365 ProPlus-avbildningen? Kolla in informationen [här](remoteapp-officesubscription.md).
* [Konfigurera Active Directory för RemoteApp](remoteapp-ad.md).

## <a name="step-1-set-up-your-virtual-network"></a>Steg 1: Ställ in det virtuella nätverket
Du kan distribuera en hybridsamling som använder ett befintligt Azure virtuellt nätverk eller skapa ett nytt virtuellt nätverk. Ett virtuellt nätverk kan dina användare komma åt data i det lokala nätverket via RemoteApp fjärresurser. Med hjälp av Azure-nätverk ger din samling direkt network access tooother Azure-tjänster och virtuella datorer distribueras toothat virtuellt nätverk.

Kontrollera att du granskar hello [VNET planera](remoteapp-planvnet.md) och [VNET storlek](remoteapp-vnetsizing.md) information innan du skapar ditt VNET.

### <a name="create-an-azure-vnet-and-join-it-tooyour-active-directory-deployment"></a>Skapa ett Azure VNET och ansluta den tooyour Active Directory-distribution
Börja med att skapa en [virtuellt nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Detta görs på hello **nätverk** fliken i hello Azure-portalen. Du måste tooconnect ditt virtuella nätverk toohello Active Directory-distribution som är synkroniserade tooyour Azure Active Directory-klient.

Se [skapa ett virtuellt nätverk med hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) för mer information.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Kontrollera att det virtuella nätverket är redo för Azure RemoteApp
Innan du skapar din samling, kontrollera att ditt nya virtuella nätverk är klar. Du kan kontrollera detta genom att göra hello följande:

1. Skapa en virtuell Azure-dator i hello undernät av hello virtuella nätverk som du just har skapat för RemoteApp.
2. Använda Fjärrskrivbord tooconnect toohello virtuell dator. (Klicka på **ansluta**.)
3. Ansluta den toohello samma Active Directory-distribution som du vill toouse för RemoteApp.

Fungerade som? Ditt virtuella nätverk och undernät som är klara för Azure RemoteApp!

Du hittar mer information om att skapa virtuella datorer i Azure och koppla toothem med fjärrskrivbord [här](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Steg 2: Skapa en Azure RemoteApp-samling
1. I hello [Azure-portalen](http://manage.windowsazure.com)går toohello Azure RemoteApp-sidan.
2. Klicka på **nya > Skapa med VNET**.
3. Ange ett namn för samlingen.
4. Välj hello plan som du vill toouse - standard eller grundläggande.
5. Välj ditt VNET i hello nedrullningsbara listan och sedan på undernätet.
6. Välj toojoin den tooyour domän.
7. Klicka på **skapa RemoteApp-samlingen**.

När Azure RemoteApp-samlingen har skapats, dubbelklickar du på hello namnet på hello-samling. Som kommer att få in hello **Snabbstart** sida - detta är där du har konfigurerat hello samling.

Något går fel? Kolla in hello [hybridsamling felsökningsinformation](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-toohello-local-domain"></a>Steg 3: Länka din samling toohello lokala domän
1. På hello **Snabbstart** klickar du på **ansluta till en lokal domän**.
2. Lägg till hello Azure RemoteApp tooyour lokala Active Directory domän för tjänstkonto. Du behöver hello domännamn, organisationsenhet, användarnamn för tjänstkonto och lösenord.
   
    Det här är hello information du samlade in om du har följt stegen hello i [konfigurera Active Directory för Azure RemoteApp](remoteapp-ad.md).

## <a name="step-4-link-tooan-azure-remoteapp-image"></a>Steg 4: Länken tooan Azure RemoteApp-avbildning
En Azure RemoteApp-mallavbildningen innehåller hello-program som du vill tooshare med användare. Du kan antingen skapa en ny [mallavbildningen](remoteapp-imageoptions.md) eller länk tooan befintlig avbildning (en redan importerats eller överföra tooAzure RemoteApp). Du kan också länka tooone av hello Azure RemoteApp [mallavbildningarna](remoteapp-images.md) som innehåller Office 365 eller Office 2013 (för Utvärderingsanvändning) program.

Om du överför hello ny avbildning måste tooenter hello namn och välj hello plats för hello avbildningen. På hello nästa sida i guiden hello, visas en uppsättning PowerShell-cmdlet - kopia samt köra dessa cmdlet: ar från en upphöjd Windows PowerShell-prompt tooupload hello angivna avbildning.

Om du länkar tooan befintliga mallavbildningen kan bara ange hello avbildningens namn, plats och tillhörande Azure-prenumeration.

## <a name="step-5-configure-active-directory-directory-synchronization"></a>Steg 5: Konfigurera Active Directory katalogsynkronisering
Azure RemoteApp kräver att du har integrerat med Azure Active Directory genom att antingen 1) Konfigurera Azure Active Directory-synkronisering med hello alternativet för Lösenordssynkronisering eller 2) Konfigurera Azure Active Directory Sync utan hello Lösenordssynkronisering alternativet, men med en domän som är Federerad tooAD FS.

Checka ut [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – den här artikeln kan du konfigurera katalogintegrering i steg 4.

Se [översikt över katalogsynkronisering](http://msdn.microsoft.com//library/azure/hh967642.aspx) för planering av information och detaljerade anvisningar.

## <a name="step-6-publish-apps"></a>Steg 6: Publicera appar
En app i Azure RemoteApp är hello app eller ett program som du anger tooyour användare. Det finns i hello-mallavbildningen som du har överfört för hello samling. När en användare ansluter till en app, visas toorun i sina lokala miljö, men den verkligen körs i Azure.

Innan användarna kan komma åt appar, måste toopublish dem – kan dina användare åtkomst hello appar via hello fjärrskrivbordsklienten.

Du kan publicera flera appar tooyour samling. Hello publishing sidan klickar du på **publicera** tooadd en app. Du kan antingen publicera från hello **starta** menyn på hello mallavbildningen eller genom att ange hello sökväg på hello mallavbildningen för hello app. Om du väljer tooadd från hello **starta** -menyn, Välj hello programmet tooadd. Om du väljer tooprovide hello sökvägen toohello app måste ange ett namn för hello appen och hello sökvägen toowhere som den är installerad på hello mallavbildningen.

## <a name="step-7-configure-user-access"></a>Steg 7: Konfigurera användaråtkomst
Nu när du har skapat din samling måste tooadd hello användare som du vill toobe kan toouse dina fjärresurser. hello användare att ange åtkomst tooneed tooexist i hello Active Directory-klient som är associerade med prenumerationen hello du använt toocreate Azure RemoteApp-samlingen.

1. Hello Snabbstart sidan klickar du på **konfigurera användaråtkomst**.
2. Ange hello arbetskonto (från Active Directory) eller Microsoft-konto som du vill toogrant åtkomst för.
   
   **Kommentarer:**
   
   Kontrollera att du använder hello  *user@domain.com*  format.
   
   Om du använder Office 365 ProPlus i samlingen, använder du hello Active Directory identiteter för dina användare. På så sätt kan du verifiera licensiering.
3. När användarna hello validerats, klickar du på **spara**.

## <a name="next-steps"></a>Nästa steg
Det - du har skapat och distribuerat hybrid Azure RemoteApp-samlingen. hello nästa steg är toohave användarna ladda ned och installera hello fjärrskrivbordsklienten. Hello-URL för hello klient hittar du på sidan för hello Azure RemoteApp-Snabbstart. Kan användare logga in på klienten hello och komma åt hello-appar som du har publicerat.

### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att i tillägg toorating den här artikeln och skriva kommentarer nedan, kan du göra ändringar toohello själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Rulla uppåt och klicka på **Redigera på GitHub** toomake ändringar - de kommer toous för granskning och sedan, när vi har godkänt dem, visas dina ändringar och förbättringar här.

