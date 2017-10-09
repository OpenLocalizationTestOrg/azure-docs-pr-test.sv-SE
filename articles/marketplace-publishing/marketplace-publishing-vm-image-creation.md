---
title: "aaaCreating en avbildning av virtuell dator för hello Azure Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar om hur toocreate en virtuell dator bild för hello Azure Marketplace för andra toopurchase."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 65e1c0530bb050fb379a52544e36c55faacd84df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-virtual-machine-image-for-hello-azure-marketplace"></a>Guiden toocreate en avbildning av virtuell dator för hello Azure Marketplace
Den här artikeln **steg 2**, vägleder dig genom förbereder hello virtuella hårddiskar (VHD) som du ska distribuera toohello Azure Marketplace. De virtuella hårddiskarna är hello foundation för din SKU: N. hello processen skiljer sig åt beroende på om du tillhandahåller en Linux- eller Windows-baserade SKU. Den här artikeln täcker båda scenarierna. Den här processen kan utföras parallellt med [skapande av konton och registrering][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Definiera erbjudanden och SKU: er
I det här avsnittet lär du dig toodefine hello erbjudanden och deras associerade SKU: er.

Ett erbjudande är en ”överordnad” tooall av dess SKU: er. Du kan ha flera erbjudanden. Hur du bestämmer dig för toostructure är erbjudandena upp tooyou. När ett erbjudande pushas toostaging pushas tillsammans med alla dess SKU: er. Du noga överväga din SKU-identifierare, eftersom de kommer att vara synliga i hello-URL:

* Azure.com: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}
* Azure preview portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}  

En SKU är hello kommersiella namn för en VM-avbildning. En VM-avbildning som innehåller en operativsystemdisk och noll eller flera datadiskar. Det är i stort sett hello fullständig lagringsprofil för en virtuell dator. En virtuell Hårddisk krävs per disk. Även tomt datadiskar kräver en VHD-toobe skapas.

Oavsett vilket operativsystem som du använder, lägga till endast hello minsta antal datadiskar som krävs av hello SKU. Kunder kan inte ta bort diskar som är en del av en avbildning vid hello tiden för distributionen, men kan alltid lägga till diskar under eller efter distributionen om de behöver dem..

> [!IMPORTANT]
> **Ändra inte Diskantalet i en ny Avbildningsversion.** Om du måste konfigurera om datadiskar hello bild, definiera en ny SKU. En ny version för avbildningen med annan disk antal har hello risken för att dela nya distribution baserad på hello ny Avbildningsversion i fall av automatisk skalning, automatisk distribution av lösningar via ARM-mallar och andra scenarier.
>
>

### <a name="11-add-an-offer"></a>1.1 Lägg till ett erbjudande
1. Logga in toohello [Publiceringsportal] [ link-pubportal] med säljaren-konto.
2. Välj hello **virtuella datorer** fliken hello Publishing Portal. Ange ditt erbjudande i hello ange fältet. hello är erbjudandet vanligtvis hello namn för hello produkt eller tjänst som du planerar toosell i hello Azure Marketplace.
3. Välj **Skapa**.

### <a name="12-define-a-sku"></a>1.2 definiera en SKU
När du har lagt till ett erbjudande, du behöver toodefine och identifiera din SKU: er. Du kan ha flera erbjudanden och varje erbjudande kan ha flera SKU: er under den. När ett erbjudande pushas toostaging pushas tillsammans med alla dess SKU: er.

1. **Lägg till en SKU.** hello SKU kräver en identifierare som används i hello-URL. hello-ID måste vara unikt inom din publiceringsprofil, men det finns ingen risk för identifierare kollision med andra utgivare.

   > [!NOTE]
   > hello erbjudandet och SKU-ID visas i URL: en i hello erbjudandet i hello Marketplace.
   >
   >
