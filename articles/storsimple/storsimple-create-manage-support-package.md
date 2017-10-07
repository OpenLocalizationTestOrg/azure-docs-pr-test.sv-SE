---
title: "aaaCreate ett supportpaket för StorSimple | Microsoft Docs"
description: "Lär dig hur toocreate, dekryptera och redigera ett supportpaket för din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 209aeee50e823fd2ca96ababd1d0cf3ea9cdad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a>Skapa och hantera ett stödpaket för StorSimple
## <a name="overview"></a>Översikt
Ett supportpaket för StorSimple är ett enkelt att använda som samlar in alla relevanta loggar tooassist Microsoft-supporten att felsöka eventuella problem för StorSimple-enhet används. hello är insamlade loggar krypterade och komprimerade.

Den här självstudiekursen innehåller stegvisa instruktioner toocreate och hantera hello supportpaket.

## <a name="create-and-upload-a-support-package-in-hello-azure-classic-portal"></a>Skapa och ladda upp ett supportpaket i hello klassiska Azure-portalen
Du kan skapa och ladda upp en stöd paketet toohello Microsoft Support webbplats via hello **Underhåll** sidan hello tjänsterna i hello klassiska Azure-portalen.

> [!NOTE]
> hello överför kräver en nyckel för support. Supportpersonalen bör ge den här tooyou i ett e-postmeddelande.
> 
> 

Ett paket med stöd för krypterade och komprimerade (CAB-fil) är skapas och överförda toohello supportwebbplats. hello supportteknikern kan sedan Hämta paketet från hello supportwebbplats för felsökning av problem med hello.

Utför följande steg i hello klassiska portal toocreate ett supportpaket hello.

#### <a name="toocreate-a-support-package-in-hello-azure-classic-portal"></a>toocreate ett supportpaket i hello klassiska Azure-portalen
1. Välj **enheter** > **Underhåll**.
2. I hello **stödpaketet** väljer **skapa och ladda upp supportpaket**.
3. I hello **skapa och ladda upp supportpaket** dialogrutan rutan, hello följande:
   
    ![Skapa för stödpaket](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * I hello **stöd nyckel** text Ange hello nyckel. Supportpersonalen Microsoft bör skicka den här nyckeln tooyou i e-post.
   * Välj hello kryssrutan tooprovide medgivande tooautomatically överför hello stöd paketet toohello Microsoft-supporten plats.
   * Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-create-manage-support-package/IC740895.png).

## <a name="manually-create-a-support-package"></a>Skapa ett supportpaket manuellt
I vissa fall måste toomanually skapa hello supportpaket via Windows PowerShell för StorSimple. Exempel:

* Om du behöver tooremove känslig information från loggen filer tidigare toosharing Microsoft support.
* Om du har problem med överföring hello-paket på grund av problem med tooconnectivity.

Du kan dela din manuellt genererad supportpaket Microsoft support via e-post. Utför följande steg toocreate ett supportpaket i Windows PowerShell för StorSimple hello.

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a>toocreate ett supportpaket i Windows PowerShell för StorSimple
1. toostart en Windows PowerShell-session som administratör på fjärrdatorn hello som har använt tooconnect tooyour StorSimple-enhet, ange hello följande kommando:
   
    `Start PowerShell`
2. Anslut toohello SSAdmin konsolen för din enhet i hello Windows PowerShell-sessionen:
   
   * Ange Kommandotolken hello:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * Ange administratörslösenord för enheten hello dialogrutan som öppnas. hello standardlösenordet är:
     
      `Password1`
     
      ![Dialogrutan för PowerShell-autentiseringsuppgifter](./media/storsimple-create-manage-support-package/IC740962.png)
   * Välj **OK**.
   * Ange Kommandotolken hello:
     
      `Enter-PSSession $MS`
