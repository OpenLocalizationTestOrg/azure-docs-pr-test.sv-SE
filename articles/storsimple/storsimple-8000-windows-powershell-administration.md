---
title: "aaaPowerShell för StorSimple-enhetshantering | Microsoft Docs"
description: "Lär dig hur toouse Windows PowerShell för StorSimple toomanage din StorSimple-enhet."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/03/2017
ms.author: alkohli@microsoft.com
ms.openlocfilehash: e9e4bd025933cdef68b861d93749a107d1689536
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-for-storsimple-tooadminister-your-device"></a>Använda Windows PowerShell för StorSimple tooadminister din enhet

## <a name="overview"></a>Översikt

Windows PowerShell för StorSimple innehåller ett kommandoradsgränssnitt som du kan använda toomanage din Microsoft Azure StorSimple-enhet. Som framgår av hello-namnet är ett kommandoradsgränssnitt för Windows PowerShell-baserade, som är inbyggd i ett begränsat körningsutrymme. Ur hello av hello användare på kommandoraden för hello visas ett begränsat körningsutrymme som en begränsad version av Windows PowerShell. Det här gränssnittet har ytterligare dedikerad cmdlets som är inriktad på att hantera din Microsoft Azure StorSimple-enhet utan att några av hello grundläggande funktionerna i Windows PowerShell.

Den här artikeln beskriver hello Windows PowerShell för StorSimple-funktioner, inklusive hur du kan ansluta toothis gränssnitt, och innehåller länkar toostep-för-steg-procedurer eller arbetsflöden som du kan utföra med hjälp av det här gränssnittet. hello arbetsflöden är hur tooregister enheten, konfigurera hello nätverksgränssnittet på din enhet, installera uppdateringar som kräver hello enheten toobe i underhållsläge, ändra hello enhetens tillstånd och felsöka eventuella problem som kan uppstå.

När du har läst den här artikeln kommer du att kunna:

* Ansluta tooyour StorSimple-enhet med Windows PowerShell för StorSimple.
* Administrera din StorSimple-enhet med Windows PowerShell för StorSimple.
* Få hjälp i Windows PowerShell för StorSimple.

