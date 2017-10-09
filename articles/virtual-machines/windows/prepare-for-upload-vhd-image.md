---
title: aaaPrepare en Windows-VHD tooupload tooAzure | Microsoft Docs
description: Hur tooprepare Windows VHD- eller VHDX innan du laddar upp tooAzure
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: genli
ms.openlocfilehash: 530390e4c6a4f66ddfd4da23338f9bb3708c299f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-windows-vhd-or-vhdx-tooupload-tooazure"></a>Förbereda en Windows-VHD eller VHDX tooupload tooAzure
Innan du laddar upp en Windows-datorer (VM) från lokala tooMicrosoft Azure måste du förbereda hello virtuell hårddisk (VHD eller VHDX). Azure stöder endast generering 1 virtuella datorer som är i hello VHD-format och har en fast storlek disk. Maximal storlek för hello tillåts för hello VHD 1,023 GB. Du kan konvertera en generation 1 virtuell dator från hello VHDX filen system tooVHD och från en dynamiskt expanderande disken toofixed storlek. Men du kan inte ändra en virtuell dator generation. Mer information finns i [bör jag skapa en generation 1 eller 2 virtuella datorn i Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Mer information om hello Supportpolicy för Azure VM finns [Microsoft server programvarusupport för Microsoft Azure Virtual Machines](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!Note]
> hello-instruktioner i den här artikeln gäller toohello 64-bitars version av Windows Server 2008 R2 och senare Windows server-operativsystem. Information om den körs 32-bitars version av operativsystemet i Azure finns [stöd för 32-bitars operativsystem i virtuella Azure-datorer](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-hello-virtual-disk-toovhd-and-fixed-size-disk"></a>Konvertera hello virtuell disk tooVHD och med fast storlek 
Om du behöver tooconvert krävs virtuell disk-toohello format för Azure, Använd en av hello-metoderna i det här avsnittet. Säkerhetskopiera hello VM innan du kör hello virtuell disk konverteringen och se till att hello Windows VHD fungerar korrekt på hello lokal server. Lös eventuella fel i hello Virtuella datorn innan du försöker tooconvert eller överför den tooAzure.

När du konverterar hello disk kan skapa en virtuell dator som använder hello konverteras disken. Starta och logga in toohello VM toofinish förbereder hello VM för överföring.

### <a name="convert-disk-using-hyper-v-manager"></a>Konvertera disk med hjälp av Hyper-V Manager
1. Öppna Hyper-V Manager och välj den lokala datorn hello vänster. Hello-menyn ovanför hello datorlistan **åtgärd** > **redigera Disk**.
2. På hello **hitta virtuell hårddisk** skärmen, leta upp och markera din virtuella hårddisk.
3. På hello **Välj åtgärd** skärmen och välj sedan **konvertera** och **nästa**.
4. Om du behöver tooconvert från VHDX, Välj **VHD** och klicka sedan på **nästa**
5. Om du behöver tooconvert från en dynamiskt expanderande disk markerar **fast storlek** och klicka sedan på **nästa**
6. Leta upp och välj en sökväg toosave hello nya VHD-filen till.
7. Klicka på **Slutför**.

>[!NOTE]
>hello kommandona i den här artikeln måste köras på en upphöjd PowerShell-session.

### <a name="convert-disk-by-using-powershell"></a>Konvertera disk med hjälp av PowerShell
Du kan konvertera en virtuell disk med hjälp av hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) i Windows PowerShell. Välj **kör som administratör** när du startar PowerShell. 

hello konverteras följande exempelkommando från VHDX tooVHD och från en dynamiskt expanderande toofixed-diskstorlek:

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
I det här kommandot ersätter du hello värde för ”-sökvägen” med hello sökvägen toohello virtuell hårddisk som du vill tooconvert och hello värde för ”-DestinationPath” med hello nya sökvägen och namnet på hello konverteras disken.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Konvertera från VMware VMDK diskformat
Om du har en Windows VM-avbildning i hello [VMDK-filformat](https://en.wikipedia.org/wiki/VMDK), konvertera tooa VHD med hjälp av hello [Microsoft VM Converter](https://www.microsoft.com/download/details.aspx?id=42497). Mer information finns i hello bloggartikel [hur tooConvert en VMware VMDK tooHyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Ange Windows-konfigurationer för Azure

På hello VM som du planerar tooupload tooAzure, köra alla kommandon i hello följande steg från ett [upphöjt kommandotolksfönster](https://technet.microsoft.com/library/cc947813.aspx):

1. Ta bort alla statiska beständig väg på hello routningstabellen:
   
   * tooview hello routningstabellen, kör `route print` hello Kommandotolken.
   * Kontrollera hello **beständiga vägar** avsnitt. Om en beständig väg använder [väg delete](https://technet.microsoft.com/library/cc739598.apx) tooremove den.
2. Ta bort hello WinHTTP-proxy:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```
3. Ange hello disk SAN-principen för[Onlineall](https://technet.microsoft.com/library/gg252636.aspx). 
   
    ```PowerShell
    diskpart 
    ```
    Skriv följande kommandon hello i hello öppna Kommandotolkens fönster:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Ange tid för Coordinated Universal Time (UTC) för Windows och hello hello (w32time) för Windows tidstjänst starttyp för**automatiskt**:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" 1 -Type DWord

    Set-Service -Name w32time -StartupType Auto
    ```
5. Ange hello power profil toohello **högpresterande**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```

## <a name="check-hello-windows-services"></a>Kontrollera hello Windows-tjänster
Kontrollera att var och en av följande Windows-tjänster hello anges toohello **Windows standardvärdena**. Dessa är hello minsta antal tjänster som måste ställas in toomake till att hello VM är ansluten till. tooreset hello inställningar för Start, kör hello följande kommandon:
   
```PowerShell
Set-Service -Name bfe -StartupType Auto
Set-Service -Name dhcp -StartupType Auto
Set-Service -Name dnscache -StartupType Auto
Set-Service -Name IKEEXT -StartupType Auto
Set-Service -Name iphlpsvc -StartupType Auto
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Auto
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Auto
Set-Service -Name RemoteRegistry -StartupType Auto
```

## <a name="update-remote-desktop-registry-settings"></a>Uppdatera registerinställningarna för fjärrskrivbord
Kontrollera att hello följande inställningar har konfigurerats korrekt för anslutning till fjärrskrivbord:

>[!Note] 
>Du får ett felmeddelande när du kör hello **Set-ItemProperty-sökvägen ' HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal tjänster - namnet &lt;objektnamn&gt; &lt;värdet&gt;** i följande steg. hello felmeddelande kan ignoreras. Det innebär att endast hello domänen inte är överföra den konfigurationen med hjälp av ett grupprincipobjekt.
>
>

1. Remote Desktop Protocol (RDP) är aktiverat:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord
    ```
   
2. hello RDP-porten är korrekt konfigurerad (standard port 3389):
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" 3389 -Type DWord
    ```
    När du distribuerar en virtuell dator skapas hello standardregler mot port 3389. Om du vill toochange hello portnummer, att göra det när hello VM distribueras i Azure.

3. hello-lyssnare lyssnar på varje gränssnitt:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" 0 -Type DWord
   ```
4. Konfigurera hello autentisering på nätverksnivå läge för hello RDP-anslutningar:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" 1 -Type DWord
     ```

5. Ange hello keep alive-värde:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" 1 -Type DWord
    ```
6. Återansluta:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" 0 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" 0 -Type DWord
    ```
7. Begränsa hello antalet samtidiga anslutningar:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" 4294967295 -Type DWord
    ```
8. Om det inte finns några självsignerade certifikat knutna toohello RDP-lyssnare, ta bort dem:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash"
    ```
    Detta är toomake till att du kan ansluta hello början när du distribuerar hello VM. Också kan granska på senare efter hello VM distribueras i Azure om det behövs.

9. Om hello VM ska ingå i en domän, markerar du alla hello följande inställningar toomake att inte återställs hello tidigare inställningar. hello-principer som måste vara markerad är hello följande:
    
    - RDP aktiveras:

         Datorn Datorkonfiguration\Principer\Windows Settings\Administrative Templates\ komponenter\Fjärrskrivbordstjänster\Värdserver för fjärrskrivbordssession\Anslutningar:
         
         **Tillåt användare tooconnect via fjärranslutning med hjälp av fjärrskrivbord**

    - NLA Grupprincip:

        Settings\Administrative Templates\Components\Remote för fjärrskrivbord fjärrskrivbordssession\Säkerhet: 
        
        **Kräv användare autentisering för fjärranslutningar genom att använda autentisering på nätverksnivå**
    
    - Behålla Alive-inställningar:

        Datorn Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för fjärrskrivbordssession\Anslutningar: 
        
        **Konfigurera intervall för keep-alive-anslutningen**

    - Återansluta inställningar:

        Datorn Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för fjärrskrivbordssession\Anslutningar: 
        
        **Automatisk återanslutning**

    - Begränsa hello antalet inställningar:

        Datorn Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för fjärrskrivbordssession\Anslutningar: 
        
        **Begränsa antalet anslutningar**

## <a name="configure-windows-firewall-rules"></a>Konfigurera regler för Windows-brandväggen
1. Aktivera Windows-brandväggen på hello tre profiler (domän, Standard och offentlig):

   ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 1 -Type DWord
   ```

2. Kör följande kommando i PowerShell tooallow WinRM via hello tre brandväggsprofiler (domän, privata och offentliga) hello och aktivera hello PowerShell Remote service:
   
   ```PowerShell
    Enable-PSRemoting -force
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```
3. Aktivera hello följande brandväggen regler tooallow hello RDP-trafik 

   ```PowerShell
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   ```   
4. Aktivera hello fil och skrivardelning regel så att hello VM kan svara tooa ping-kommandot i hello virtuellt nätverk:

   ```PowerShell
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   ``` 
5. Om hello VM ska ingå i en domän, kontrollera hello följande inställningar toomake att inte återställs hello tidigare inställningar. hello AD-principer som måste vara markerad är hello följande:

    - Aktivera hello profiler för Windows-brandväggen

        Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Nätverk\Nätverksanslutningar\Förhindra Connection\Windows Firewall\Domain Profile\Windows datorbrandvägg: **skydda alla nätverksanslutningar**

       Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Nätverk\Nätverksanslutningar\Förhindra Connection\Windows Firewall\Standard Profile\Windows datorbrandvägg: **skydda alla nätverksanslutningar**

    - Aktivera RDP 

        Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Nätverk\Nätverksanslutningar\Förhindra Connection\Windows Firewall\Domain Profile\Windows datorbrandvägg: **Tillåt inkommande undantag för fjärrskrivbord**

        Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Nätverk\Nätverksanslutningar\Förhindra Connection\Windows Firewall\Standard Profile\Windows datorbrandvägg: **Tillåt inkommande undantag för fjärrskrivbord**

    - Aktivera ICMP-V4

        Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Nätverk\Nätverksanslutningar\Förhindra Connection\Windows Firewall\Domain Profile\Windows datorbrandvägg: **Tillåt ICMP-undantag**

        Datorkonfiguration\Principer\Windows Settings\Administrative mallar\Nätverk\Nätverksanslutningar\Förhindra Connection\Windows Firewall\Standard Profile\Windows datorbrandvägg: **Tillåt ICMP-undantag**

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Kontrollera den virtuella datorn är felfri, säker och går att nå med RDP 
1. toomake att hello disk är felfri och konsekvent, gör en kontroll disk på hello nästa VM omstart:

    ```PowerShell
    Chkdsk /f
    ```
    Kontrollera att hello rapporten innehåller en ren och hälsosam disk.

2. Ange inställningar för hello Data BCD (Boot Configuration). 

    > [!Note]
    > Kontrollera att du kan köra dessa kommandon på en CMD-kommandotolk och **inte** på PowerShell:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. Kontrollera att Windows Management Instrumentation hello-databasen är konsekvent. tooperform detta, kör hello följande kommando:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Om hello-databasen är skadad finns [WMI: databasen skadas eller inte](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

4. Kontrollera att det inte något annat program som använder hello port 3389. Den här porten används för hello RDP-tjänst i Azure. Du kan köra **netstat - anob** toosee vilka portar i används på hello VM:

    ```PowerShell
    netstat -anob
    ```

5. Om hello Windows VHD som du vill tooupload är en domänkontrollant, följer du dessa steg:

    A. Följ [dessa extra steg](https://support.microsoft.com/kb/2904015) tooprepare hello disk.

    B. Se till att du vet hello DSRM-lösenordet om du har toostart hello VM i DSRM på någon punkt. Du kanske vill toorefer toothis länkar tooset hello [DSRM-lösenordet](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

6. Kontrollera att hello inbyggt administratörskonto och lösenord är kända tooyou. Du kanske vill tooreset hello aktuella lokala administratörslösenordet och se till att du kan använda det här kontot toosign i tooWindows via hello RDP-anslutning. Den här behörigheten åtkomst styrs av hello ”Tillåt inloggning genom Fjärrskrivbordstjänster” grupprincipobjekt. Du kan visa det här objektet i hello Redigeraren för lokal grupprincip under:

    Datorn Datorkonfiguration\Windows datorkonfiguration\windowsinställningar\säkerhetsinställningar\avancerad Säkerhetsinställningar\Lokala principer\Tilldelning av användarrättigheter

7. Kontrollera följande AD hello principerna toomake till att du inte blockerar din RDP-åtkomst via RDP eller från hello nätverk:

    - Datorkonfiguration\Windows datorkonfiguration\windowsinställningar\säkerhetsinställningar\avancerad Säkerhetsinställningar\Lokala principer\user Rights Assignment\Deny åtkomst toothis datorer från hello nätverk

    - Datorn Datorkonfiguration\Windows datorkonfiguration\windowsinställningar\säkerhetsinställningar\avancerad Säkerhetsinställningar\Lokala principer\user Rights Assignment\Deny inloggning via Fjärrskrivbordstjänster


8. Starta om hello VM toomake till att Windows är fortfarande felfri kan nås med hjälp av hello RDP-anslutning. Du kan nu vill toocreate en virtuell dator i din lokala Hyper-V toomake att hello VM startar helt och sedan testa om det är RDP kan nås.

9. Ta bort eventuella extra Transport Driver Interface filter, till exempel programvara som analyserar TCP paket eller extra brandväggar. Också kan granska på senare efter hello VM distribueras i Azure om det behövs.

10. Avinstallera alla andra tredje parts programvara och drivrutiner som är relaterade toophysical komponenter eller någon annan virtualiseringsteknik används.

### <a name="install-windows-updates"></a>Installera uppdateringar för Windows
hello bästa konfigurationen är för**har hello korrigeringsnivå hello datorns på hello senaste**. Om detta inte är möjligt, kontrollerar du att hello följande uppdateringar installeras:

|                       |                   |           |                                       Minsta filversion x64       |                                      |                                      |                            |
|-------------------------|-------------------|------------------------------------|---------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|
| Komponent               | Binär            | Windows 7 och Windows Server 2008 R2 | Windows 8 och Windows Server 2012             | Windows 8.1 och Windows Server 2012 R2 | Windows 10 och Windows Server 2016 RS1 | Windows 10 RS2             |
| Lagring                 | disk.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061           | -                                    | -                          |
|                         | Storport.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.332             |
|                         | NTFS.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.447             |
|                         | Iologmsg.dll      | 6.1.7601.23403 - KB3125574         | 6.2.9200.16384 - KB2995387                  | -                                    | -                                    | -                          |
|                         | Classpnp.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614           | 10.0.14393.953 - KB4022715           | -                          |
|                         | Volsnap.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384           | -                                    | 10.0.15063.0               |
|                         | partmgr.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | volmgr.sys        |                                    |                                             |                                      |                                      | 10.0.15063.0               |
|                         | Volmgrx.sys       | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | 10.0.15063.0               |
|                         | Msiscsi.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726           | 10.0.14393.1066 - KB4022715          | 10.0.15063.447             |
|                         | MSDSM.sys         | 6.1.7601.23403 - KB3125574         | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726           | -                                    | -                          |
|                         | MPIO.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726           | 10.0.14393.1198 - KB4022715          | -                          |
|                         | Fveapi.dll        | 6.1.7601.23311 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614           | 10.0.14393.576 - KB4022715           | -                          |
|                         | Fveapibase.dll    | 6.1.7601.23403 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614           | 10.0.14393.206 - KB4022715           | -                          |
| Nätverk                 | netvsc.sys        | -                                  | -                                           | -                                    | 10.0.14393.1198 - KB4022715          | 10.0.15063.250 - KB4020001 |
|                         | mrxsmb10.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726           | 10.0.14393.479 - KB4022715           | 10.0.15063.483             |
|                         | mrxsmb20.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.483             |
|                         | Mrxsmb.sys        | 6.1.7601.23816 - KB4022722         | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | Tcpip.sys         | 6.1.7601.23761 - KB4022722         | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.447             |
|                         | HTTP.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726           | 10.0.14393.251 - KB4022715           | 10.0.15063.483             |
|                         | vmswitch.sys      | 6.1.7601.23727 - KB4022719         | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.138             |
| Kärna                    | Ntoskrnl.exe      | 6.1.7601.23807 - KB4022719         | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.483             |
| Fjärrskrivbordstjänster | rdpcorets.dll     | 6.2.9200.21506 - KB4022719         | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.0               |
|                         | TERMSRV.dll       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850           | 10.0.14393.0 - KB4022715             | 10.0.15063.0               |
|                         | termdd.sys        | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Win32k.sys        | 6.1.7601.23807 - KB4022719         | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726           | 10.0.14393.594 - KB4022715           | -                          |
|                         | rdpdd.dll         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Rdpwd.sys         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
| Säkerhet                | På grund av tooWannaCrypt | KB4012212                          | KB4012213                                   | KB4012213                            | KB4012606                            | KB4012606                  |
|                         |                   |                                    | KB4012216                                   |                                      | KB4013198                            | KB4013198                  |
|                         |                   | KB4012215                          | KB4012214                                   | KB4012216                            | KB4013429                            | KB4013429                  |
|                         |                   |                                    | KB4012217                                   |                                      | KB4013429                            | KB4013429                  |
       
### När toouse sysprep<a id="step23"></a>    

Sysprep är en process som du kan köra i en windows-installation som återställs hello installation av hello system och ger en ”out of hello box experience” genom att ta bort alla personliga data och återställa flera komponenter. Du gör detta om du vill toocreate en mall som du kan distribuera flera andra virtuella datorer som har en viss konfiguration. Detta kallas en **generaliserad avbildning**.

Om du i stället du vill att endast toocreate en virtuell dator från en disk inte toouse sysprep. I så fall kan du bara skapa hello virtuell dator från vad kallas en **specialiserad avbildning**.

Mer information om hur toocreate en virtuell dator från en särskild disk, se:

- [Skapa en virtuell dator från en särskild disk](create-vm-specialized.md)
- [Skapa en virtuell dator från en särskild VHD-disk](https://azure.microsoft.com/resources/templates/201-vm-specialized-vhd/)

Om du vill toocreate en generaliserad avbildning måste toorun sysprep. Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx). 

Stöder den här generalisering inte varje roll eller program som är installerad på en Windows-baserad dator. Så innan du kör den här proceduren, se följande artikel toomake Kontrollera toohello stöds hello rollen för den datorn av sysprep. Mer information [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="steps-toogeneralize-a-vhd"></a>Steg toogeneralize en virtuell Hårddisk

>[!NOTE]
> När du har kört sysprep.exe som anges i hello följande inaktivera hello VM och inte aktiverar det igen förrän du har skapat en avbildning från den i Azure.

1. Logga in toohello Windows VM.
2. Kör **kommandotolk** som administratör. 
3. Ändra hello katalog till: **%windir%\system32\sysprep**, och kör sedan **sysprep.exe**.
3. I hello **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att hello **Generalize** är markerad.

    ![Systemförberedelseverktyget](media/prepare-for-upload-vhd-image/syspre.png)
4. I **avstängningsalternativ**väljer **avstängning**.
5. Klicka på **OK**.
6. Stäng hello virtuella datorn när Sysprep är klar. Använd inte **starta om** tooshut ned hello VM.
7. Hello är VHD nu redo toobe har överförts. Mer information om hur toocreate en virtuell dator från en generaliserad disk, se [överför en generaliserad virtuell Hårddisk och använda toocreate en nya virtuella datorer i Azure](sa-upload-generalized.md).


## <a name="complete-recommended-configurations"></a>Fullständig rekommenderade konfigurationer
hello följande inställningar påverkar inte VHD-överföring. Vi rekommenderar dock att du har konfigurerat dem.

* Installera hello [Azure VM-agenten](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du kan aktivera VM-tillägg. hello VM-tillägg implementerar de flesta av hello kritiska funktioner du kanske vill toouse med dina virtuella datorer, till exempel återställa lösenord, konfigurera RDP och så vidare. Mer information finns i:

    - [VM-Agent och tillägg – del 1](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-1/)
    - [VM-Agent och tillägg – del 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)
* hello Dump loggen kan vara användbar vid felsökning av problem med Windows krascher. Aktivera hello Dump Logginsamling:
  
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "AutoReboot" -Value 0 -Type DWord
    New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
    Om du får några fel under någon av hello procedur stegen i den här artikeln, det innebär att det redan finns hello registernycklar. Använd följande kommandon i stället hello i den här situationen:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
*  När hello VM har skapats i Azure, rekommenderar vi att du placerar hello växlingsfilens storlek på hello ”Temporal enhet” volym tooimprove prestanda. Du kan konfigurera detta på följande sätt:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile"
    ```
Om det finns några datadisk som är bifogade toohello VM, är hello Temporal enhet Volymens enhetsbeteckning vanligtvis ”d”. Den här beteckningen kan vara olika beroende på hello antalet tillgängliga enheter och hello-inställningar som du gör.

## <a name="next-steps"></a>Nästa steg
* [Överför en virtuell Windows-dator avbildningen tooAzure för Resource Manager distributioner](upload-generalized-managed.md)