3. Ange hello kommandona i hello-sessionen som öppnas.
   
   * För nätverksresurser som är lösenordsskyddad, ange:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       Du uppmanas för ett lösenord, en sökväg toohello delad nätverksmapp och krypteringslösenfrasen (eftersom hello supportpaket krypteras). Ett supportpaket skapas i hello angivna mappen.
   * För resurser som inte är lösenordsskyddad, behöver inte hello `-Credential` parameter. Ange hello följande:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       hello supportpaket skapas för både domänkontrollanter i hello angivna delad nätverksmapp. Det är en krypterad, komprimerad fil som kan skickas tooMicrosoft stöd för felsökning. Mer information finns i [kontakta Microsoft Support](storsimple-contact-microsoft-support.md).

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a>hello Export HcsSupportPackage cmdlet-parametrar
Du kan använda följande parametrar med hello Export HcsSupportPackage cmdlet hello.

| Parameter | Nödvändiga/valfria | Beskrivning |
| --- | --- | --- |
| `-Path` |Krävs |Använd tooprovide hello platsen för hello delad nätverksmapp där hello supportpaket är placerad. |
| `-EncryptionPassphrase` |Krävs |Använd tooprovide en lösenfras toohelp kryptera hello supportpaket. |
| `-Credential` |Valfri |Använd toosupply autentiseringsuppgifter för hello delad nätverksmapp. |
| `-Force` |Valfri |Använd tooskip hello kryptering lösenfras bekräftelsesteg. |
| `-PackageTag` |Valfri |Använda toospecify en katalog under *sökväg* vilka hello stöd paketet är placerad. hello standardvärdet är [enhetsnamn]-[aktuellt datum och time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Valfri |Ange som **klustret** (standard) toocreate ett supportpaket för både domänkontrollanter. Om du vill toocreate ett paket endast för hello aktuella controller anger **domänkontrollant**. |

## <a name="edit-a-support-package"></a>Redigera ett supportpaket
När du har genererat ett supportpaket kan behöva du tooedit hello paketet tooremove känslig information. Detta kan inkludera volymnamn, enhetens IP-adresser och säkerhetskopiering namn från hello loggfiler.

> [!IMPORTANT]
> Du kan bara redigera ett supportpaket som har genererats via Windows PowerShell för StorSimple. Du kan inte redigera ett paket som skapats i hello klassiska Azure-portalen med StorSimple Manager-tjänsten.
> 
> 

tooedit ett supportpaket innan du laddar upp den på hello Microsoft Support webbplats först dekryptera hello supportpaket, redigera hello filer och kryptera den igen. Utför följande steg hello.

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a>tooedit ett supportpaket i Windows PowerShell för StorSimple
1. Generera ett supportpaket som beskrivs ovan, [toocreate ett supportpaket i Windows PowerShell för StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Hämta hello skriptet](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalt på klientdatorerna.
3. Importera hello Windows PowerShell-modulen. Ange hello sökvägen toohello lokal mapp som du hämtade hello skript. tooimport hello modulen, ange:
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. Alla filer som hello *.aes* filer som är komprimerade och krypteras. toodecompress och dekryptera filer, ange:
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    Observera att hello faktiska filnamnstillägg nu visas för alla hello-filer.
   
    ![Redigera supportpaket](./media/storsimple-create-manage-support-package/IC750706.png)
5. När du uppmanas att ange hello krypteringslösenfrasen ange hello lösenfras som du använde när hello stödpaketet skapades.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. Bläddra toohello mapp som innehåller hello loggfiler. Eftersom hello loggfiler är nu packa upp och dekrypteras, kommer dessa har ursprungliga filnamnstillägg. Ändrar dessa filer tooremove kundspecifik information, till exempel volymnamn och enheten IP-adresser, och spara hello-filer.
7. Stäng hello filer toocompress dem med gzip och kryptera dem med AES 256. Detta är hastighet och säkerhet i överföra hello supportpaket över ett nätverk. toocompress och kryptera filer, ange hello följande:
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Redigera supportpaket](./media/storsimple-create-manage-support-package/IC750707.png)
8. När du uppmanas ange krypteringslösenfrasen för hello ändrade supportpaket.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. Skriv ned nya lösenfrasen som hello, så att du kan dela den med Microsoft-supporten när det behövs.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Exempel: Redigera filer i ett supportpaket på en lösenordsskyddad resurs
hello som följande exempel visar hur toodecrypt, redigera och kryptera ett supportpaket.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[Använd paket och enheten loggar tootroubleshoot distributionen enheten](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

