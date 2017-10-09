---
title: "aaaAzure RemoteApp vanliga frågor och svar | Microsoft Docs"
description: "Läs svaren toohello mest vanliga frågor och svar om Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: swadhwa
editor: 
ms.assetid: bad66603-91f9-437f-8a70-236405d2a27f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: da981fea9e38b4e74694aeaba5f97c8ed897ccd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-faq"></a>Vanliga frågor och svar om Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Vi har fått hello följande frågor om Azure RemoteApp. Har du andra frågor? Besök hello [RemoteApp-forumen](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) och berätta för oss vad du behöver tooknow eller listrutan kommentaren nedan.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Hittar du inte det du letar efter? Har du en fråga vi inte har svarat på?
Om du inte hittar hello information du behöver eller du har ytterligare frågor som vi inte täcker här, gå toohello [Azure RemoteApp-forumet](http://aka.ms/araforum) och din fråga. Vi kan alltid lägga till fler svar här.

## <a name="what-is-azure-remoteapp"></a>Vad är Azure RemoteApp?
* **Vad är Azure RemoteApp?** RemoteApp är en Azure-tjänst som hjälper dig att ge säker åtkomst tooapplications från många olika användarenheter. Läs mer om [Azure RemoteApp](remoteapp-whatis.md).
* **Vilka är distributionsalternativen för hello?** Det finns två typer av RemoteApp-samlingar: moln och hybrid. Vilken typ av samling du behöver beror på ett antal faktorer, till exempel om det krävs domänanslutning. Klicka [här](remoteapp-collections.md) för att se en diskussion om de olika alternativen.

## <a name="quick-tips-on-using-azure-remoteapp"></a>Snabba tips om att använda Azure RemoteApp
* **Hur lång tid tar det innan jag blir frånkopplad? Hur länge kan jag vara inaktiv innan jag hello Start?** 4 timmar. Om du eller en av användarna är inaktiva i 4 timmar loggas du automatiskt ut från Azure RemoteApp. Checka ut hello andra standardinställningarna i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).
* **Kan jag prova på den här tjänsten gratis?** Ja. Det finns en kostnadsfri utvärderingsversion som gäller i 30 dagar. När hello utvärderingsversionen upphör övergång du tooa betald konto (som du kan använda i produktionen) eller sluta använda hello-tjänsten. Påbörja din kostnadsfria utvärderingsperiod genom att gå för[portal.azure.com](http://portal.azure.com) -skapa en ny instans av RemoteApp. Med hello kostnadsfri utvärderingsversion, kan du skapa 2 instanser av RemoteApp med 10 användare per instans. Kom ihåg att den här utvärderingsversionen bara gäller i 30 dagar.
  
  ## <a name="azure-remoteapp-subscription-details"></a>Information om Azure RemoteApp-prenumerationen
* **Vad är hello tjänstbegränsningarna?** Du lär dig mer om hello standardinställningar och gränser för Azure RemoteApp i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md). Kontakta oss om du har fler frågor.
* **Hur många användare gör jag har toohave?** Du måste ha minst 20 användare. Tydlighetens skull att toobe Rensa super - hello minsta är 20. Du kommer att debiteras för 20 användare. 
* **Hur mycket kostar RemoteApp?** Mer info finns på sidan med [Azure RemoteApp-prisinformation](https://azure.microsoft.com/pricing/details/remoteapp/).
* **Kostar en typ av samling mer än en annan?** Ja, det kan den göra, beroende på dina samlingskrav. En hybridsamling kräver en anslutning från Azure RemoteApp tooyour lokalt nätverk. Om du använder ett befintligt VNET/ExpressRoute tillkommer ingen ytterligare avgift. Men om du använder ett nytt Azure VNET och antingen en gateway eller Expressroute, debiteras du för hello [VPN-gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) eller [Express Route](https://azure.microsoft.com/pricing/details/expressroute/). Den här kostnaden (beskrivs i hello länkar) kostar ovanpå den månatliga Azure RemoteApp.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Samlingar – vad stöds, vilka bör du använda och annan info
* **Stöds anpassade LOB-program (Line-of-Business)?** Ja. toouse ett anpassat program i Azure RemoteApp, skapa en [anpassad mallavbildning](remoteapp-create-custom-image.md), och sedan ladda upp den toohello RemoteApp-samlingen.
* **Fungerar mitt anpassade LOB-program i Azure RemoteApp?**  hello bästa sätt toofigure som är tootest ut den. Kolla in hello [RD Compatibility Center](http://www.rdcompatibility.com/compatibility/default.aspx).
* **Vilken distributionsmetod (moln eller hybrid) är bäst för vårt företag?** Hybridsamlingar ger hello mest omfattande funktionerna om du vill ha full integration med enkel inloggning (SSO) och säkra lokala nätverksanslutningar. Molnsamlingar ger ett flexibelt och enkelt sätt tooisolate distributionen genom att använda flera autentiseringsmetoder. Läs mer om hello [distributionsalternativ](remoteapp-whatis.md).
* **Vi har SQL-databaser eller andra databaser antingen på plats eller i Azure. Vilken distributionstyp ska vi använda?** Det beror på var SQL-databasen eller serverdelsdatabasen finns. Om hello databasen är i ett privat nätverk, kan du använda hello hybridsamlingen. Om hello databasen är utsatta toohello Internet och tillåter anslutningar tooconnect tooit, kan du använda hello molnsamling.
* **Hur är det med enhetsmappning, USB-portar och seriella portar, delning av Urklipp och omdirigering av skrivare?** Alla de här funktionerna stöds i Azure RemoteApp. Delning av Urklipp och omdirigering av skrivare aktiveras som standard. Läs mer om omdirigering [här](remoteapp-redirection.md). 

## <a name="template-images"></a>Mallavbildningar
* **Kan jag använda ett moln eller en befintlig virtuell dator som hello mall för RemoteApp-samlingen?** Visst! Du kan skapa en avbildning baserat på en Azure VM, använda en hello avbildningarna som ingår i prenumerationen eller skapa en anpassad avbildning. Kolla in hello [RemoteApp-avbildningsalternativen](remoteapp-imageoptions.md).

## <a name="network-options"></a>Nätverksalternativ
* **Hej hybridsamling kräver ett VNET. Kan vi använda vårt befintliga VNET?** Du kan om hello befintliga VNET är ett Azure VNET. I avsnittet ”steg 1: konfigurera ditt virtuella nätverk” i hello [anvisningarna för hybridsamling](remoteapp-create-hybrid-deployment.md) för mer information.
* **Kan jag använda ett VNET med en molnsamling?** Det går bra. Mer info finns i artikeln om att [skapa en molnsamling](remoteapp-create-cloud-deployment.md), speciellt i Steg 1.

## <a name="authentication-options"></a>Autentiseringsalternativ
* **Hur är det med autentisering? Vilka metoder stöds?**  hello molnsamlingen stöder Microsoft-konton och Azure Active Directory-konton som är Office 365-konton. Hej hybridsamlingen stöder enbart Azure Active Directory-konton som har synkroniserats (med ett verktyg som [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) från en Windows Server Active Directory-distribution, särskilt antingen synkroniserats med hello Alternativet för synkronisering av lösenord eller synkroniserade med Active Directory Federation Services (AD FS) konfigurerats. Du kan även konfigurera [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

> [!NOTE]
> hello Azure Active Directory-användare måste komma från hello-klient som är associerad med din prenumeration. (Du kan visa och ändra din prenumeration på hello **inställningar** fliken hello-portalen. Se [ändra hello Azure Active Directory-klient som används av RemoteApp](remoteapp-changetenant.md) för mer information.)
> 
> 

* **Varför kan jag inte ge mitt Azure Active Directory-kontoåtkomst?**  hello Azure Active Directory-användare måste vara från hello-katalog som är associerad med din prenumeration. Du kan visa eller ändra den katalogen på hello-inställningar i hello-portalen. Se [ändra hello Azure Active Directory-klient som används av RemoteApp](remoteapp-changetenant.md) för mer information.

## <a name="clients---what-device-can-i-use-tooaccess-azure-remoteapp"></a>Klienter – vilka enheter kan jag använda tooaccess Azure RemoteApp?
Du kan hitta praktisk klientinformation, däribland anvisningar för att installera hello olika klienter på [åtkomst till dina appar i Azure RemoteApp](remoteapp-clients.md).

* **Vilka enheter och operativsystem hello klientprogram stöder?**
  Första hello datorer och surfplattor: 
  
  * Windows 10 (klientförhandsgranskning)
  * Windows 8.1 och Windows 8
  * Windows 7 Service Pack 1
  * Mac OS X
  * Windows RT
  * Android-surfplattor
  * iPad-enheter

    Och hello telefoner:
  * iPhone
  * Android-telefoner
  * Windows Phone
    
    [Hämta](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) en RemoteApp-klient nu.
* **Stöder Azure RemoteApp tunna klienter?** Ja, hello följande tunna Windows Embedded-klienter stöds:
  
  * Windows Embedded Standard 7
  * Windows Embedded 8 Standard
  * Windows Embedded 8.1 Industry Pro
  * Windows 10 IoT Enterprise
* **Vilken version av Windows Server stöds för hello Remote värd för fjärrskrivbordssession (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Support och feedback
* **Vad är hello RemoteApp för supportplan?** Support för fakturering och prenumerationer ges utan kostnad. Teknisk support är tillgänglig via hello [Azure-tjänstplaner](https://azure.microsoft.com/support/plans/). Du hittar även kostnadsfri communitysupport i [Azure-diskussionsforumet](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
* **Hur gör jag för att skicka feedback?** Besök hello [Feedbackforum](https://feedback.azure.com/forums/247748-azure-remoteapp/).
* **Vem kan jag kontakta toolearn mer om Azure RemoteApp?** I tillägg tooour [diskussionsforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), vilket är en bra utgångspunkt toopost frågor, du kan ansluta till hello veckovis [be hello experter webbseminariet](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), där pratar vi om allt som rör RemoteApp.
* **Finns det någon RemoteApp-dokumentation?** Bra att du frågade. Dessutom toohello hjälpinnehåll i hello portalen lådan (Klicka bara på hello **?** på en sida i portalen hello) hello följande artiklar är tillgängliga tooteach du om RemoteApp:
  
  * **Kom igång:**
    * [Vad är RemoteApp?](remoteapp-whatis.md)
    * [Vad är hello RemoteApp-mallavbildningarna?](remoteapp-images.md)
    * [Hur fungerar licensiering?](remoteapp-licensing.md)
    * [Hur fungerar RemoteApp och Office tillsammans?](remoteapp-o365.md)
    * [Hur fungerar omdirigering i RemoteApp](remoteapp-redirection.md)?
  * **Distribuera:**
    * [Skapa en anpassad mallavbildning](remoteapp-create-custom-image.md)
    * [Skapa en hybridsamling](remoteapp-create-hybrid-deployment.md)
    * [Skapa en molnsamling](remoteapp-create-cloud-deployment.md)
    * [Konfigurera Azure Active Directory för RemoteApp](remoteapp-ad.md)
    * [Publicera en app i RemoteApp](remoteapp-publish.md)
  * **Hantera:**
    
    * [Lägga till användare](remoteapp-user.md)
    * [Tips för att konfigurera och använda RemoteApp](remoteapp-bestpractices.md)    
    
    Videor! Det finns även ett antal videor om RemoteApp. Vissa innehåller en introduktion ([introduktion tooAzure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) medan andra går igenom distribution ([Cloud deployment](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) och [hybriddistribution](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Kolla in dem!

### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att i tillägg toorating den här artikeln och skriva kommentarer nedan, kan du göra ändringar toohello själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Rulla uppåt och klicka på **Redigera på GitHub** toomake ändringar - de kommer toous för granskning och sedan, när vi har godkänt dem, visas dina ändringar och förbättringar här.

