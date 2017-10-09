---
title: aaaTroubleshoot StorSimple distributionsproblem | Microsoft Docs
description: "Beskriver hur toodiagnose och åtgärda fel som uppstår när du distribuerar först StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f8352eaa-193c-42d1-8818-0b8e02d8485d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 9a31a3cfb3be577500b226c2bc8172dd8664bad9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Felsöka distributionsproblem för StorSimple-enhet
## <a name="overview"></a>Översikt
Den här artikeln innehåller användbar felsökningsinformation för din Microsoft Azure StorSimple-distribution. Här beskrivs vanliga problem och möjliga orsaker rekommenderade steg toohelp du löser problem som kan uppstå när du konfigurerar StorSimple. Den här informationen gäller tooboth hello lokala fysiska StorSimple-enheten och hello virtuella StorSimple-enheten.

> [!NOTE]
> Enheten configuration-relaterade problem som du kan stöta kan uppstå när du distribuerar hello enhet för hello första gången eller de kan inträffa senare, när hello enheten är i drift. Den här artikeln fokuserar på att felsöka distributionsproblem för första gången. tootroubleshoot en operativa enhet, gå för[felsökning av problem med operativa enheter](storsimple-troubleshoot-operational-device.md).
> 
> 

Den här artikeln beskriver hello felsökningsverktyg StorSimple distributioner också och innehåller en stegvis Felsökningsexempel.

## <a name="first-time-deployment-issues"></a>Första gången distributionsproblem
Om du stöter på ett problem när du distribuerar din enhet för hello första gången, bör du hello följande:

* Om du felsöker en fysisk enhet, kontrollerar du att hello maskinvara har installerats och konfigurerats enligt beskrivningen i [installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md) eller [installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md).
* Kontrollera förutsättningarna för distribution. Se till att du har alla hello informationen som beskrivs i hello [checklista för distributionskonfiguration](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
* Granska hello StorSimple viktig information toosee om hello problem beskrivs. hello viktig information innehåller lösningar för kända problem. 

Under distributionen av enheten utfärdar hello vanligaste som användare står inför uppstå när de kör installationsguiden för hello och när de registrerar hello enheten via Windows PowerShell för StorSimple. (Du använder Windows PowerShell för StorSimple tooregister och konfigurera din StorSimple-enhet. Mer information om enhetsregistrering finns [steg3: konfigurera och registrera enheten via Windows PowerShell för StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

hello följande avsnitt kan hjälpa dig att lösa problem som kan uppstå när du konfigurerar hello StorSimple-enheten för hello första gången.

## <a name="first-time-setup-wizard-process"></a>Installationsguiden för första gången
hello följande steg sammanfattar hello installationsguiden. Detaljerad installationsinformation finns [distribuera din lokala StorSimple-enhet](storsimple-deployment-walkthrough.md).

1. Kör hello [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) cmdlet toostart hello med installationsguiden som hjälper dig att hello återstående steg. 
2. Konfigurera nätverket hello: hello installationsguiden kan du konfigurera nätverksinställningar för hello DATA 0-nätverksgränssnittet på din StorSimple-enhet. Inställningarna omfattar hello följande:
   * Virtuella IP-adresser (VIP), nätmask och gateway – hello [Set HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet har körts i hello bakgrund. Den konfigurerar hello IP-adress, nätmask och gateway för hello DATA 0-nätverksgränssnittet på din StorSimple-enhet.
   * Primär DNS-server – hello [Set HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet har körts i hello bakgrund. Konfigurerar den hello DNS-inställningarna för din StorSimple-lösning.
   * NTP-server – hello [Set HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet har körts i hello bakgrund. Konfigurerar den hello NTP-serverinställningar för din StorSimple-lösning.
   * Valfria web proxy – hello [Set HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet har körts i hello bakgrund. Den anger och aktiverar hello webbproxykonfiguration för din StorSimple-lösning.
3. Ställ in hello lösenord: hello nästa steg är tooset enhetsadministratör och StorSimple Snapshot Manager lösenord. Om du kör uppdatering 1, kommer du inte att nödvändiga tooset hello StorSimple Snapshot Manager lösenord.
   
   * hello enhetens administratörslösenord är används toolog på tooyour enhet. hello enheten standardlösenord är **Password1**.
   * Hej StorSimple Snapshot Manager lösenord krävs när du konfigurerar en enhet toouse StorSimple Snapshot Manager. Behöver du toofirst hello lösenordet i installationsguiden för hello och du kan sedan ange och ändra från hello StorSimple Manager-tjänsten. Det här lösenordet autentiseras hello enhet med StorSimple Snapshot Manager.
     
     > [!IMPORTANT]
     > Samlas in innan registrering av lösenord, men används bara när du har registrerat hello enhet. Om det finns ett fel tooapply ett lösenord måste kommer du att tillfrågas toosupply hello lösenord igen förrän hello nödvändiga lösenord (som uppfyller krav på komplexitet hello) samlas in.
     > 
     > 
4. Registrera hello enhet: hello sista steget är tooregister hello enheten med hello StorSimple Manager-tjänsten körs i Microsoft Azure. hello registrering måste du för[hämta nyckel för tjänstregistrering hello](storsimple-manage-service.md#get-the-service-registration-key) från hello klassiska Azure-portalen och ange den i hello installationsguiden. När hello enheten har registrerats tillhandahålls en krypteringsnyckel för tjänstdata tooyou. Att se till att tookeep krypteringsnycklar nyckeln på en säker plats eftersom den är nödvändiga tooregister alla efterföljande enheter med hello-tjänsten.

## <a name="common-errors-during-device-deployment"></a>Vanliga fel under distributionen av enheten
Hej följande tabeller listan hello vanliga fel som kan uppstå när du:

* Konfigurera inställningar för hello krävs.
* Konfigurera proxyinställningar för hello valfri webbserver.
* Ställ in hello enhetsadministratör och StorSimple Snapshot Manager lösenord. 
* Registrera hello-enhet. 

## <a name="errors-during-hello-required-network-settings"></a>Fel under hello krävs nätverksinställningar
| Nej. | Felmeddelande | Möjliga orsaker | Rekommenderad åtgärd |
| --- | --- | --- | --- |
| 1 |Anropa HcsSetupWizard: Det här kommandot kan endast köras på hello aktiva styrenhet. |Konfigurationen utfördes på hello passiva domänkontrollant. |Kör kommandot från hello aktiva styrenhet. Mer information finns i [identifiera en domänkontrollant för en aktiv på enheten](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Anropa HcsSetupWizard: Enheten är inte klar. |Det finns problem med hello nätverksanslutningen på DATA 0. |Kontrollera hello fysiska nätverksanslutningen på DATA 0. |
| 3 |Anropa HcsSetupWizard: Det finns en IP-adresskonflikt med ett annat system i nätverket hello (undantag från HRESULT: 0x80070263). |hello IP som angetts för DATA 0 var redan används av en annan dator. |Ange en ny IP-adress som inte används. |
| 4 |Anropa HcsSetupWizard: Det gick inte att en klusterresurs. (Undantag från HRESULT: 0x800713AE). |Duplicera VIP. hello tillhandahålls IP används redan. |Ange en ny IP-adress som inte används. |
| 5 |Anropa HcsSetupWizard: Ogiltig IPv4-adress. |hello IP-adress har angetts i ett felaktigt format. |Kontrollera hello format och ange IP-adressen igen. Mer information finns i [Ipv4-adresser][1]. |
| 6 |Anropa HcsSetupWizard: Ogiltig IPv6-adress. |hello IP-adress har angetts i ett felaktigt format. |Kontrollera hello format och ange IP-adressen igen. Mer information finns i [IPv6-adressering][2]. |
| 7 |Anropa HcsSetupWizard: Det finns inga fler slutpunkter från hello slutpunktsmappning. (Undantag från HRESULT: 0x800706D9) |hello klusterfunktionaliteten fungerar inte. |[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. |

## <a name="errors-during-hello-optional-web-proxy-settings"></a>Fel under hello valfria web proxy-inställningar
| Nej. | Felmeddelande | Möjliga orsaker | Rekommenderad åtgärd |
| --- | --- | --- | --- |
| 1 |Anropa HcsSetupWizard: Ogiltig parameter (undantag från HRESULT: 0x80070057) |En hello parametrar som angavs för hello proxyinställningar är inte giltig. |hello URI har inte angetts i hello korrekt format. Använd hello följande format: http://*<IP address or FQDN of hello web proxy server>*:*<TCP port number>* |
| 2 |Anropa HcsSetupWizard: RPC-servern är inte tillgänglig (undantag från HRESULT: 0x800706ba) |hello orsaken är en av följande hello:<ol><li>hello klustret är inte in.</li><li>hello passiva domänkontrollant inte kan kommunicera med hello aktiva styrenheten och hello kommandot körs från passiva domänkontrollant.</li></ol> |Beroende på hello rotorsaken:<ol><li>[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) toomake att hello klustret är igång.</li><li>Kör kommandot hello från hello aktiva styrenhet. Om du vill att toorun hello kommandot från hello passiva domänkontrollant måste tooensure hello passiva styrenheten kan kommunicera med hello aktiva styrenhet. Du behöver för[kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) om den här anslutningen har brutits.</li></ol> |
| 3 |Anropa HcsSetupWizard: RPC-anrop misslyckades (undantag från HRESULT: 0x800706be) |Klustret har stoppats. |[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) toomake att hello klustret är igång. |
| 4 |Anropa HcsSetupWizard: Klusterresursen gick inte att hitta (undantag från HRESULT: 0x8007138f) |hello klusterresurs hittades inte. Detta kan hända när hello installationen inte var korrekt. |Du kan behöva tooreset hello enheten toohello fabriksinställningarna. [Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) toocreate en klusterresurs. |
| 5 |Anropa HcsSetupWizard: Klusterresursen inte online (undantag från HRESULT: 0x8007138c) |Klusterresurser är inte online. |[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. |

## <a name="errors-related-toodevice-administrator-and-storsimple-snapshot-manager-passwords"></a>Fel relaterade toodevice administratör och lösenord för StorSimple Snapshot Manager
hello standard enhetens administratörslösenord är **Password1**. Det här lösenordet upphör att gälla efter hello första inloggning. Därför behöver du toouse hello installationsprogrammet guiden toochange den. Du måste ange ett nytt administratörslösenord för enheten när du registrerar hello enhet för hello första gången. 

Om du använder hello StorSimple Snapshot Manager-program som körs på hello Windows Server-värd toomanage hello enhet, måste du också ange ett lösenord för StorSimple Snapshot Manager under registreringen för första gången. 

Kontrollera att lösenorden uppfyller hello följande krav:

* Administratörslösenord för enheten ska vara mellan 8 och 15 tecken långt.
* Lösenordet för StorSimple Snapshot Manager ska vara 14 eller 15 tecken långt.
* Lösenord måste innehålla 3 av följande 4 teckentyper hello: gemener, versaler, siffror och särskilda. 
* Ditt lösenord kan inte hello samtidigt som hello senast 24 lösenord.

Dessutom, Tänk på att lösenord upphör att gälla varje år och kan ändras endast när du har registrerat enheten hello. Om hello registreringen misslyckas av någon anledning, ändras inte hello lösenord. Mer information om enhetsadministratör och lösenord för StorSimple Snapshot Manager gå för[Använd hello StorSimple Manager service toochange StorSimple-lösenord](storsimple-change-passwords.md).

Du kan stöta på ett eller flera av följande fel när du ställer in hello enhetsadministratör och lösenord för StorSimple Snapshot Manager hello.

| Nej. | Felmeddelande | Rekommenderad åtgärd |
| --- | --- | --- |
| 1 |hello lösenord överstiger hello maximal längd. |Använda ett lösenord som uppfyller dessa krav:<ul><li>Administratörslösenord för enheten måste vara mellan 8 och 15 tecken långt.</li><li>Lösenordet för StorSimple Snapshot Manager måste vara 14 eller 15 tecken långt.</li></ul> |
| 2 |hello lösenordet uppfyller inte hello krävs längd. |Använda ett lösenord som uppfyller dessa krav:<ul><li>Administratörslösenord för enheten måste vara mellan 8 och 15 tecken långt.</li><li>Lösenordet för StorSimple Snapshot Manager måste vara 14 eller 15 tecken långt.</lu></ul> |
| 3 |hello lösenordet måste innehålla gemener. |Lösenordet måste innehålla 3 av följande 4 teckentyper hello: gemener, versaler, siffror och särskilda. Kontrollera att lösenordet uppfyller dessa krav. |
| 4 |hello lösenordet måste innehålla numeriska tecken. |Lösenordet måste innehålla 3 av följande 4 teckentyper hello: gemener, versaler, siffror och särskilda. Kontrollera att lösenordet uppfyller dessa krav. |
| 5 |hello lösenordet måste innehålla specialtecken. |Lösenordet måste innehålla 3 av följande 4 teckentyper hello: gemener, versaler, siffror och särskilda. Kontrollera att lösenordet uppfyller dessa krav. |
| 6 |hello lösenordet måste innehålla 3 av följande 4 teckentyper hello: versaler, gemener, siffror och särskilda. |Lösenordet innehåller inte hello krävs typer av tecken. Kontrollera att lösenordet uppfyller dessa krav. |
| 7 |Parametern matchar inte bekräftelsen. |Kontrollera att lösenordet uppfyller alla krav och att du angett rätt. |
| 8 |Lösenordet får inte matcha hello standard. |hello standardlösenordet är *Password1*. Du måste toochange lösenordet när du loggar in för hello första gången. |
| 9 |hello-lösenord som du har angett matchar inte hello enhetens lösenord. Skriv hello lösenordet igen. |Kontrollera hello lösenord och ange den igen. |

Lösenord har samlats in innan hello enheten är registrerad, men tillämpas endast efter att registreringen har lyckats. hello lösenord återställningsarbetsflöde kräver hello enheten toobe registrerad. 

> [!IMPORTANT]
> I allmänhet om ett försök tooapply misslyckas lösenord, och sedan hello programvara flera gånger försöker toocollect hello lösenord tills den lyckas. I sällsynta fall kan inte hello lösenord användas. I så fall kan du registrera hello enhet och fortsätter, men hello lösenord ändras inte. Får du några tecken som toowhich lösenord inte har ändrats, hello enhetens administratörslösenord eller hello lösenordet för StorSimple Snapshot Manager. Om detta inträffar rekommenderar vi att du ändrar båda lösenorden.
> 
> 

Du kan återställa hello lösenord i hello klassiska Azure-portalen via hello StorSimple Manager-tjänsten. Mer information finns: 

* [Ändra hello enhetens administratörslösenord](storsimple-change-passwords.md#change-the-device-administrator-password).
* [Ändra hello StorSimple Snapshot Manager lösenord](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Fel vid enhetsregistrering
Du kan använda hello StorSimple Manager-tjänsten körs i Microsoft Azure tooregister hello enhet. Du kan stöta på ett eller flera av följande problem vid enhetsregistrering hello.

| Nej. | Felmeddelande | Möjliga orsaker | Rekommenderad åtgärd |
| --- | --- | --- | --- |
| 1 |Fel 350027: Misslyckades tooregister hello enheten med hello StorSimple Manager. | |Vänta några minuter och försök sedan hello igen. Hello problemet kvarstår [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md). |
| 2 |Fel 350013: Ett fel har uppstått i registrering hello enhet. Detta kan bero på tooincorrect nyckel för tjänstregistrering. | |Registrera enheten hello igen med hello rätt nyckel för tjänstregistrering. Mer information finns i [hämta hello nyckel för tjänstregistrering.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 |Fel 350063: Autentisering tooStorSimple Manager-tjänsten angavs, men registreringen misslyckades. Försök hello igen efter en stund. |Det här felet indikerar att autentisering med ACS har passerat men hello registrera anropet toohello tjänsten misslyckades. Detta kan bero på ett tekniskt problem sporadiska nätverk. |Ange hello problemet kvarstår [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md). |
| 4 |Fel 350049: Det gick inte att nå hello-tjänsten under registreringen. |När hello anrop toohello service, tas ett webbundantag emot. I vissa fall kan få detta fast med du försöker hello igen senare. |Kontrollera IP-adress och DNS-namn och försök sedan hello igen. Om hello problemet kvarstår [kontaktar Microsoft Support.](storsimple-contact-microsoft-support.md) |
| 5 |Fel 350031: hello enheten har redan registrerats. | |Ingen åtgärd krävs. |
| 6 |Fel 350016: Det gick inte att registrering av enheten. | |Kontrollera att hello registreringsnyckel är korrekt. |
| 7 |Anropa HcsSetupWizard: Ett fel uppstod vid registrering av enheten; Detta kan bero på tooincorrect IP-adress eller DNS-namn. Kontrollera nätverksinställningarna och försök igen. Om hello problemet kvarstår [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md). (Fel 350050) |Se till att enheten kan pinga hello utanför nätverket. Om du inte har anslutning toooutside nätverket misslyckas hello registrering med det här felet. Det här felet kan vara en kombination av en eller flera av följande hello:<ul><li>Felaktigt IP</li><li>Felaktig undernät</li><li>Felaktig gateway</li><li>Felaktiga DNS-inställningar</li></ul> |Se hello stegen i hello [stegvis Felsökningsexempel](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Anropa HcsSetupWizard: aktuella hello-åtgärden misslyckades på grund av tooan internt tjänstfel [0x1FBE2]. Gör hello åtgärden igen senare. Kontakta Microsoft Support om hello problemet kvarstår. |Detta är ett allmänt fel uppstod för alla användare osynliga fel från tjänsten eller agent. hello vanligaste orsaken kan vara att hello ACS-autentiseringen misslyckades. En möjlig orsak till hello felet är att det finns problem med hello NTP-serverkonfiguration och tiden på hello enheten är inte korrekt. |Korrekt hello tid (om det finns problem) och försök sedan hello registrering igen. Om du använder hello Set HcsSystem - tidszonen kommandot tooadjust hello tidszon versal varje ord i hello tidszon (till exempel ”Pacific Standard Time”).  Om problemet kvarstår [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. |
| 9 |Varning: Det gick inte att aktivera hello enhet. Enhetsadministratören och StorSimple Snapshot Manager lösenord har inte ändrats. |Om hello registreringen misslyckas ändras inte hello enhetsadministratör och StorSimple Snapshot Manager lösenord. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Verktyg för felsökning av StorSimple-distributioner
StorSimple innehåller flera verktyg som du kan använda tootroubleshoot StorSimple-lösningen. Exempel på dessa är:

* Stöd för paket och enhetsloggar 
* Cmdlets som utformats speciellt för felsökning 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Stöd för paket och tillgängliga enhetsloggar för felsökning
Ett supportpaket innehåller alla relevanta hello-loggar som kan underlätta hello Microsoft Support-teamet med felsökning av enhetsproblem med. Du kan använda Windows PowerShell för StorSimple toogenerate ett stöd för krypterade paket som du sedan kan dela med supporttekniker.

### <a name="tooview-hello-logs-or-hello-contents-of-hello-support-package"></a>tooview hello loggar eller hello innehållet i hello support-paket
1. Använda Windows PowerShell för StorSimple toogenerate ett supportpaket enligt beskrivningen i [skapa och hantera ett stödpaket](storsimple-create-manage-support-package.md).
2. Hämta hello [dekryptering skriptet](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalt på klientdatorn.
3. Använd den här [stegvisa anvisningar](storsimple-create-manage-support-package.md#edit-a-support-package) tooopen och dekryptera hello supportpaket.
4. hello dekryptera supportpaket loggar är etw/etvx format. Du kan utföra följande steg tooview hello filerna i Windows Loggboken:
   
   1. Kör hello **eventvwr** på Windows-klienter. Detta startar hello Loggboken.
   2. I hello **åtgärder** rutan klickar du på **Öppna sparad loggfil** och punkt toohello loggfiler i etvx/etw-format (hello support-paket). Du kan nu visa hello-filen. När du har öppnat filen hello du högerklicka på och spara hello filen som text.
      
      > [!IMPORTANT]
      > Du kan också använda hello **Get-WinEvent** cmdlet tooopen filerna i Windows PowerShell. Mer information finns i [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) i hello Windows PowerShell-cmdlet-referensdokumentationen.
      > 
      > 
5. När det öppnar hello loggarna i Loggboken, leta efter hello följande loggar som innehåller konfiguration för problem relaterade toohello enhet:
   
   * hcs_pfconfig/Operational logg
   * hcs_pfconfig/Config
6. Sök efter strängar i hello loggfiler relaterade toohello cmdletar som anropas av hello installationsguiden. Se [installationsguiden för första gången](#first-time-setup-wizard-process) för en lista över dessa cmdlets. 
7. Om du inte kan toofigure ut hello orsaken hello problemet kan du [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. Använd hello stegen i [skapa en supportbegäran](storsimple-contact-microsoft-support.md#create-a-support-request) när du kontaktar Microsoft Support för hjälp.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlet: ar tillgängliga för felsökning
Använd följande Windows PowerShell-cmdlets toodetect anslutningsfel hello.

* `Get-NetAdapter`: Använd den här cmdlet toodetect hello hälsotillståndet för nätverksgränssnitt. 
* `Test-Connection`: Använd den här cmdlet toocheck hello nätverksanslutning i och utanför hello nätverk.
* `Test-HcsmConnection`: Använd den här cmdlet toocheck hello anslutningen för en enhet som har registrerats.

Om du kör uppdatering 1 på StorSimple-enheten finns också hello följande diagnostiska cmdlets.

* `Sync-HcsTime`: Använd cmdlet toodisplay enheten nu och framtvinga tidssynkronisering med hello NTP-server.
* `Enable-HcsPing`och `Disable-HcsPing`: använda dessa cmdletar tooallow hello värdar tooping hello nätverksgränssnitt på din StorSimple-enhet. Som standard svarar inte hello StorSimple nätverksgränssnitt tooping begäranden.
* `Trace-HcsRoute`: Använd den här cmdleten som en route spårning för. Den skickar paket tooeach routern hello sätt tooa slutdestinationen under en viss tidsperiod och beräknar sedan resultatet baserat på hälsningspaket som returneras från varje hopp. Eftersom `Trace-HcsRoute` visar hello graden av paketförlust vid varje router eller en länk som du kan identifiera vilka routrar eller länkar som orsakar problem med nätverket. 
* `Get-HcsRoutingTable`: Använd den här cmdlet toodisplay hello lokala routningstabellen.

## <a name="troubleshoot-with-hello-get-netadapter-cmdlet"></a>Felsöka med hello Get-NetAdapter cmdlet
När du konfigurerar nätverksgränssnitt för en distribution för första gången enheten är hello maskinvarustatus inte tillgänglig i hello StorSimple Manager-tjänsten UI eftersom hello enhet ännu inte har registrerats med hello-tjänsten. Dessutom kanske hello maskinvarustatus sidan inte alltid korrekt återspeglar hello tillståndet för hello enheten, särskilt om det finns problem som påverkar Tjänstsynkronisering. I sådana fall måste du använda hello `Get-NetAdapter` cmdlet toodetermine hello hälsa och status för din nätverksgränssnitt.

### <a name="toosee-a-list-of-all-hello-network-adapters-on-your-device"></a>toosee en lista över alla hello nätverkskort på enheten
1. Starta Windows PowerShell för StorSimple och skriv sedan `Get-NetAdapter`. 
2. Använd hello utdata från hello `Get-NetAdapter` cmdlet och hello följande riktlinjer toounderstand hello status för din nätverksgränssnitt.
   
   * Om hello-gränssnittet är felfri och aktiverad, hello **ifIndex** status visas som **in**.
   * Om hello-gränssnittet är felfri men är inte anslutna (med en nätverkskabel), hello **ifIndex** visas som **inaktiverade**.
   * Om hello-gränssnittet är felfri men är inte aktiverad, hello **ifIndex** status visas som **NotPresent**.
   * Om hello-gränssnittet inte finns, visas den inte i den här listan. Hej StorSimple Manager-tjänsten UI fortfarande visas det här gränssnittet i ett felaktigt tillstånd.

Mer information om hur toouse denna cmdlet, gå för[GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) i hello Windows PowerShell-cmdlet-referens. 

hello följande avsnitt visar exempel på utdata från hello `Get-NetAdapter` cmdlet. 

 I de här exemplen styrenhet 0 är hello passiva domänkontrollant och har konfigurerats på följande sätt:

* DATA 0, DATA 1, DATA 2 och DATA 3 nätverk gränssnitt som fanns på hello enhet.
* DATA 4 och 5 DATA nätverkskort inte finns några; Därför kan visas de inte i hello utdata.
* DATA 0 har aktiverats.

Kontrollant 1 var hello aktiv domänkontrollant och har konfigurerats på följande sätt:

* DATA 0, DATA 1, DATA 2, DATA 3, 4 DATA och DATA 5 nätverk gränssnitt som fanns på hello enhet.
* DATA 0 har aktiverats.

**Exempel på utdata-styrenhet 0**

hello följer hello utdata från styrenhet 0 (hello passiva styrenhet). DATA 1, DATA 2 och DATA 3 är inte ansluten. DATA 4 och 5 DATA visas inte eftersom de inte finns på hello enhet. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Exempel på utdata – kontrollant 1**

hello följer hello utdata från kontrollant 1 (hello aktiva styrenhet). Endast hello DATA 0 nätverkskort på hello enhet har konfigurerats och fungerar.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent


## <a name="troubleshoot-with-hello-test-connection-cmdlet"></a>Felsöka med hello Test-Connection cmdlet
Du kan använda hello `Test-Connection` cmdlet toodetermine om din StorSimple-enhet kan ansluta toohello utanför nätverket. Om alla hello nätverk parametrar, inklusive hello DNS är korrekt konfigurerade i hello installationsguiden kan du använda hello `Test-Connection` cmdlet tooping en känd adress utanför hello nätverk, till exempel outlook.com. 

Du bör aktivera ping tootroubleshoot anslutningsproblem med denna cmdlet om ping har inaktiverats.

Se följande exempel på utdata från hello hello `Test-Connection` cmdlet. 

> [!NOTE]
> I hello första exemplet är hello enheten konfigurerad med en felaktig DNS. I andra exemplet hello är hello DNS korrekt.
> 
> 

**Exempel på utdata – felaktig DNS**

I följande exempel hello, finns inga utdata för hello IPV4 och IPV6-adresser, vilket indikerar att hello DNS inte är löst. Det innebär att det finns ingen anslutning toohello utanför nätverket och en korrekt DNS måste toobe angavs. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Exempel på utdata – rätt DNS**

I följande exempel hello, hello hello DNS returnerar IPV4-adress, som anger att hello DNS är korrekt konfigurerad. Det här bekräftar att det finns anslutning toohello utanför nätverket. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-hello-test-hcsmconnection-cmdlet"></a>Felsöka med hello Test-HcsmConnection cmdlet
Använd hello `Test-HcsmConnection` cmdlet för en enhet som redan är ansluten tooand som registrerats med StorSimple Manager-tjänsten. Denna cmdlet hjälper dig att kontrollera hello anslutningen mellan en registrerad enhet och hello motsvarande StorSimple Manager-tjänsten. Du kan köra det här kommandot på Windows PowerShell för StorSimple. 

### <a name="toorun-hello-test-hcsmconnection-cmdlet"></a>toorun hello Test-HcsmConnection cmdlet
1. Kontrollera att hello-enheten är registrerad.
2. Kontrollera status för hello-enheter. Om hello enheten inaktiveras i underhållsläge eller offline, kan du se hello följande fel: 
   
   * ErrorCode.CiSDeviceDecommissioned – detta anger hello enheten har inaktiverats.
   * ErrorCode.DeviceNotReady – detta anger att hello-enheten är i underhållsläge.
   * ErrorCode.DeviceNotReady – detta anger att hello-enheten inte är online.
3. Kontrollera att hello StorSimple Manager-tjänsten körs (Använd hello [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet). Om hello-tjänsten inte körs, kan du se hello följande fel:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – detta anger att ett undantag uppstod när du körde Get-ClusterResource.
4. Kontrollera hello Access Control Service (ACS) token. Om det genererar ett webbundantag kanske hello resultatet av ett gateway-problem, saknas proxyautentisering, en felaktig DNS- eller ett autentiseringsfel. Du kan se hello följande fel:
   
   * ErrorCode.CiSApplianceGateway – detta anger ett HttpStatusCode.BadGateway undantag: hello namn lösartjänsten gick inte att matcha hello värdnamn. 
   * ErrorCode.CiSApplianceProxy – detta anger ett HttpStatusCode.ProxyAuthenticationRequired undantag (HTTP-statuskod 407): hello-klienten kunde inte autentisera med hello proxyserver. 
   * ErrorCode.CiSApplianceDNSError – detta anger ett WebExceptionStatus.NameResolutionFailure undantag: hello namn lösartjänsten gick inte att matcha hello värdnamn.
   * ErrorCode.CiSApplianceACSError – detta anger att hello tjänsten returnerade ett autentiseringsfel, men det finns en anslutning.
     
     Om det inget genereras ett webbundantag, kontrollera om ErrorCode.CiSApplianceFailure. Detta anger att hello installation misslyckades.
5. Kontrollera hello cloud service-anslutning. Om tjänsten hello genererar ett webbundantag, kan du se hello följande fel:
   
   * ErrorCode.CiSApplianceGateway – detta anger ett HttpStatusCode.BadGateway undantag: en mellanliggande proxyserver tog emot en felaktig begäran från en annan proxy eller hello ursprungliga servern.
   * ErrorCode.CiSApplianceProxy – detta anger ett HttpStatusCode.ProxyAuthenticationRequired undantag (HTTP-statuskod 407): hello-klienten kunde inte autentisera med hello proxyserver. 
   * ErrorCode.CiSApplianceDNSError – detta anger ett WebExceptionStatus.NameResolutionFailure undantag: hello namn lösartjänsten gick inte att matcha hello värdnamn.
   * ErrorCode.CiSApplianceACSError – detta anger att hello tjänsten returnerade ett autentiseringsfel, men det finns en anslutning.
     
     Om det inget genereras ett webbundantag, kontrollera om ErrorCode.CiSApplianceSaasServiceError. Det här tyder på problem med hello StorSimple Manager-tjänsten.
6. Kontrollera Azure Service Bus-anslutningen. ErrorCode.CiSApplianceServiceBusError anger hello enheten inte kan ansluta toohello Service Bus.

Hej loggfiler CiSCommandletLog0Curr.errlog och CiSAgentsvc0Curr.errlog har mer information, till exempel undantagsinformation. 

Mer information om hur toouse hello cmdlet gå för[Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) refererar dokumentationen i hello Windows PowerShell.

> [!IMPORTANT]
> Du kan köra denna cmdlet för både hello aktiva och passiva hello-styrenhet. 
> 
> 

Se följande exempel på utdata från hello hello `Test-HcsmConnection` cmdlet. 

**Exempel på utdata – registrerades enhet som kör StorSimple Release (juli 2014)**

hello det första exemplet är från en enhet som har registrerats med hello StorSimple Manager-tjänst och har inga problem med nätverksanslutningen. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes toocomplete....
     Checking device authentication.  ... Success.
     Checking connectivity from device tooStorSimple Manager service.  ... This operation will take few minutes toocomplete....
     Checking connectivity from device tooStorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service tooStorSimple device. .... Success.
     Controller1>

**Exempel på utdata – registrerades enhet som kör StorSimple uppdatering 1**

Om du kör uppdatering 1 på din StorSimple-enhet behöver du inte toorun med hello utförlig växel.

      Controller1>Test-HcsmConnection

      Checking device registration state  ... Success
      Device registered successfully

      Checking primary NTP server [time.windows.com] ... Success

      Checking web proxy  ... NOT SET

      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET

      Checking device online  ... Success

      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success

      Checking connectivity from device tooservice  ... This will take a few minutes.

      Checking connectivity from device tooservice  ... Success

      Checking connectivity from service toodevice  ... Success

      Checking connectivity tooMicrosoft Update servers  ... Success
      Controller1>

**Exempel på utdata – offline enhet som kör StorSimple Release (juli 2014)**

Det här exemplet är från en enhet som har statusen **Offline** i hello klassiska Azure-portalen.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device tooSaaS.. Failure

hello enheten kunde inte ansluta med hello aktuella webbproxykonfigurationen. Detta kan bero på ett problem med hello webbproxykonfigurationen eller ett problem med nätverksanslutningen. I det här fallet bör du se till att din webbproxyinställningarna är korrekta och webbproxyservrarna är online och kan nås. 

## <a name="troubleshoot-with-hello-sync-hcstime-cmdlet"></a>Felsöka med hello Sync-HcsTime cmdlet
Använd cmdlet toodisplay hello enheten nu. Om hello tid har en förskjutning med hello NTP-server, kan du använda den här cmdlet-tooforce-synkronisera hello tid med NTP-server. Om hello förskjutningen mellan hello enheten och NTP-server är större än 5 minuter, visas en varning. Om hello förskjutningen överskrider 15 minuter, kommer hello enheten går offline. Du kan fortfarande använda den här cmdlet tooforce tidssynkronisering. Dock hello förskjutningen överskrider 15 timmar, sedan kommer du inte att kunna tooforce-hello synkroniseringstid och ett felmeddelande visas.

**Exempel på utdata – framtvingad tidssynkronisering med synkronisering HcsTime**

     Controller0>Sync-HcsTime
     hello current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want tooresync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-hello-enable-hcsping-and-disable-hcsping-cmdlets"></a>Felsöka med hello HcsPing aktivera och inaktivera HcsPing-cmdlets
Använd dessa cmdlets tooensure att hello nätverksgränssnitt på enheten svarar tooICMP ping-begäranden. Som standard svarar inte hello StorSimple nätverksgränssnitt tooping begäranden. Använder denna cmdlet är hello enklaste sättet tooknow om enheten är online och kan nås.  

**Exempel på utdata – HcsPing aktivera och inaktivera HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-hello-trace-hcsroute-cmdlet"></a>Felsöka med hello Trace HcsRoute cmdlet
Använd denna cmdlet som ett verktyg för spårning av vägen. Den skickar paket tooeach routern hello sätt tooa slutdestinationen under en viss tidsperiod och beräknar sedan resultatet baserat på hälsningspaket som returneras från varje hopp. Eftersom hello cmdlet visar hello graden av paketförlust vid varje router eller länk, kan du identifiera vilka routrar eller länkar som orsakar problem med nätverket.

**Exempel på utdata visar hur tootrace hello flödet för ett paket med Trace-HcsRoute**

     Controller0>Trace-HcsRoute -Target 10.126.174.25

     Tracing route toocontoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]

     Computing statistics for 25 seconds...
                 Source tooHere   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]

     Trace complete.

## <a name="troubleshoot-with-hello-get-hcsroutingtable-cmdlet"></a>Felsöka med hello Get-HcsRoutingTable cmdlet
Använd denna cmdlet tooview hello-routningstabell för din StorSimple-enhet. En routningstabell är en uppsättning regler som kan hjälpa dig att avgöra var datapaket som skickas över ett nätverk för IP (Internet Protocol) dirigeras. 

hello routningstabellen visar hello gränssnitt och hello gateway att vägar hello data toohello angivna nätverk. Det ger även hello routning mått som är hello beslutsfattare för hello sökvägen tooreach en viss destination. hello lägre hello routning mått, hello högre hello prioritet. 

Till exempel om du har 2 nätverksgränssnitten DATA 2 och DATA 3 anslutna toohello Internet. Om hello routning mätvärden för DATA 2 och DATA 3 är 15 och 261, är DATA 2 med hello lägre routning mått för hello prioriterade gränssnittet används tooreach hello Internet.

Om du kör uppdatering 1 på StorSimple-enheten har DATA 0-nätverksgränssnittet hello högsta inställning för hello molnet trafik. Detta innebär att även om det finns andra moln-aktiverat gränssnitt, hello molntrafiken ska skickas via DATA 0. 

Om du kör hello `Get-HcsRoutingTable` cmdlet utan att ange eventuella parametrar (som hello som följande exempel visar IT-avdelning), hello cmdlet kommer utdata routningstabeller för både IPv4 och IPv6. Du kan också ange `Get-HcsRoutingTable -IPv4` eller `Get-HcsRoutingTable -IPv6` tooget relevanta routningstabellen.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================

      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================

      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None

      Controller0>

## <a name="step-by-step-storsimple-troubleshooting-example"></a>Stegvis Felsökningsexempel för StorSimple
hello följande exempel visar stegvisa felsökning av en StorSimple-distribution. I exemplet hello misslyckas enhetsregistrering med ett felmeddelande om att hello nätverksinställningar eller hello DNS-namn är felaktigt.

hello felmeddelande är:

     Invoke-HcsSetupWizard: An error has occurred while registering hello device. This could be due tooincorrect IP address or DNS name. Please check your network settings and try again. If hello problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

hello fel kan orsakas av något av följande hello:

* Installation av felaktig maskinvara
* Felaktig nätverksgränssnitt
* Felaktigt IP-adress, nätmask, gateway, primär DNS-server eller webbproxy
* Felaktig registreringsnyckel
* Felaktiga brandväggsinställningar

### <a name="toolocate-and-fix-hello-device-registration-problem"></a>toolocate och fixa hello registrering enhetsproblem
1. Kontrollera enhetskonfigurationen av: kör på hello aktiva styrenheten, `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > hello installationsguiden måste köras på hello aktiva styrenhet. tooverify som du är ansluten toohello aktiva styrenheten, titta på hello banderoll visas i hello seriekonsol. hello banderoll anger om du är ansluten toocontroller 0 eller 1-styrenheten och om hello domänkontrollant är aktiva eller passiva. Mer information finns för[identifiera en domänkontrollant för en aktiv på enheten](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
   > 
   > 
2. Kontrollera att enheten hello är korrekt kabelansluten: Kontrollera hello nätverket kablar på hello enhet tillbaka plana. hello kablar är särskilda toohello enhetsmodell. Mer information finns för[installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md) eller [installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > Om du använder 10 GbE-nätverksportar behöver toouse hello QSFP SFP-kort och SFP-kablar. Mer information finns i hello [lista över kablar och växlar mottagarna rekommenderas för hello 10 GbE portar](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
   > 
   > 
3. Kontrollera hello hälsa för gränssnitt hello:
   
   * Använd hello Get-NetAdapter cmdlet toodetect hello hälsotillstånd hello nätverksgränssnitt för DATA 0. 
   * Om hello länken inte fungerar, hello **ifindex** status visar hello gränssnittet är nere. Du måste sedan toocheck hello nätverksanslutning hello port toohello installation och toohello växel. Du måste också toorule ut felaktiga kablar. 
   * Om du misstänker att hello DATA 0-port på hello aktiva styrenheten misslyckades, du kan kontrollera detta genom att ansluta toohello DATA 0-port på en domänkontrollant 1. tooconfirm, koppla hello nätverkskabeln från hello baksidan hello enhet från styrenhet 0 ansluta hello kabel toocontroller 1 och kör hello Get-NetAdapter cmdleten igen. 
     Om hello DATA 0 port på en domänkontrollant misslyckas [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. Du kan behöva tooreplace hello domänkontrollant på datorn.
4. Kontrollera hello anslutningen toohello växel:
   
   * Se till att DATA 0 nätverksgränssnitt på styrenhet 0 och kontrollant 1 i din primära hölje är på hello samma undernät. 
   * Kontrollera hello nav eller en router. Normalt bör du ansluta båda domänkontrollanterna toohello samma nav eller en router. 
   * Se till att hello växlar du använder för hello har DATA 0 för både domänkontrollanter i hello samma vLAN.
5. Ta bort användarfel:
   
   * Kör installationsguiden för hello igen (kör **Invoke-HcsSetupWizard**), och ange hello värden igen toomake att det inte finns några fel. 
   * Verifiera registrering hello nyckel som används. Hej samma registreringsnyckel kan vara används tooconnect flera enheter tooa StorSimple Manager-tjänsten. Använd hello procedur i [hämta hello nyckel för tjänstregistrering](storsimple-manage-service.md#get-the-service-registration-key) tooensure som du använder hello rätt registreringsnyckel.
     
     > [!IMPORTANT]
     > Om du har flera tjänster som körs, måste tooensure hello Registreringsnyckeln för hello lämplig tjänst är används tooregister hello enhet. Om du har registrerat en enhet med hello fel StorSimple Manager-tjänsten, behöver du för[kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. Du kan ha tooperform en fabriksåterställning av hello-enhet (vilket kan resultera i dataförlust) toothen ansluta den toohello avsedd service.
     > 
     > 
6. Använd hello Test-Connection cmdlet tooverify att du har anslutning toohello utanför nätverket. Mer information finns för[felsökning med hello Test-Connection cmdlet](#troubleshoot-with-the-test-connection-cmdlet).
7. Kontrollera brandväggen störningar. Om du har verifierat att hello virtuella IP-adresser (VIP), undernät, gateway och DNS-inställningarna är korrekta, och du fortfarande se anslutningsproblem sedan är det möjligt att din brandvägg blockerar kommunikation mellan enheten och hello utanför nätverket. Du måste tooensure att portarna 80 och 443 är tillgängliga på din StorSimple-enhet för utgående kommunikation. Mer information finns i [nätverkskrav för din StorSimple-enhet](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Titta på hello loggar. Gå för[stödja paket och tillgängliga enhetsloggar för felsökning](#support-packages-and-device-logs-available-for-troubleshooting).
9. Om hello föregående stegen inte löser problemet hello [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) om du behöver hjälp.

## <a name="next-steps"></a>Nästa steg
[Lär dig hur tootroubleshoot en operativa enhet](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