2. **Lägga till en sammanfattande beskrivning för din SKU.** Översikt över beskrivningar är synliga toocustomers så bör du se dem lättläst. Den här informationen behövs inte toobe låst tills hello ”Push tooStaging” fas. Fram till dess kan du är ledigt tooedit den.
3. Om du använder Windows-baserade SKU: er kan du följa hello föreslagna länkar tooacquire hello godkända versioner av Windows Server.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Skapa en virtuell Hårddisk på Azure-kompatibel (Linux-baserat)
Det här avsnittet fokuserar på bästa praxis för att skapa en Linux-baserade VM-avbildning för hello Azure Marketplace. En stegvis genomgång finns toohello följande dokumentation: [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Skapa en virtuell Hårddisk på Azure-kompatibel (Windows-baserade)
Det här avsnittet fokuserar på hello steg toocreate en SKU baserat på Windows Server för hello Azure Marketplace.

### <a name="31-ensure-that-you-are-using-hello-correct-base-vhds"></a>3.1 se till att du använder hello korrigera grundläggande virtuella hårddiskar
hello operativsystemet VHD för VM-avbildning måste baseras på en Azure-godkända basavbildning som innehåller Windows Server eller SQL Server.

toobegin, skapa en virtuell dator från en av följande avbildningar som finns på hello hello [Microsoft Azure-portalen][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])
* SQLServer 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])
* SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])

Dessa länkar kan också finnas i hello Publishing Portal under hello SKU sida.

> [!TIP]
> Om du använder hello aktuella Azure-portalen eller PowerShell, har Windows Server-avbildningar publicerade på 8 September 2014 och senare godkänts.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2 Skapa ditt Windows-baserad virtuell dator
Du kan skapa den virtuella datorn baserat på en godkänd basavbildning i några enkla steg från hello Microsoft Azure-portalen. hello följande är en översikt över hello processen:

1. Välj hello basavbildning sidan **Skapa virtuell dator** toobe dirigeras toohello nya [Microsoft Azure-portalen][link-azure-portal].

    ![Rita][img-acom-1]
2. Logga in toohello portal med hello Microsoft-konto och lösenord för hello Azure-prenumeration du vill toouse.
3. Följ hello prompter toocreate en virtuell dator med hjälp av hello basavbildning som du har valt. Behöver du tooprovide ett värdnamn (namnet på hello dator), användarnamn (registrerad som administratör) och lösenord för hello VM.

    ![Rita][img-portal-vm-create]
4. Välj hello storleken på hello VM toodeploy:

    a.    Om du planerar toodevelop hello VHD lokala spelar hello storlek ingen roll. Överväg att använda en av hello mindre virtuella datorer.

    b.    Om du planerar toodevelop hello avbildning i Azure bör du bör använda en av hello VM-storlekar för hello valda avbildningen.

    c.    Information om priser finns toohello **rekommenderas prisnivåer** selector visas på hello-portalen. Det ger hello tre rekommenderade storlekar som tillhandahålls av hello utgivare. (I det här fallet hello utgivaren är Microsoft)

    ![Rita][img-portal-vm-size]
5. Ange egenskaper för:

    a.    Snabb distribution kan du lämna hello standardvärden för hello under **valfri konfiguration** och **resursgruppen**.

    b.    Under **Lagringskonto**, du kan också välja hello storage-konto som hello operativsystemet virtuella Hårddisken ska sparas.

    c.    Under **resursgruppen**, du kan också välja hello logisk grupp i vilka tooplace hello VM.
6. Välj hello **plats** för distribution:

    a.    Om du planerar toodevelop hello VHD lokala roll inte hello plats eftersom du kommer att överföra hello avbildningen tooAzure senare.

    b.    Om du planerar toodevelop hello avbildning i Azure, Överväg att använda någon av hello USA-baserade Microsoft Azure-regioner från hello början. Detta förbättrar hello VHD kopieringen som Microsoft utför åt dig när du skickar avbildningen för certifiering.

    ![Rita][img-portal-vm-location]
7. Klicka på **Skapa**. hello VM startar toodeploy. I minuter, får en lyckad distribution och kan börja toocreate hello avbildning för din SKU.

### <a name="33-develop-your-vhd-in-hello-cloud"></a>3.3 utveckla den virtuella Hårddisken i hello moln
Vi rekommenderar starkt att du utveckla den virtuella Hårddisken i hello molnet med hjälp av Remote Desktop Protocol (RDP). Du kan ansluta tooRDP med hello användarnamn och lösenord som angetts under etableringen.