> [!NOTE]
> * Windows PowerShell för StorSimple-cmdlets kan du toomanage din StorSimple-enhet från en seriekonsolen eller via fjärranslutning via Windows PowerShell-fjärrkommunikation. Mer information om varje hello enskilda cmdletar som kan användas i det här gränssnittet finns för[cmdlet-referens för Windows PowerShell för StorSimple](https://technet.microsoft.com/library/dn688168.aspx).
> * hello Azure PowerShell StorSimple cmdlets är en annan samling av cmdlets som gör att tooautomate StorSimple-servicenivå och migreringen från hello kommandorad. Mer information om hello Azure PowerShell-cmdlets för StorSimple gå toohello [cmdlet-referens för Azure StorSimple](https://docs.microsoft.com/powershell/servicemanagement/azure.storsimple/v3.1.0/azure.storsimple).


Du kan komma åt hello Windows PowerShell för StorSimple med någon av följande metoder hello:

* [Ansluta tooStorSimple enhetens seriekonsol](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [Fjärransluta tooStorSimple med Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-toowindows-powershell-for-storsimple-via-hello-device-serial-console"></a>Ansluta tooWindows PowerShell för StorSimple via hello enhetens seriekonsol

Du kan [hämta PuTTY](http://www.putty.org/) eller liknande terminalemulering programvara tooconnect tooWindows PowerShell för StorSimple. Du behöver tooconfigure PuTTY specifikt tooaccess hello Microsoft Azure StorSimple-enhet. hello följande avsnitt innehåller detaljerade anvisningar om hur tooconfigure PuTTy och Anslut toohello enhet. Olika menyalternativen i hello seriekonsolen beskrivs också.

### <a name="putty-settings"></a>PuTTY-inställningar

Kontrollera att du använder hello följande inställningar för PuTTY tooconnect toohello Windows PowerShell-gränssnittet från hello seriekonsol.

#### <a name="tooconfigure-putty"></a>tooconfigure PuTTY

1. I hello PuTTY **omkonfiguration** i dialogrutan hello **kategori** väljer **tangentbord**.
2. Kontrollera att hello följande alternativ är valt (detta är hello standardinställningarna när du startar en ny session).
   
   | Tangentbord objekt | Välj |
   | --- | --- |
   | BACKSTEG |Kontroll-? (127) |
   | Hem- och nycklar |Standard |
   | Funktionstangenter och tangentbordet |ESC [n ~ |
   | Ursprungligt tillstånd för piltangenterna |Normal |
   | Ursprungligt tillstånd för numeriska tangentbordet |Normal |
   | Aktivera funktioner för extra tangentbord |CTRL + ALT + skiljer sig från AltGr |
   
    ![Putty inställningar som stöds](./media/storsimple-windows-powershell-administration/IC740877.png)
3. Klicka på **Använd**.
4. I hello **kategori** väljer **översättning**.
5. I hello **Remote teckenuppsättningen** väljer **UTF-8**.
6. Under **hantering av raden ritning tecken**väljer **Använd Unicode-kodpunkter för rad ritning**. hello visar följande skärmbild hello rätt PuTTY val.
   
    ![UTF Putty-inställningar](./media/storsimple-windows-powershell-administration/IC740878.png)
7. Klicka på **Använd**.

Du kan nu använda PuTTY tooconnect toohello enhetens seriekonsol genom att göra hello följande steg.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-hello-serial-console"></a>Om hello seriekonsol

När du öppnar hello Windows PowerShell-gränssnittet för din StorSimple-enhet via seriekonsolen hello visas Banderollmeddelandet följt av menyalternativen.

Hej Banderollmeddelandet innehåller grundläggande StorSimple enhetsinformation, till exempel hello modellen, namn, version för installerad programvara och status för hello-domänkontrollant som du ansluter till. hello följande bild visar ett exempel på Banderollmeddelandet.

![Seriell Banderollmeddelandet](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> Du kan använda hello banderoll meddelandet tooidentify om hello domänkontrollant som du är ansluten toois _Active_ eller _passiva_.

Följande bild visar hello hello olika runspace alternativ som är tillgängliga i menyn för seriekonsolen av hello.

![Registrera din enhet 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Du kan välja mellan hello följande inställningar:

1. **Logga in med fullständig åtkomst** det här alternativet kan du tooconnect (med hello korrekta autentiseringsuppgifter) toohello **SSAdminConsole** runspace på hello lokala domänkontrollant. (hello lokal domänkontrollant är hello-domänkontrollant som du för närvarande kommer åt via hello seriekonsol av StorSimple-enheten). Det här alternativet kan också användas tooallow Microsoft-supporten tooaccess obegränsad runspace (en supportsession) tootroubleshoot problemen möjliga enhet. När du använder alternativ 1 toolog på kan tillåta du hello Microsoft-supporten tekniker tooaccess obegränsad runspace genom att köra en viss cmdlet. Mer information finns för[starta en session med stöd för](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).
   
2. **Logga in toopeer domänkontrollant med fullständig åtkomst** det här alternativet är hello samma som alternativ 1, förutom att du kan ansluta (med hello korrekta autentiseringsuppgifter) toohello **SSAdminConsole** runspace på hello peer-domänkontrollant. Eftersom hello StorSimple-enheten är en hög tillgänglighet med två domänkontrollanter i en konfiguration för aktivt-passivt refererar peer toohello andra domänkontrollanter i hello-enhet som du kommer åt via hello seriekonsol).
   Liknande toooption 1, det här alternativet kan också vara används tooallow Microsoft-supporten tooaccess obegränsad runspace på en peer-domänkontrollant.

3. **Ansluta med begränsad åtkomst** det här alternativet är används tooaccess Windows PowerShell-gränssnittet i begränsat läge. Du ombeds inte ange autentiseringsuppgifter. Det här alternativet ansluter tooa mer begränsat körningsutrymme jämfört med toooptions 1 och 2.  Vissa av hello uppgifter som är tillgängliga via alternativ 1 som **kan* utföras i den här runspace är:
   
   * Återställ fabriksinställningarna toohello
   * Ändra hello lösenord
   * Aktivera eller inaktivera stöd för åtkomst
   * Tillämpa uppdateringar
   * Installera snabbkorrigeringar

    > [!NOTE]
    > Det är hello önskade alternativ om du har glömt hello enhetens administratörslösenord och kan inte ansluta via alternativ 1 eller 2.

4. **Ändra språk** det här alternativet kan du toochange hello visningsspråket på hello Windows PowerShell-gränssnittet. hello-språk som stöds är engelska, japanska, ryska, franska, koreanska söder, spanska, italienska, tyska, kinesiska och portugisiska (Brasilien).

## <a name="connect-remotely-toostorsimple-using-windows-powershell-for-storsimple"></a>Fjärransluta tooStorSimple som använder Windows PowerShell för StorSimple

Du kan använda Windows PowerShell-fjärrkommunikation tooconnect tooyour StorSimple-enhet. När du ansluter det här sättet kan se du inte en meny. (Du se en meny endast om du använder hello seriekonsolen på hello enheten tooconnect. Fjärranslutning tar dig direkt toohello motsvarigheten till ”alternativ 1 – fullständig åtkomst” på hello seriekonsol.) Med Windows PowerShell-fjärrkommunikation ansluta tooa specifika runspace. Du kan också ange hello visningsspråket.

hello visningsspråket är oberoende av hello språk som du ställer in med hello **ändra språk** alternativ i hello menyn för seriekonsolen. Fjärr-PowerShell hämtar automatiskt upp hello språk för hello enheten om du ansluter från om inget anges.

> [!NOTE]
> Du kan använda Windows PowerShell-fjärrkommunikation och hello virtuell värd tooconnect toohello moln installation om du arbetar med virtuella Microsoft Azure-värdar och StorSimple moln installationer. Om du har ställt in en plats på hello värden när toosave information från Windows PowerShell-sessionen hello bör du vara medveten att hello _alla_ huvudnamn innehåller endast autentiserade användare. Därför, om du har lagt upp hello tooallow åtkomst till resursen av _alla_ och du ansluter utan att ange autentiseringsuppgifter, hello oautentiserade anonym principal används och visas ett felmeddelande. toofix problemet på hello dela värden du måste aktivera hello gästkontot och ge hello gäst konto fullständig åtkomst toohello resurs eller du måste ange giltiga autentiseringsuppgifter tillsammans med hello Windows PowerShell-cmdlet.


Du kan använda HTTP eller HTTPS tooconnect via Windows PowerShell-fjärrkommunikation. Använda hello instruktioner hello följande kurser:

* [Fjärransluta via HTTP](storsimple-remote-connect.md#connect-through-http)
* [Fjärransluta via HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Säkerhetsaspekter för anslutning

När du beslutar hur tooconnect tooWindows PowerShell för StorSimple, Tänk hello följande:

* Ansluta direkt toohello enhetens seriekonsol är säkra, men anslutande toohello seriekonsolen över nätverksväxlar är inte. Var försiktig av hello säkerhetsrisk när du ansluter toodevice seriellt via nätverksväxlar.
* Ansluter via en HTTP-session kan erbjuda mer säkerhet än att ansluta via hello seriekonsol över nätverket. Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk.
* Ansluter via en HTTPS-session är hello säkraste och hello rekommenderas.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Administrera din StorSimple-enhet med Windows PowerShell för StorSimple

hello visas nedan en sammanfattning av alla hello vanliga administrativa uppgifter och komplexa arbetsflöden som kan utföras i hello Windows PowerShell-gränssnittet för din StorSimple-enhet. Mer information om varje arbetsflöde klickar du på hello lämpliga posten i hello tabellen.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell för StorSimple-arbetsflöden

| Om du vill toodo... | Använd den här proceduren. |
| --- | --- |
| Registrera din enhet |[Konfigurera och registrera hello-enhet med hjälp av Windows PowerShell för StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| Konfigurera webbproxy</br>Visa web proxy-inställningar |[Konfigurera en proxyserver för din StorSimple-enhet](storsimple-8000-configure-web-proxy.md) |
| Ändra DATA 0 inställningar för nätverksgränssnittet på din enhet |[Ändra DATA 0-nätverksgränssnittet för din StorSimple-enhet](storsimple-8000-modify-data-0.md) |
| Stoppa en domänkontrollant </br> Starta om eller stänga av en domänkontrollant </br> Stänga av en enhet</br>Återställ standardinställningar för hello enheten toofactory |[Hantera styrenheter](storsimple-8000-manage-device-controller.md) |
| Installera Underhåll läge uppdateringar och snabbkorrigeringar |[Uppdatera din enhet](storsimple-update-device.md) |
| Ange underhållsläge </br>Avsluta underhållsläge |[Lägen för StorSimple-enhet](storsimple-8000-device-modes.md) |
| Skapa ett supportpaket</br>Dekryptera och redigera ett supportpaket |[Skapa och hantera ett stödpaket](storsimple-8000-create-manage-support-package.md) |
| Starta en session med stöd</br> |[Starta en session med stöd i Windows PowerShell för StorSimple](storsimple-8000-create-manage-support-package.md#create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Få hjälp i Windows PowerShell för StorSimple

I Windows PowerShell för StorSimple är hjälp tillgänglig. En aktuell version av hjälpen finns även, som du kan använda tooupdate hello hjälp i systemet.

Få hjälp i det här gränssnittet är liknande toothat i Windows PowerShell och de flesta av hello hjälp-relaterade cmdlets fungerar. Du hittar hjälp för Windows PowerShell online i hello TechNet-biblioteket: [med Windows PowerShell-skript](http://go.microsoft.com/fwlink/?LinkID=108518).

hello följer en kort beskrivning av hello typer av hjälp för det här Windows PowerShell-gränssnittet, inklusive hur tooupdate hello hjälp.

### <a name="tooget-help-for-a-cmdlet"></a>tooget hjälp för en cmdlet

* tooget hjälpen för cmdlet eller funktion, Använd hello följande kommando:`Get-Help <cmdlet-name>`
* tooget onlinehjälpen för alla cmdletar använder tidigare hello-cmdlet med hello `-Online` parameter:`Get-Help <cmdlet-name> -Online`
* För fullständig hjälp kan du använda hello `–Full` parameter, och använda hello exempel `–Examples` parameter.

### <a name="tooupdate-help"></a>tooupdate hjälp

Du kan enkelt uppdatera hello hjälp i hello Windows PowerShell-gränssnittet. Utföra hello följande steg tooupdate hello hjälp i systemet.

#### <a name="tooupdate-cmdlet-help"></a>tooupdate cmdlet hjälp
1. Starta Windows PowerShell med hello **kör som administratör** alternativet.
2. I hello kommandotolk, skriver du:`Update-Help`
3. hello uppdaterad hjälp filer kommer att installeras.
4. När hello hjälpfilerna är installerade, skriver du: `Get-Help Get-Command`. Detta visar en lista över cmdlets som hjälp är tillgänglig.

> [!NOTE]
> tooget en lista över alla tillgängliga hello-cmdletar i en runspace logga in toohello motsvarande menyalternativ och köra hello `Get-Command` cmdlet.


## <a name="next-steps"></a>Nästa steg

Om du får problem med din StorSimple-enhet när du utför en hello ovan arbetsflöden finns för[StorSimple distributioner felsökningsverktyg](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

