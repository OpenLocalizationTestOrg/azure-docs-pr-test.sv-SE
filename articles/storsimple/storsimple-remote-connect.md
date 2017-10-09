---
title: "aaaConnect via fjärranslutning tooyour StorSimple-enhet | Microsoft Docs"
description: "Förklarar hur tooconfigure enheten för fjärrhantering och hur tooconnect tooWindows PowerShell för StorSimple via HTTP eller HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 923377aa-f451-4656-87de-5e95a34a6a2a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 55ed8fcdd997901301e0adc164a302216cde0332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a>Fjärransluta tooyour StorSimple 8000-serieenhet

## <a name="overview"></a>Översikt
Du kan använda Windows PowerShell-fjärrkommunikation tooconnect tooyour StorSimple-enhet. När du ansluter det här sättet kan se du inte en meny. (Du se en meny endast om du använder hello seriekonsolen på hello enheten tooconnect.) Med Windows PowerShell-fjärrkommunikation ansluta tooa specifika runspace. Du kan också ange hello visningsspråket. 

Mer information om hur du använder Windows PowerShell-fjärrkommunikation toomanage enheten finns för[använda Windows PowerShell för StorSimple tooadminister StorSimple-enheten](storsimple-windows-powershell-administration.md).

Den här självstudiekursen beskrivs hur tooconfigure enheten för fjärrhantering och sedan hur tooconnect tooWindows PowerShell för StorSimple. Du kan använda HTTP eller HTTPS tooconnect via Windows PowerShell-fjärrkommunikation. Men när du beslutar hur tooconnect tooWindows PowerShell för StorSimple, Tänk hello följande: 

* Ansluta direkt toohello enhetens seriekonsol är säkra, men anslutande toohello seriekonsolen över nätverksväxlar är inte. Var försiktig av hello säkerhetsrisk när du ansluter toohello enhetens seriekonsol via nätverksväxlar. 
* Ansluter via en HTTP-session kan erbjuda mer säkerhet än att ansluta via seriekonsol hello hello nätverket. Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk. 
* Ansluter via en HTTPS-session med ett självsignerat certifikat är hello säkraste och hello rekommenderas.

Du kan fjärransluta toohello Windows PowerShell-gränssnittet. Fjärråtkomst tooyour StorSimple-enhet via hello Windows PowerShell-gränssnittet är inte aktiverad som standard. Du behöver tooenable fjärrhantering på hello enheten först och sedan på hello-klient som är används tooaccess din enhet.

hello stegen som beskrivs i den här artikeln utfördes på ett värdsystem som kör Windows Server 2012 R2.

## <a name="connect-through-http"></a>Ansluta via HTTP
Ansluter tooWindows PowerShell för StorSimple via en HTTP-session är säkrare än att ansluta via hello seriekonsol av StorSimple-enheten. Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk.

Du kan använda hello klassiska Azure-portalen eller hello seriekonsolen tooconfigure för fjärrhantering. Välj hello följande procedurer:

* [Använda hello Azure klassiska portal tooenable remote management via HTTP](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [Använda hello seriekonsolen tooenable remote management via HTTP](#use-the-serial-console-to-enable-remote-management-over-http)

När du aktiverar fjärrhantering, Använd hello följa proceduren tooprepare hello-klient för en fjärranslutning.

* [Förbered hello-klient för fjärranslutning](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-http"></a>Använda hello Azure klassiska portal tooenable remote management via HTTP
Utföra hello följa stegen i hello Azure klassiska portal tooenable fjärrhantering över HTTP.

#### <a name="tooenable-remote-management-through-hello-azure-classic-portal"></a>tooenable fjärrhantering via hello klassiska Azure-portalen
1. Åtkomst **enheter** > **konfigurera** för din enhet.
2. Bläddra nedåt toohello **fjärrhantering** avsnitt.
3. Ange **aktivera fjärrhantering** för**Ja**.
4. Nu kan du välja tooconnect med HTTP. (hello är standard tooconnect via HTTPS.) Se till att HTTP har valts.
   
   > [!NOTE]
   > Det är bara acceptabelt att ansluta över HTTP på betrodda nätverk.
   > 
   > 
5. Klicka på **spara** på hello hello sidans nederkant.

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a>Använda hello seriekonsolen tooenable remote management via HTTP
Utför följande hello seriekonsolen tooenable remote enhetshantering hello.

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a>tooenable fjärrhantering via hello enhetens seriekonsol
1. På menyn för seriekonsolen av hello väljer du alternativ 1. Mer information om hur du använder hello seriekonsolen på hello enhet gå för[ansluta tooWindows PowerShell för StorSimple via enhetens seriekonsol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. I hello kommandotolk, skriver du:`Enable-HcsRemoteManagement –AllowHttp`
3. Du meddelas om hello säkerhetsrisker med att använda HTTP tooconnect toohello enhet. När du uppmanas bekräfta genom att skriva **Y**.
4. Kontrollera att HTTP har aktiverats genom att skriva:`Get-HcsSystem`
5. Kontrollera att hello **RemoteManagementMode** fältet visar **HttpsAndHttpEnabled**.hello följande bild visar de här inställningarna i PuTTY.
   
     ![Seriell HTTPS och HTTP-aktiverad](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a>Förbered hello-klient för fjärranslutning
Utför följande steg på klientens hello tooenable fjärrhantering hello.

#### <a name="tooprepare-hello-client-for-remote-connection"></a>tooprepare hello klienten för fjärranslutning
1. Starta en Windows PowerShell-session som administratör.
2. Skriv följande kommando tooadd hello IP-adress i listan över betrodda värdar hello StorSimple enheten toohello klientens hello: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Ersätt <*device_ip*> med hello IP-adressen för din enhet, till exempel: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Skriv följande kommando toosave hello enheten autentiseringsuppgifter i en variabel hello: 
   
    ```
    $cred = Get-Credential
    ```
    
4. Hello i dialogrutan som visas:
   
   1. Skriv hello användarnamn i formatet: *device_ip\SSAdmin*.
   2. Ange hello enhetens administratörslösenord som angavs när hello enhet har konfigurerats med hello installationsguiden. hello standardlösenordet är *Password1*.
5. Starta Windows PowerShell-sessionen hello enheten genom att skriva följande kommando:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > toocreate en Windows PowerShell-session för användning med hello virtuell StorSimple-enhet, Lägg till hello `–Port` parametern och ange hello offentlig port som du konfigurerade i fjärrkommunikation för virtuell StorSimple-enhet.
   > 
   > 
   
     Du bör nu ha en aktiv Windows PowerShell-session toohello fjärrenheten.
   
    ![PowerShell-fjärrkommunikation med HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Ansluta via HTTPS
Ansluter tooWindows PowerShell för StorSimple via en HTTPS-session är hello säkraste och rekommenderade metoden för enhet som ansluter via fjärranslutning tooyour Microsoft Azure StorSimple. hello följande procedurer beskrivs hur tooset in hello serial-konsolen och klienten datorer så att du kan använda HTTPS tooconnect tooWindows PowerShell för StorSimple.

Du kan använda hello klassiska Azure-portalen eller hello seriekonsolen tooconfigure för fjärrhantering. Välj hello följande procedurer:

* [Använda hello Azure klassiska portal tooenable remote management via HTTPS](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [Använda hello seriekonsolen tooenable remote management via HTTPS](#use-the-serial-console-to-enable-remote-management-over-https)

När du aktiverar fjärrhantering, Använd följande procedurer tooprepare hello värden för en fjärrhantering hello och ansluter toohello enheten från hello fjärrvärden.

* [Förbereda hello värden för fjärrhantering](#prepare-the-host-for-remote-management)
* [Anslut toohello enhet från hello fjärrvärden](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-https"></a>Använda hello Azure klassiska portal tooenable remote management via HTTPS
Utföra hello följa stegen i hello Azure klassiska portal tooenable fjärrhantering över HTTPS.

#### <a name="tooenable-remote-management-over-https-from-hello-azure-classic-portal"></a>tooenable remote management via HTTPS från hello klassiska Azure-portalen
1. Åtkomst **enheter** > **konfigurera** för din enhet.
2. Bläddra nedåt toohello **fjärrhantering** avsnitt.
3. Ange **aktivera fjärrhantering** för**Ja**.
4. Nu kan du välja tooconnect med hjälp av HTTPS. (hello är standard tooconnect via HTTPS.) Kontrollera att HTTPS är markerad. 
5. Klicka på **hämta certifikat för fjärrhantering**. Ange en plats toosave den här filen. Du behöver tooinstall certifikatet på hello klienten eller värddatorn dator som du ska använda tooconnect toohello enhet.
6. Klicka på **spara** på hello hello sidans nederkant.

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a>Använda hello seriekonsolen tooenable remote management via HTTPS
Utför följande hello seriekonsolen tooenable remote enhetshantering hello.

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a>tooenable fjärrhantering via hello enhetens seriekonsol
1. På menyn för seriekonsolen av hello väljer du alternativ 1. Mer information om hur du använder hello seriekonsolen på hello enhet gå för[ansluta tooWindows PowerShell för StorSimple via enhetens seriekonsol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. I hello kommandotolk, skriver du: 
   
     `Enable-HcsRemoteManagement`
   
    Detta bör aktivera HTTPS på enheten.
3. Kontrollera att HTTPS har aktiverats genom att skriva: 
   
     `Get-HcsSystem`
   
    Kontrollera att hello **RemoteManagementMode** fältet visar **HttpsEnabled**.hello följande bild visar de här inställningarna i PuTTY.
   
     ![Seriell HTTPS-aktiverade](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Från hello utdata från `Get-HcsSystem`, kopiera hello serienumret för hello enheten och spara den för senare användning.
   
   > [!NOTE]
   > hello serienummer mappar toohello CN-namn i hello certifikat.
   > 
   > 
5. Hämta ett certifikat för fjärrhantering genom att skriva: 
   
     `Get-HcsRemoteManagementCert`
   
    Ett certifikat liknande toohello följande visas.
   
    ![Hämta certifikat för fjärrhantering](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Kopiera hello i hello certifikat från **---BEGIN CERTIFICATE---** för**---END CERTIFICATE---** i en textredigerare, till exempel Anteckningar och spara den som en .cer-fil. (Du kommer att kopiera den här filen tooyour fjärrvärden när du förbereder hello värden.)
   
   > [!NOTE]
   > toogenerate ett nytt certifikat använder hello `Set-HcsRemoteManagementCert` cmdlet.
   > 
   > 

### <a name="prepare-hello-host-for-remote-management"></a>Förbereda hello värden för fjärrhantering
tooprepare hello värddatorn för en anslutning som använder en HTTPS-session, utföra hello följande procedurer:

* [Importera hello .cer-fil i hello rotarkivet hello klienten eller fjärrvärden](#to-import-the-certificate-on-the-remote-host).
* [Lägg till hello enhetens serienummer toohello hosts-filen på din fjärrvärden](#to-add-device-serial-numbers-to-the-remote-host).

Var och en av de här procedurerna beskrivs nedan.

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a>tooimport hello certifikatet på fjärrvärden hello
1. Högerklicka på hello .cer-fil och välj **installera certifikat**. Detta startar hello guiden Importera certifikat.
   
    ![Guiden Importera 1 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. För **plats**väljer **lokal dator**, och klicka sedan på **nästa**.
3. Välj **placera alla certifikat i följande store hello**, och klicka sedan på **Bläddra**. Navigera toohello rotarkivet på din fjärrvärden och klicka sedan på **nästa**.
   
    ![Guiden Importera 2 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. Klicka på **Slutför**. Visas ett meddelande som talar om att hello importen lyckades.
   
    ![Guiden Importera 3 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a>tooadd enhetens serienummer toohello fjärrvärden
1. Starta Anteckningar som administratör och öppna hello värdfilen som finns på \Windows\System32\Drivers\etc.
2. Lägg till följande tre poster tooyour värdfilen hello: **DATA 0 IP-adress**, **fasta IP-adressen för styrenhet 0**, och **domänkontrollant 1 fasta IP-adressen**.
3. Ange hello enhetens serienummer som du sparade tidigare. Den här toohello IP-adressen som visas i följande bild hello. Lägg till för styrenhet 0 och 1 **Controller0** och **Controller1** hello slutet av hello serienummer (CN-namn).
   
    ![Lägger till CN-namnet toohosts fil](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Spara hello hosts-filen.

### <a name="connect-toohello-device-from-hello-remote-host"></a>Anslut toohello enhet från hello fjärrvärden
Använda Windows PowerShell och SSL tooenter en SSAdmin sessionen på din enhet från en fjärrvärd eller klienten. Hej SSAdmin session mappar toooption 1 i hello [seriekonsolen](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) -menyn i enheten.

Utföra hello följa proceduren på hello som du vill toomake hello Windows PowerShell fjärranslutning.

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a>tooenter en SSAdmin sessionen på hello-enhet med hjälp av Windows PowerShell och SSL
1. Starta en Windows PowerShell-session som administratör.
2. Lägg till hello enhetens IP-adress toohello klientens betrodda värdar genom att skriva:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Där <*device_ip*> är hello IP-adressen för din enhet, till exempel: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Skapa en ny autentiseringsuppgift genom att skriva: 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Där <*IP målenhet*> är hello IP-adressen för DATA 0 för din enhet, till exempel **10.126.173.90** enligt hello föregående bild av hello hosts-filen. Ange dessutom hello administratörslösenordet för enheten.
4. Skapa en session genom att skriva:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Ange hello hello - ComputerName parameter i cmdleten hello <*serienumret för målenhet*>. Det här serienumret mappades toohello IP-adress för DATA 0 i hello värdfilen på fjärrvärden; till exempel **SHX0991003G44MT** som visas i följande bild hello.
5. Ange: 
   
     `Enter-PSSession $session`
6. Du behöver toowait några minuter och du kommer sedan att anslutna tooyour enheten via HTTPS via SSL. Du ser ett meddelande som anger att du är ansluten tooyour enhet.
   
    ![PowerShell-fjärrkommunikation med hjälp av HTTPS och SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [med hjälp av Windows PowerShell tooadminister StorSimple-enheten](storsimple-windows-powershell-administration.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