> [!IMPORTANT]
> Om du utvecklar dina VHD lokalt (vilket inte rekommenderas), se [och skapa en avbildning av virtuell dator lokalt](marketplace-publishing-vm-image-creation-on-premise.md). Hämta den virtuella Hårddisken är inte nödvändigt om du utvecklar i hello molnet.
>
>

**Anslut via RDP med hello [Microsoft Azure-portalen][link-azure-portal]**

1. Välj **Bläddra** > **VMs**.
2. hello virtuella datorer blad öppnas. Kontrollera att hello VM som du vill tooconnect med körs och markera den hello listan med distribuerade virtuella datorer.
3. Ett blad öppnas som beskriver hello valt VM. Hello överkant, klickar du på **Anslut**.
4. Du kan ange tooenter hello användarnamn och lösenord som du angav under etableringen.

**Anslut via RDP med hjälp av PowerShell**

toodownload en fjärrfil skrivbord tooa lokala dator använder hello [cmdlet Get-AzureRemoteDesktopFile][link-technet-2]. I ordning toouse denna cmdlet måste du tooknow hello namnet på hello-tjänst och namnet på hello VM. Om du har skapat hello VM från hello [Microsoft Azure-portalen][link-azure-portal], du kan hitta den här informationen under Egenskaper för Virtuella datorer:

1. I hello Microsoft Azure portal, väljer **Bläddra** > **VMs**.
2. hello virtuella datorer blad öppnas. Välj hello VM som du har distribuerat.
3. Ett blad öppnas som beskriver hello valt VM.
4. Klicka på **Egenskaper**.
5. hello första delen av hello domännamn är hello tjänstnamn. hello är värden hello VM-namn.

    ![Rita][img-portal-vm-rdp]
6. hello cmdlet toodownload hello RDP-filen för hello skapade VM toohello administratörens lokala dator är som följer.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

