---
title: "aaaHow toocreate en anpassad mallavbildning för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad mall bild för Azure RemoteApp. Du kan använda den här mallen med en hybrid eller molnet samling."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: b9ec5b51-f7cd-470b-8545-d0fd714c5982
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9e3a0b2d3cc56177ea51406e6cecfe19ff235340
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-custom-template-image-for-azure-remoteapp"></a>Hur toocreate en anpassad mall bild för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp använder en Windows Server 2012 R2 mallen avbildningen toohost alla hello program som du vill tooshare med dina användare. toocreate en anpassad RemoteApp-mallavbildningen som du kan börja med en befintlig avbildning eller skapa en ny. 

> [!TIP]
> Visste du att du kan skapa en avbildning från en Azure VM? True artikeln och minskar hello lång tid det tar tooimport hello avbildningen. Kolla in hello steg [här](remoteapp-image-on-azurevm.md).
> 
> 

hello kraven för hello-avbildning som kan överföras för användning med Azure RemoteApp är:

* hello avbildningens storlek bör vara en multipel av MB. Om du försöker tooupload en avbildning som inte är en exakt multipel misslyckas hello överföringen.
* hello bildstorleken måste vara 127 GB eller mindre.
* Det måste finnas i en VHD-fil (VHDX-filer [Hyper-V virtuella hårddiskar] är för närvarande stöds inte).
* hello VHD får inte vara en virtuell dator i generation 2.
* hello VHD kan vara fast storlek eller dynamiskt expanderande. En dynamiskt expanderande virtuell Hårddisk rekommenderas eftersom det tar mindre tid tooupload tooAzure än en VHD-fil med fast storlek.
* hello disken måste initieras med hello Master Boot Record (MBR) partitionering format. hello partitionstyp för GUID partition table (GPT) stöds inte.
* hello VHD måste innehålla en enkel installation av Windows Server 2012 R2. Den kan innehålla flera volymer, men bara en som innehåller en installation av Windows.
* hello Remote värd för fjärrskrivbordssession (RDSH)-rollen och hello Skrivbordsmiljö måste vara installerad.
* hello Anslutningsutjämning för fjärrskrivbord roll måste *inte* installeras.
* hello Krypterande filsystem (EFS) måste vara inaktiverad.
* hello bilden måste vara Sysprep med hello parametrar **/oobe / generalize/shutdown** (Använd inte hello **/mode:vm** parametern).
* Överför den virtuella Hårddisken från en ögonblicksbild kedja stöds inte.

**Innan du börjar**

Du behöver toodo hello följande innan du skapar hello-tjänsten:

