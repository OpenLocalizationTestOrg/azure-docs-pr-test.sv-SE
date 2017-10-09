---
title: "aaaSet in webbproxy för en StorSimple-enhet | Microsoft Docs"
description: "Lär dig hur toouse Windows PowerShell för StorSimple tooconfigure web proxy-inställningar för din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6c2ca351-a7c6-4da6-ab5e-c081e6d08261
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: c0213eb2a1902ff994147bb16593f0ff300eb70c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>Konfigurera en proxyserver för din StorSimple-enhet
## <a name="overview"></a>Översikt
Den här självstudiekursen beskriver hur toouse Windows PowerShell för StorSimple tooconfigure och visa web proxy-inställningar för din StorSimple-enhet. hello web proxy-inställningar som används av hello StorSimple-enhet när du kommunicerar med hello molnet. En webbproxyserver är används tooadd ytterligare en säkerhetsnivå filtrera innehåll, cachelagra tooease bandbreddskraven eller även bidra med analytics.

Webbproxyn är en valfri konfiguration för din StorSimple-enhet. Du kan konfigurera webbproxy endast via Windows PowerShell för StorSimple. hello konfiguration är en tvåstegsprocess:

1. Du måste först konfigurera webbproxyinställningarna via hello installationsguiden eller Windows PowerShell för StorSimple-cmdlets.
2. Sen kan du aktivera hello konfigurerad webbproxyinställningarna via Windows PowerShell för StorSimple-cmdlets.

När hello webbproxykonfigurationen är klar kan visa du hello konfigurerad webbproxyinställningarna i både hello Microsoft Azure StorSimple Manager-tjänsten och hello Windows PowerShell för StorSimple. 

När du har läst den här självstudiekursen kommer du att kunna:

* Konfigurera webbproxy med hjälp av installationsguiden och cmdlet: ar
* Aktivera webbproxy med hjälp av cmdlet: ar
* Visa webbproxyinställningarna i hello klassiska Azure-portalen
* Felsök fel under webbproxykonfigurationen

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Konfigurera webbproxy via Windows PowerShell för StorSimple
Du använder någon av följande tooconfigure webbproxyinställningarna hello:

* Konfigurera guiden tooguide dig genom hello konfigurationssteg.
* Cmdlets i Windows PowerShell för StorSimple.

Var och en av dessa metoder beskrivs i följande avsnitt hello.

## <a name="configure-web-proxy-via-hello-setup-wizard"></a>Konfigurera webbproxy via hello installationsguiden
Du kan använda hello installationsprogrammet guiden tooguide dig genom hello steg för webbproxykonfigurationen. Utföra hello följande steg tooconfigure webbproxy på enheten.

#### <a name="tooconfigure-web-proxy-via-hello-setup-wizard"></a>tooconfigure webbproxy via hello installationsguiden
1. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst** och ange hello **enhetens administratörslösenord**. Typen hello följande kommando toostart en installationsprogrammet guidesession:
   
    `Invoke-HcsSetupWizard`
2. Om detta är hello första gången du använder hello installationsguiden för registrering av enheten måste tooconfigure alla hello krävs nätverksinställningar tills du når hello webbproxykonfigurationen. Om enheten redan är registrerad, kan du acceptera alla hello konfigurerade nätverksinställningarna tills du når hello webbproxykonfigurationen. I installationsguiden för hello när begärd tooconfigure web proxy-inställningar, Skriv **Ja**.
3. För hello **URL för Web Proxy**, ange hello IP-adress eller hello fullständigt kvalificerade domännamnet (FQDN) för din web proxy server och hello TCP-portnummer som du vill att din enhet toouse vid kommunikation med hello molnet. Använd hello följande format:
   
    `http://<IP address or FQDN of hello web proxy server>:<TCP port number>`
   
    Som standard, har TCP-portnummer 8080 angetts.