Mer information om RDP kan hittas på MSDN i hello artikeln [ansluta tooan virtuella Azure-datorn med RDP eller SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Konfigurera en virtuell dator och skapa din SKU**

Använd Hyper-v efter hello operativsystemet VHD hämtas och konfigurera en VM-toobegin skapar din SKU. Detaljerade anvisningar finns på följande länk för TechNet hello: [installera Hyper-v och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-hello-correct-vhd-size"></a>3.4 Välj hello rätt storlek för virtuell Hårddisk
hello Windows-operativsystem VHD i VM-avbildning ska skapas som en virtuell Hårddisk fast format 128 GB.  

Om hello fysiska storlek är mindre än 128 GB, vara hello VHD sparse. hello grundläggande Windows och SQL Server-avbildningar som uppfyller redan kraven, så ändra inte hello-format eller hello storleken på hello VHD hämtades.  

Datadiskar kan vara så stor som 1 TB. Kom ihåg att kunder inte ändra storlek på virtuella hårddiskar i en bild när hello distribution när du funderar över hello diskstorleken. Data disk virtuella hårddiskar ska skapas som en virtuell Hårddisk med fast format. De bör också vara sparse. Datadiskar kan antingen vara tomma eller innehålla data.

### <a name="35-install-hello-latest-windows-patches"></a>3.5 Installera hello senaste korrigeringarna i Windows
hello grundläggande avbildningar innehåller hello senaste korrigeringarna in tootheir publiceringsdatum. Se till att Windows Update har körts och att alla hello senaste kritiska och viktiga uppdateringar har installerats innan du publicerar hello operativsystemet VHD som du har skapat.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3,6 utföra ytterligare aktiviteter för konfiguration och schema vid behov
Överväg att använda en schemalagd aktivitet som körs vid start toomake alla slutgiltiga ändringar toohello VM när den har distribuerats om ytterligare konfiguration krävs:

* Det är en bästa praxis toohave hello aktiviteten Ta bort själva vid har körts.
* Ingen konfiguration bör lita på enheter än enheter C eller D, eftersom de är bara två hello-enheter som alltid är garanterat tooexist. Enhet C är hello operativsystemdisken och enhet D är hello tillfälliga lokal disk.

### <a name="37-generalize-hello-image"></a>3.7 generalisera hello-bild
Alla avbildningar i hello Azure Marketplace måste vara återanvändbara i ett allmänt sätt. Med andra ord måste hello operativsystemet VHD vara generaliserad:

* För Windows hello bilden ska vara ”Sysprep” och inga konfigurationer bör göras som inte stöder hello **sysprep** kommando.
* Du kan köra följande kommando från hello katalogen % windir%\System32\Sysprep hello.

        sysprep.exe /generalize /oobe /shutdown

  Information om hur toosysprep hello operativsystem finns i steg i följande MSDN-artikel hello: [skapa och ladda upp en Windows Server VHD-tooAzure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Distribuera en virtuell dator från din virtuella hårddiskar
När du har laddat upp de virtuella hårddiskarna (operativsystem hello generaliserad virtuell Hårddisk och noll eller fler data disk virtuella hårddiskar) tooan Azure storage-konto kan du registrera dem som en användare VM-avbildning. Du kan sedan testa avbildningen. Observera att eftersom operativsystemet VHD är generaliserad, du inte kan direkt distribuera hello VM genom att tillhandahålla hello VHD URL.

toolearn mer om VM-avbildningar, granska hello följande blogginlägg:

* [VM-avbildning](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [VM avbildningen PowerShell så](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Om VM-avbildningar i Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-hello-necessary-tools-powershell-and-azure-cli"></a>Ställ in hello verktyg som krävs, PowerShell och Azure CLI
* [Hur toosetup PowerShell](/powershell/azure/overview)
* [Hur toosetup Azure CLI](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 skapa en användare VM-avbildning
#### <a name="capture-vm"></a>Spela in VM
Läs hello länkar som anges nedan anvisningar om hur du fångar hello VM med hjälp av Azure-API/PowerShell CLI.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Generalisera avbildningen
Läs hello länkar som anges nedan anvisningar om hur du fångar hello VM med hjälp av Azure-API/PowerShell CLI.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 distribuera en virtuell dator från en användare VM-avbildning
toodeploy en virtuell dator från en användare VM-avbildning som du kan använda hello aktuella [Azure-portalen](https://manage.windowsazure.com) eller PowerShell.

**Distribuera en virtuell dator från hello aktuella Azure-portalen**

1. Gå för**ny** > **Compute** > **virtuella** > **från galleriet**.

    ![Rita][img-manage-vm-new]
2. Gå för**Mina avbildningar**, och sedan hello Välj VM-avbildning från vilken toodeploy en virtuell dator:

   1. Betala per uppmärksam toowhich bilden, eftersom hello **Mina avbildningar** visas både avbildningar av operativsystem och VM-avbildningar.
   2. Titta på hello antalet diskar kan hjälpa avgöra vilken typ av bild som du distribuerar, eftersom hello majoriteten av VM-avbildningar har mer än en disk. Det är dock fortfarande möjligt toohave en VM-avbildning med bara en enda disk, som sedan måste **antalet diskar** ange too1.

      ![Rita][img-manage-vm-select]
3. Följ guiden för hello VM skapa och ange hello VM-namn, VM-storlek, plats, användarnamn och lösenord.

**Distribuera en virtuell dator från PowerShell**

Du kan använda hello följande cmdlets toodeploy som en stor virtuell dator från hello generaliserad VM-avbildning som precis har skapat.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Se [felsöka vanliga problem som påträffas under skapande av VHD] för ytterligare hjälp.
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. Hämta certifikat för VM-avbildning
hello nästa steg förbereder VM-avbildning för hello Azure Marketplace är den certifierade toohave.

Den här processen omfattar att köra ett verktyg för särskilda certifiering, överför hello verifiering resultat toohello Azure-behållaren där de virtuella hårddiskarna finnas, lägga till ett erbjudande, definiera din SKU, och skickar den virtuella datorn image för certifiering.

### <a name="51-download-and-run-hello-certification-test-tool-for-azure-certified"></a>5.1 hämta och köra hello certifikatutfärdare-verktyget för Azure-certifierad
hello certifikatutfärdare verktyget körs på en körande VM som etablerats från dina användare VM-avbildning, tooensure som hello VM-avbildning är kompatibel med Microsoft Azure. Den verifierar att hello vägledning och krav om hur du förbereder den virtuella Hårddisken har uppfyllts. hello utdata hello-verktyget är en Kompatibilitetsrapport som ska överföras på hello Publishing Portal när du begär certifiering.

hello certifikatutfärdare verktyget kan användas med både Windows och Linux virtuella datorer. Ansluter tooWindows-baserade virtuella datorer via PowerShell och ansluter tooLinux virtuella datorer via SSH.Net:

1. Hämta först hello certifikatutfärdare verktyget hello [Microsoft hämtningsplats][link-msft-download].
2. Öppna hello certifikatutfärdare för och klicka sedan på hello **Starta nytt Test** knappen.
3. Från hello **testa Information** skärmen, ange ett namn för hello-testkörningen.
4. Välj om din virtuella dator körs på Linux eller Windows. Välj hello efterföljande alternativ beroende på vilket du väljer.

### <a name="connect-hello-certification-tool-tooa-linux-vm-image"></a>**Ansluta hello certifikatutfärdare verktyget tooa Linux VM-avbildning**
1. Välj hello SSH-autentiseringsläge: lösenord eller nyckel filen.
2. Ange hello Domain Name System (DNS) namn, användarnamn och lösenord om du använder lösenordsbaserad autentisering.
3. Om nyckeln autentisering med ange hello DNS-namn, användarnamn och privat nyckel plats.

   ![För lösenordsautentisering av Linux VM-avbildning][img-cert-vm-pswd-lnx]

   ![Nyckelfilen autentisering av Linux VM-avbildning][img-cert-vm-key-lnx]

### <a name="connect-hello-certification-tool-tooa-windows-based-vm-image"></a>**Ansluta hello certifikatutfärdare verktyget tooa Windows-baserade VM-avbildning**
1. Ange hello fullständigt kvalificerade VM DNS-namn (till exempel MyVMName.Cloudapp.net).
2. Ange hello användarnamn och lösenord.

   ![Autentisering av lösenord i Windows VM-avbildning][img-cert-vm-pswd-win]

När du har valt hello rätt alternativ för Linux- eller Windows-baserade Virtuella bilden, Välj **Testanslutningen** tooensure SSH.Net eller PowerShell har en giltig anslutning för testning. När en anslutning upprättas väljer **nästa** toostart hello test.

När hello test har slutförts får du hello resultat (varning-Pass/misslyckas) för varje test-element.

![Testfall för Linux VM-avbildning][img-cert-vm-test-lnx]

![Testfall för Windows VM-avbildning][img-cert-vm-test-win]

Om någon av hello testerna misslyckas certifieras inte bilden. Om detta inträffar kan du granska hello krav och gör nödvändiga ändringar.

När hello automatiserad test, tillfrågas du tooprovide ytterligare indata på VM-avbildning via en frågeformulär skärm.  Slutför hello frågor och välj sedan **nästa**.

![Certifikatutfärdare verktyget frågeformulär][img-cert-vm-questionnaire]

![Certifikatutfärdare verktyget frågeformulär][img-cert-vm-questionnaire-2]

När du har slutfört hello frågeformulär, kan du ange ytterligare information, till exempel SSH åtkomstinformation för hello Linux VM-avbildning och en förklaring till misslyckade bedömningar. Du kan hämta hello testresultaten och loggfiler för hello utförs testfall tillägg tooyour svar toohello frågeformulär. Spara hello resultat i hello samma behållare som de virtuella hårddiskarna.

![Spara certifikatutfärdare testresultat][img-cert-vm-results]

### <a name="52-get-hello-shared-access-signature-uri-for-your-vm-images"></a>5.2 hämta signatur för delad åtkomst hello URI för VM-avbildningar
Under publiceringsprocessen hello, ange OEM-tillverkare (hello uniform resource Identifier) som leda tooeach av hello virtuella hårddiskar som du har skapat för ditt SKU. Microsoft behöver åtkomst toothese virtuella hårddiskar under hello certifieringsprocess. Du måste därför toocreate signatur för delad åtkomst URI för varje virtuell Hårddisk. Detta är hello URI som ska anges i den **bilder** fliken i hello Publishing Portal.

signatur för delad åtkomst hello URI skapas bör följa toohello följande krav:

* När du genererar signatur för delad åtkomst URI: er för de virtuella hårddiskarna räcker lista och läsbehörighet. Bevilja inte skriv- eller raderingsbehörighet.
* hello varaktighet för åtkomst ska vara minst tre (3) veckor från när signatur för delad åtkomst hello URI: N har skapats.
* toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum. Hello aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.

SAS-URL kan vara genererats i flera olika sätt tooshare den virtuella Hårddisken för Azure Marketplace.
Följande är hello 3 rekommenderade verktyg:

1.  Azure Lagringsutforskaren
2.  Microsoft Lagringsutforskaren
3.  Azure CLI

**Azure Lagringsutforskaren (rekommenderas för Windows-användare)**

Följande är hello steg för att generera SAS-URL med hjälp av Azure Lagringsutforskaren

1. Hämta [förhandsvisning 3 av Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) från CodePlex. Gå för[Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) och på **”hämtar”**.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. Hämta [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) och installera efter packa upp den.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. Öppna programmet hello när den har installerats.
4. Klicka på **Lägg till konto**.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Ange hello lagringskontonamn, lagringskontonyckel och lagring slutpunkter domän. Detta är hello storage-konto i din Azure-prenumeration där du har sparat den virtuella Hårddisken på Azure-portalen.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. När Azure Lagringsutforskaren ansluts tooyour lagringskonto, startas visar alla hello innehåller inom hello storage-konto. Välj hello behållare dit du kopierade hello operativsystemet disk VHD-filen (även datadiskar om de är tillämpliga för ditt scenario).

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. När du har valt hello blob-behållare startar Azure Lagringsutforskaren visar hello filer i hello behållaren. Välj hello avbildningsfil (.vhd) som behöver toobe har skickats.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  När du har valt hello VHD-filen i hello behållaren, klicka på hello **säkerhet** fliken.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  I hello **Blob-behållaren säkerhet** dialogrutan lämna hello standardinställningar på hello **åtkomstnivå** fliken och klicka sedan på **signaturer för delad åtkomst** fliken.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. Så hello nedan toogenerate en signatur för delad åtkomst URI för hello VHD-avbildningen:

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Åtkomst tillåts från:** toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum. Hello aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.

    b. **Åtkomst till tillåten:** Välj ett datum som är minst tre veckor efter hello **åtkomst tillåts från** datum.

    c. **Åtgärder som tillåts:** väljer hello **lista** och **Läs** behörigheter.

    d. Om du har valt VHD-filen på rätt sätt och sedan filen visas i **Blob-namnet tooaccess** med filnamnstillägget .vhd.

    e. Klicka på **generera signatur**.

    f. I **genereras delad åtkomst signatur URI: N för den här behållaren**, Sök efter hello efter som markerade ovan:

       - Se till att bilden filnamn och **”VHD”** i hello URI.
       - Hello slutet av hello signatur, se till att **”= rl”** visas. Detta demonstrerar att läs- och åtkomst har angetts korrekt.
       - I mitten av hello signatur, se till att **”sr = c”** visas. Detta demonstrerar att du har åtkomst behållare

11. tooensure som hello genereras delad åtkomst signatur URI fungerar klickar du på **Test i webbläsaren**. Det bör börja hello hämtningen.

12. Kopiera signatur för delad åtkomst hello URI. Detta är hello URI toopaste till hello Publishing Portal.

13. Upprepa steg 6 – 10 för varje virtuell Hårddisk i hello SKU.

**Microsoft Azure Lagringsutforskaren (Windows-/ MAC/Linux)**

Följande är hello steg för att generera SAS-URL genom att använda Microsoft Azure Lagringsutforskaren

1.  Ladda ned Microsoft Azure Lagringsutforskaren formuläret [http://storageexplorer.com/](http://storageexplorer.com/) webbplats. Gå för[Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/releasenotes.html) och på **”hämta för Windows”**.

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  Öppna programmet hello när den har installerats.

3.  Klicka på **Lägg till konto**.

4.  Konfigurera Microsoft Azure Lagringsutforskaren tooyour prenumerationen genom att logga in tooyour konto

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Gå toostorage konto och välj hello behållare

6.  Välj **”hämta resursen Åtkomstsignatur...”** med hjälp av högerklickar du på hello **behållare**

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Starttiden för uppdateringen, förfallotiden och behörigheter enligt följande

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Starttid:** toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum. Hello aktuella datumet infaller 6 oktober 2014 väljer du exempelvis 2014-10-5.

    b.  **Förfallotiden:** Välj ett datum som är minst tre veckor efter hello **starttid** datum.

    c.  **Behörigheter:** väljer hello **lista** och **Läs** behörigheter

8.  Kopiera URI för signatur för delad åtkomst av behållare

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Genererade SAS-URL är för behållare nivå och nu måste vi tooadd VHD namn i den.

    Format för behållare nivån SAS-URL:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Infoga VHD namn efter hello behållarens namn i SAS-URL som nedan`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Exempel:

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    TestRGVM201631920152.vhd är hello VHD-namn ska vara VHD SAS-URL`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Se till att bilden filnamn och **”VHD”** i hello URI.
    - I mitten av hello signatur, se till att **”sp = rl”** visas. Detta demonstrerar att läs- och åtkomst har angetts korrekt.
    - I mitten av hello signatur, se till att **”sr = c”** visas. Detta demonstrerar att du har åtkomst behållare

9.  tooensure som hello genererade delad åtkomst signatur URI fungerar, testa den i webbläsaren. Det bör starta hämtningsprocessen för hello

10. Kopiera signatur för delad åtkomst hello URI. Detta är hello URI toopaste till hello Publishing Portal.

11. Upprepa dessa steg för varje virtuell Hårddisk i hello SKU.

**Azure CLI (rekommenderas för icke-Windows & kontinuerlig Integration)**

Följande är hello steg för att generera SAS-URL med hjälp av Azure CLI

1.  Ladda ned Microsoft Azure CLI från [här](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). Du kan också hitta olika länkar för  **[Windows](http://aka.ms/webpi-azure-cli)**  och  **[MAC OS x](http://aka.ms/mac-azure-cli)**.

2.  När du har laddat ned och installera

3.  Skapa en PowerShell-fil med följande kod och spara den i lokalt

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Uppdatera hello följande parametrar i ovan

    a. **`<StorageAccountName>`**: Ange namnet på ditt lagringskonto

    b. **`<Storage Account Key>`**: Ger din lagringskontonyckel

    c. **`<Permission Start Date>`**: toosafeguard för UTC-tid, väljer hello dagen före hello aktuellt datum. Till exempel om hello aktuellt datum är 26 oktober 2016 och värdet ska vara 10/25/2016

    d. **`<Permission End Date>`**: Välj ett datum som är minst tre veckor efter hello **startdatum**. Värdet ska vara **2016-02-11**.

    Följande är kodexempel hello när du har uppdaterat rätt parametrar

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  Öppna Redigeraren för Powershell med ”Kör som administratör”-läge och öppna filen i steg #3.

5.  Kör hello skript och det ger du hello SAS-URL för behållaren åtkomst

    Följande kommer att hello utdata från hello SAS signatur och kopiera hello markerat en del i en anteckningar

    ![Rita](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Nu får du behållaren nivå SAS-URL och du behöver tooadd VHD namn i den.

    Behållaren nivån SAS-URL #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  Infoga VHD namn efter hello behållarnamn i SAS-URL som visas nedan`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Exempel:

    TestRGVM201631920152.vhd är hello VHD-namn ska vara VHD SAS-URL

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Kontrollera att dina bildfilens namn och ”VHD” i hello URI.
    -   I mitten av hello signatur, se till att ”sp = rl” visas. Detta demonstrerar att läs- och åtkomst har angetts korrekt.
    -   I mitten av hello signatur, se till att ”sr = c” visas. Detta demonstrerar att du har åtkomst behållare

8.  tooensure som hello genererade delad åtkomst signatur URI fungerar, testa den i webbläsaren. Det bör starta hämtningsprocessen för hello

9.  Kopiera signatur för delad åtkomst hello URI. Detta är hello URI toopaste till hello Publishing Portal.

10. Upprepa dessa steg för varje virtuell Hårddisk i hello SKU.


### <a name="53-provide-information-about-hello-vm-image-and-request-certification-in-hello-publishing-portal"></a>5.3 innehåller information om hello VM-avbildning och begär certifiering i hello Publishing Portal
När du har skapat ditt erbjudande och SKU, ska du ange hello avbildningsinformation som är associerade med den SKU:

1. Gå toohello [Publiceringsportal][link-pubportal], och sedan logga in med ditt säljare.
2. Välj hello **VM-avbildningar** fliken.
3. hello identifierare som nämns i hello överst på sidan för hello är faktiskt hello erbjuder identifierare och inte hello SKU-identifierare.
4. Ange hello egenskaper under hello **SKU: er** avsnitt.
5. Under **operativsystemsfamilj**, klicka på hello typ av operativsystem som är associerade med hello operativsystemet VHD.
6. I hello **operativsystemet** rutan, beskriver hello-operativsystemet. Överväg ett format som operativsystemsfamilj, typ, version och uppdateringar. Ett exempel är ”Windows Server Datacenter 2014 R2”.
7. Välj toosix rekommenderade storlekar för virtuella datorer. Dessa är rekommendationer som får visas toohello kunden i hello priser nivå bladet i hello Azure-portalen när de bestämma toopurchase och distribuera avbildningen. **Detta är endast rekommendationer. hello kunden är kan tooselect VM-storlek som hanterar hello diskar som anges i bilden.**
8. Ange hello version. hello versionsfältet kapslar in en semantiska version tooidentify hello produkt och dess uppdateringar:
   * Versioner ska hello formatet X.Y.Z, där X, Y och Z är heltal.
   * Bilder i olika SKU: er kan ha olika versioner av högre och lägre.
   * Versioner i en SKU bara ska inkrementella ändringar som ökar hello korrigering version (Z från X.Y.Z).
9. I hello **OS VHD URL** ange hello signatur för delad åtkomst URI som skapats för hello operativsystemet VHD.
10. Om det finns datadiskar som är associerade med denna SKU, Välj hello logisk enhet nummer (LUN) toowhich som den här data disk toobe monteras på distribution.
11. I hello **LUN X VHD URL** ange hello signatur för delad åtkomst URI som skapats för hello först data VHD.

    ![Rita](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Vanliga problem med SAS-URL & korrigeringar

|Problem|Felmeddelande|Åtgärda|Länk till dokumentation|
|---|---|---|---|
|Det gick inte att kopiera bilder - ””? hittades inte i SAS-url|Fel: Kopiera avbildningar. Det går inte toodownload blob med som SAS-Uri.|Uppdatera hello SAS-Url med rekommenderade verktyg|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Det gick inte att kopiera bilder - ”a” och ”se” parametrar inte i SAS-url|Fel: Kopiera avbildningar. Det går inte toodownload blob med som SAS-Uri.|Uppdatera hello SAS-Url med Start- och slutdatum på den|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Det gick inte att kopiera avbildningar - ”sp = rl” inte i SAS-url|Fel: Kopiera avbildningar. Det går inte toodownload blob med angivna SAS-Uri|Uppdatera hello SAS-Url med behörigheter som har angetts som ”läsa” och ”lista|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Det gick inte att kopiera avbildningar - SAS-url har blanksteg i vhd-namn|Fel: Kopiera avbildningar. Det går inte toodownload blob med som SAS-Uri.|Uppdatera hello SAS-Url utan blanksteg|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Det gick inte att kopiera avbildningar – Url-auktorisering för SAS-fel|Fel: Kopiera avbildningar. Det går inte toodownload blob på grund av tooauthorization fel|Återskapa hello SAS-Url|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a>Nästa steg
När du är klar med hello SKU uppgifter du kan flytta framåt toohello [administratörshandboken för Azure Marketplace marketing content][link-pushstaging]. I steget av hello publiceringsprocessen och ger du hello marknadsföring innehåll, priser och andra nödvändiga innan information för**steg3: testa den virtuella datorn erbjuder i Förproduktion**, där du kan testa olika användningsfall scenarier innan du distribuerar hello erbjudande toohello Azure Marketplace för offentliga synlighet och inköp.  

## <a name="see-also"></a>Se även
* [Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