* [Registrera dig](https://azure.microsoft.com/services/remoteapp/) för RemoteApp.
* Skapa ett användarkonto i Active Directory toouse som hello RemoteApp-tjänstkontot. Begränsa hello behörigheter för det här kontot så att den kan bara ansluta till datorer toohello domän. Se [konfigurera Azure Active Directory för RemoteApp](remoteapp-ad.md) för mer information.
* Samla in information om ditt lokala nätverk: IP-adressen information och information om VPN-enhet.
* Installera hello [Azure PowerShell](/powershell/azure/overview) modul.
* Samla in information om hello-användare som du vill toogrant åtkomst till. Detta kan vara antingen Microsoft-kontoinformation eller Active Directory fungerar kontoinformationen för användare.

## <a name="create-a-template-image"></a>Skapa en för mallavbildning
Hello hög nivå steg toocreate en ny mall-avbildning från grunden är följande:

1. Leta upp en Windows Server 2012 R2 Update DVD eller ISO-avbildning.
2. Skapa en VHD-fil.
3. Installera Windows Server 2012 R2.
4. Installera hello Remote värd för fjärrskrivbordssession (RDSH)-rollen och hello Skrivbordsmiljö.
5. Installera ytterligare funktioner som krävs för dina program.
6. Installera och konfigurera dina program. toomake dela appar enklare, Lägg till appar eller program som du vill tooshare toohello **starta** menyn hello-avbildning, särskilt i **%systemdrive%\ProgramData\Microsoft\Windows\Start menyn\Program.
7. Utföra några ytterligare Windows-konfigurationer som krävs av dina program.
8. Inaktivera hello Krypterande filsystem (EFS).
9. **KRÄVS:** gå tooWindows Update och installera alla viktiga uppdateringar.
10. SYSPREP-avbildning som hello.

hello är detaljerade anvisningar för att skapa en ny avbildning:

1. Leta upp en Windows Server 2012 R2 Update DVD eller ISO-avbildning.
2. Skapa en VHD-fil med hjälp av Diskhantering.
   
   1. Öppna Diskhantering (diskmgmt.msc).
   2. Skapa en dynamiskt expanderande virtuell Hårddisk på 40 GB i storlek. (Uppskattning hello mängden utrymme som krävs för Windows, program och anpassningar. Windows Server med hello RDSH-rollen och funktionen Skrivbordsmiljö kräver cirka 10 GB diskutrymme).
      
      1. Klicka på **åtgärd > Skapa virtuell Hårddisk**.
      2. Ange hello plats, storlek och VHD-format. Välj **dynamiskt expanderande**, och klicka sedan på **OK**.
      
      Körs på flera sekunder. Du bör se en ny disk utan någon enhetsbeteckning och i ”inte initiera” tillstånd i hello Diskhanteringskonsolen när hello skapa en virtuell Hårddisk är klar.
      
      * Högerklicka på hello disk (inte hello ledigt utrymme) och klicka sedan på **initiera Disk**. Välj **MBR** (Master Boot Record) som hello partitionstyp och klicka sedan på **OK**.
      * Skapa en ny volym: Högerklicka på hello ledigt utrymme och klicka sedan på **ny enkel volym**. Du kan acceptera hello standardvärden i hello guiden, men se till att du tilldelar en enhet bokstav tooavoid potentiella problem när du överför hello mallavbildningen.
      * Högerklicka på hello disk och klicka sedan på **koppla från VHD**.
3. Installera Windows Server 2012 R2:
   
   1. Skapa en ny virtuell dator. Använd hello guiden Ny virtuell dator i Hyper-V Manager eller i Client Hyper-V.
      1. På sidan Ange Generation hello väljer **Generation 1**.
      2. Välj på sidan Anslut virtuell hårddisk hello **använder en befintlig virtuell hårddisk**, och bläddra toohello VHD som du skapade i föregående steg i hello.
      3. Välj på sidan installationsalternativ hello **installera ett operativsystem från en start-CD/DVD_ROM**, och välj sedan hello platsen för installationsmediet för Windows Server 2012 R2.
      4. Välj andra alternativ i hello guiden nödvändiga tooinstall Windows och dina program. Hello-guiden har slutförts.
   2. När hello-guiden har slutförts redigera hello inställningarna för hello virtuell dator och gör andra ändringar nödvändiga tooinstall Windows och program, till exempel hello antalet virtuella processorer och klicka sedan på **OK**.
   3. Anslut toohello virtuell dator och installera Windows Server 2012 R2.
4. Installera hello Remote värd för fjärrskrivbordssession (RDSH)-rollen och hello Skrivbordsmiljö:
   1. Starta Serverhanteraren.
   2. Klicka på **Lägg till roller och funktioner** på Välkommen hello-skärmen eller från hello **hantera** menyn.
   3. Klicka på **nästa** på sidan för hello innan du börjar.
   4. Välj **rollbaserad eller funktionsbaserad installation**, och klicka sedan på **nästa**.
   5. Välj hello lokal dator hello listan och klicka sedan på **nästa**.
   6. Välj **Fjärrskrivbordstjänster**, och klicka sedan på **nästa**.
   7. Expandera **användargränssnitt och infrastruktur** och välj **Skrivbordsmiljö**.
   8. Klicka på **Lägg till funktioner**, och klicka sedan på **nästa**.
   9. Klicka på hello Remote Desktop Services page **nästa**.
   10. Klicka på **värd för fjärrskrivbordssession**.
   11. Klicka på **Lägg till funktioner**, och klicka sedan på **nästa**.
   12. Hello bekräfta installationen val på sidan Välj **omstart hello-målservern automatiskt om det behövs**, och klicka sedan på **Ja** på hello startar du om varningen.
   13. Klicka på **Installera**. hello datorn startas.
5. Installera ytterligare funktioner som krävs för ditt program, till exempel hello .NET Framework 3.5. tooinstall hello funktioner, kör guiden hello Lägg till roller och funktioner.
6. Installera och konfigurera hello program och program som du vill toopublish via RemoteApp.

> [!IMPORTANT]
> Installera hello RDSH-rollen innan du installerar program tooensure att eventuella problem med programkompatibilitet identifieras innan hello bilden är överförs tooRemoteApp.
> 
> Kontrollera att en genväg tooyour program (**lnk** fil) visas i hello **starta** menyn för alla användare (%systemdrive%\ProgramData\Microsoft\Windows\Start menyn\Program). Kontrollera också att hello-ikon som visas i hello **starta** menyn är vad du vill att användare toosee. Om du inte ändra den. (Du inte *har* tooadd hello programmet toohello Start-menyn, men det gör det mycket enklare toopublish hello program i RemoteApp. Du måste annars tooprovide hello installationssökväg för programmet hello när du publicerar hello app.)
> 
> 

1. Utföra några ytterligare Windows-konfigurationer som krävs av dina program.
2. Inaktivera hello Krypterande filsystem (EFS). Kör följande kommando vid en upphöjd Windows hello:
   
     Fsutil beteende ange disableencryption 1
   
   Du kan också ange eller Lägg till följande DWORD-värdet i registret hello hello:
   
     HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
3. Om du skapar en avbildning i en virtuell Azure-dator, byta namn på hello  **\%windir%\Panther\Unattend.xml** -fil, som det blockerar hello överför skript används senare inte fungerar. Ändra hello namnet på den här filen tooUnattend.old så att du kommer att ha hello-filen om du behöver toorevert din distribution.
4. Gå tooWindows Update och installera alla viktiga uppdateringar. Du kan behöva toorun Windows Update flera gånger tooget alla uppdateringar. (Du installerar ibland en uppdatering och kräver en uppdatering för att uppdatera sig själv.)
5. SYSPREP-avbildning som hello. Kör hello följande kommando vid en upphöjd kommandotolk:
   
   **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe / shutdown**
   
   **Obs:** inte använder hello **/mode:vm** växel av hello SYSPREP kommandot även om det här är en virtuell dator.

## <a name="next-steps"></a>Nästa steg
Nu när du har en egeninställd mallavbildning måste tooupload det bild tooyour RemoteApp-samlingen. Använd hello informationen i följande artiklar toocreate hello samlingen:

* [Hur toocreate en hybridsamling av RemoteApp](remoteapp-create-hybrid-deployment.md)
* [Hur toocreate en molnsamling i RemoteApp](remoteapp-create-cloud-deployment.md)