4. Välj hello autentiseringstyp som **NTLM**, **grundläggande**, eller **ingen**. Grundläggande är minst säkra hello-autentisering för hello proxyserverkonfiguration. NT LAN Manager (NTLM) är ett mycket säkert och komplexa autentiseringsprotokoll som använder ett trevägs meddelandesystem (ibland fyra om det krävs ytterligare integritet) tooauthenticate en användare. hello standardautentisering är NTLM. Mer information finns i [grundläggande](http://hc.apache.org/httpclient-3.x/authentication.html) och [NTLM-autentisering](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **Hello enheten övervakning diagram fungerar inte när grundläggande eller NTLM-autentisering är aktiverat i hello proxyserverkonfiguration för hello enhet i hello StorSimple Manager-tjänsten. För hello övervakning diagram toowork, behöver du tooensure att autentiseringen har angetts tooNONE.**
   > 
   > 
5. Om du använder autentisering anger du en **Web Proxyanvändarnamnet** och en **Web Proxylösenord**. Du måste också tooconfirm hello lösenord.
   
    ![Konfigurera webbproxy på StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Om du registrerar din enhet för hello första gången, fortsätter du med hello registrering. Om enheten redan har registrerats avslutas hello guiden. hello konfigurerade inställningar kommer att sparas.

Webbproxy nu aktiveras också. Du kan hoppa över hello [aktivera webbproxy](#enable-web-proxy) steget och gå direkt för[visa webbproxyinställningarna i hello klassiska Azure-portalen](#view-web-proxy-settings-in-the-azure-classic-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfigurera webbproxy via Windows PowerShell för StorSimple-cmdlets
Ett annat sätt tooconfigure webbproxyinställningarna är via hello Windows PowerShell för StorSimple-cmdlets. Utför följande steg tooconfigure webbproxy hello.

#### <a name="tooconfigure-web-proxy-via-cmdlets"></a>tooconfigure webbproxy via cmdlets
1. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**. När du uppmanas att ange hello **enhetens administratörslösenord**. hello standardlösenordet är `Password1`.
2. I hello kommandotolk, skriver du:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Ange och bekräfta hello lösenord när du uppmanas, enligt nedan.
   
    ![Konfigurera webbproxy på StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

hello webbproxy har nu konfigurerats och måste toobe aktiverat.

## <a name="enable-web-proxy"></a>Aktivera webbproxy
Webbproxyn är inaktiverad som standard. När du har konfigurerat hello web proxy-inställningar på din StorSimple-enhet behöver du toouse hello Windows PowerShell för StorSimple tooenable hello web proxy-inställningar.

> [!NOTE]
> **Det här steget kommer krävs inte om du använde hello installationsprogrammet guiden tooconfigure webbproxy. Webbproxy aktiveras automatiskt som standard efter en installationsprogrammet guidesessionen.**
> 
> 

Utför hello följande i Windows PowerShell för StorSimple tooenable webbproxy på din enhet:

#### <a name="tooenable-web-proxy"></a>tooenable webbproxy
1. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**. När du uppmanas att ange hello **enhetens administratörslösenord**. hello standardlösenordet är `Password1`.
2. I hello kommandotolk, skriver du:
   
    `Enable-HcsWebProxy`
   
    Du har nu aktiverats hello webbproxykonfigurationen på StorSimple-enheten.
   
    ![Konfigurera webbproxy på StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-hello-azure-classic-portal"></a>Visa webbproxyinställningarna i hello klassiska Azure-portalen
Hej webbproxyinställningarna konfigureras via hello Windows PowerShell-gränssnittet och kan inte ändras från inom hello klassiska portalen. Du kan dock visa de konfigurerade inställningarna i hello klassiska portalen. Utför följande steg tooview webbproxy hello.

#### <a name="tooview-web-proxy-settings"></a>tooview web proxy-inställningar
1. Navigera för**StorSimple Manager-tjänsten > enheter**. Markera och klicka på en enhet och gå sedan för**konfigurera**.
2. Rulla nedåt på hello **konfigurera** sidan för**Web proxyinställningar** avsnitt. Du kan visa hello konfigurerats web proxy-inställningar på din StorSimple-enhet som visas nedan.
   
    ![Visa webbproxy i hanteringsportalen](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)

## <a name="errors-during-web-proxy-configuration"></a>Fel under webbproxykonfigurationen
Om hello web proxy-inställningar har konfigurerats felaktigt, blir felmeddelanden visas toohello användaren i Windows PowerShell för StorSimple. hello i den följande tabellen beskrivs några av dessa meddelanden, troliga orsaker och rekommenderade åtgärder.

| Serienummer. | HRESULT-felkoden | Möjliga underliggande orsaker | Rekommenderad åtgärd |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Kommandot körs från hello passiva domänkontrollant och det är inte kan toocommunicate hello aktiva styrenhet. |Kör hello-kommando på hello aktiva styrenhet. toorun hello kommando från hello passiva domänkontrollant, behöver du toofix hello anslutningen från passiva tooactive domänkontrollant. Du behöver tooengage Microsoft-supporten om den här anslutningen har brutits. |
| 2. |0x800710dd - hello Åtgärdsidentifieraren är inte giltig |Proxyinställningar stöds inte på virtuella StorSimple-enheten. |Proxyinställningar stöds inte på virtuella StorSimple-enheten. De kan endast konfigureras på fysiska StorSimple-enheten. |
| 3. |0x80070057 - ogiltig parameter |En hello parametrar som angavs för hello proxyinställningar är inte giltig. |hello URI har inte angetts i rätt format. Använd hello följande format:`http://<IP address or FQDN of hello web proxy server>:<TCP port number>` |
| 4. |0x800706ba - RPC-servern är inte tillgänglig |hello orsaken är en av följande hello:</br></br>Klustret är inte in.</br></br>DataPath-tjänsten körs inte.</br></br>hello kommandot körs från passiva domänkontrollant och det är inte kan toocommunicate hello aktiva styrenhet. |Genomföra Microsoft-supporten tooensure som hello klustret är Kontrollera igång och datapath-tjänsten körs.</br></br>Kör kommandot hello från hello aktiva styrenhet. Om du vill att toorun hello kommandot från hello passiva domänkontrollant måste tooensure hello passiva styrenhet kan kommunicera med hello aktiva styrenhet. Du behöver tooengage Microsoft-supporten om den här anslutningen har brutits. |
| 5. |0X800706be - RPC-anrop misslyckades |Klustret har stoppats. |Engagera du Microsoft Support tooensure som hello klustret är igång. |
| 6. |0x8007138f - resurs inte hittas |Klusterresursen för Platform-tjänsten hittades inte. Detta kan hända när hello installationen inte var korrekt. |Du kan behöva tooperform en fabriksåterställning på enheten. Du kan behöva toocreate en plattform resurs. Kontakta Microsoft Support för nästa steg. |
| 7. |0x8007138c - klusterresursen inte online |Plattform eller datapath klusterresurser är inte online. |Kontakta Microsoft Support toohelp se till att hello datapath och plattform tjänstresurs är online. |

> [!NOTE]
> * hello ovan lista över felmeddelanden är inte komplett. 
> * Proxyinställningar för fel relaterade tooweb visas inte i hello klassiska Azure-portalen i StorSimple Manager-tjänsten. Om det finns ett problem med webbproxy när hello konfigurationen har slutförts, hello enhetens status ändras för**Offline** i hello klassiska portalen. |
> 
> 

## <a name="next-steps"></a>Nästa steg
* Om du får problem när du distribuerar din enhet eller konfigurera webbproxyinställningarna finns för[felsöka distribution din StorSimple-enheten](storsimple-troubleshoot-deployment.md).
* hur toouse hello StorSimple Manager-tjänsten, gå för toolearn[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

