---
title: aaaCreate och ladda upp en FreeBSD VM bild | Microsoft Docs
description: "Läs toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello FreeBSD operativsystemet toocreate en virtuell Azure-dator"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kyliel
ms.openlocfilehash: f3bd155e496f1a2713d36bb66ea9824ed4c210ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-freebsd-vhd-tooazure"></a>Skapa och ladda upp en FreeBSD VHD-tooAzure
Den här artikeln visar hur toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello FreeBSD-operativsystemet. När du har överfört kan du använda den som din egen avbildning toocreate en virtuell dator (VM) i Azure.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Information om hur du överför en virtuell Hårddisk med hello Resource Manager-modellen finns [här](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har hello följande objekt:

* **En Azure-prenumeration**--om du inte har ett konto kan du skapa en på bara några minuter. Om du har en MSDN-prenumeration, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Annars kan du lära dig hur för[skapa ett kostnadsfritt utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure PowerShell-verktyg**--hello Azure PowerShell-modulen måste vara installerad och konfigurerad toouse din Azure-prenumeration. toodownload hello modulen, se [Azure hämtar](https://azure.microsoft.com/downloads/). En genomgång som beskriver hur tooinstall och konfigurera hello modulen finns här. Använd hello [Azure hämtar](https://azure.microsoft.com/downloads/) cmdlet tooupload hello VHD.
* **FreeBSD operativsystem i en VHD-fil**--ett operativsystem som stöds FreeBSD måste vara installerad tooa virtuell hårddisk. Flera verktyg finns toocreate VHD-filer. Du kan till exempel använda en virtualiseringslösning till exempel Hyper-V toocreate hello VHD-filen och installera hello-operativsystemet. Anvisningar om hur tooinstall och använda Hyper-V, se [installera Hyper-V och skapa en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello senare VHDX-format stöds inte i Azure. Du kan konvertera hello diskformat tooVHD med hjälp av Hyper-V Manager eller hello cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx). Dessutom finns en [självstudiekursen på MSDN om hur toouse FreeBSD med Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

Den här uppgiften innehåller hello följande fem steg:

## <a name="step-1-prepare-hello-image-for-upload"></a>Steg 1: Förbered hello avbildning för överföring
Slutför hello följande procedurer på hello virtuell dator där du installerade hello FreeBSD-operativsystem:

1. Aktivera DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. Aktivera SSH.

    Se till att hello SSH-server är installerat och konfigurerat toostart vid start. Som standard aktiveras efter installationen från FreeBSD-skiva. 
3. Ställ in en seriekonsol.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Installera sudo.

    Hej rotkontot har inaktiverats i Azure. Detta innebär att du behöver tooutilize sudo från en icke-privilegierade användare toorun kommandon med förhöjda privilegier.

        # pkg install sudo
   
5. Förutsättningar för Azure-agenten.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Installera Azure-agenten.

    hello senaste versionen av hello Azure-agenten alltid finns på [github](https://github.com/Azure/WALinuxAgent/releases). Hej version 2.0.10 + officiellt stöder FreeBSD 10 & 10.1 och hello version 2.1.4 + (inklusive 2.2.x) officiellt stöder FreeBSD 10.2 och senare versioner.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    För 2.0, vi använder 2.0.16 som exempel:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    För 2.1 kan vi använda 2.1.4 som exempel:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > När du har installerat Azure-agenten är det en bra idé tooverify körs:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Ta bort etableringen hello system.

    Avetablering hello system tooclean den och gör den lämplig för reprovisioning. hello följande kommando tas även bort hello senaste etablerade användarkontot och hello associerade data:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nu kan du stänga av den virtuella datorn.

## <a name="step-2-create-a-storage-account-in-azure"></a>Steg 2: Skapa ett lagringskonto i Azure
Du behöver ett lagringskonto i Azure tooupload en VHD-fil så att den kan vara används toocreate en virtuell dator. Du kan använda hello Azure klassiska portal toocreate ett lagringskonto.

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Markera på hello kommandofältet **ny**.
3. Välj **datatjänster** > **lagring** > **Snabbregistrering**.

    ![Snabbt skapa ett lagringskonto](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. Fyll i hello fält på följande sätt:

   * I hello **URL** skriver du en underdomän namn toouse i URL: en för hello storage-konto. hello posten kan innehålla 3 till 24 siffror och gemener. Det här namnet blir hello värdnamn i hello-URL som används tooaddress Azure Blob storage, Azure Queue storage eller Azure Table storage-resurser för hello prenumeration.
   * I hello **plats/Tillhörighetsgrupp** nedrullningsbara menyn, Välj hello **platsen eller tillhörighetsgruppen** för hello storage-konto. En tillhörighetsgrupp kan du placera dina molntjänster och lagring i hello samma datacenter.
   * I hello **replikering** fältet, besluta om toouse **Geo-Redundant** replikering för hello storage-konto. GEO-replikering är aktiverad som standard. Det här alternativet replikerar data tooa sekundär plats, på tooyou utan kostnad, så att lagringen växlar toothat platsen om ett allvarligt fel inträffar sker hello primär plats. hello sekundära platsen tilldelas automatiskt och kan inte ändras. Om du behöver mer kontroll över hello platsen för din molnbaserade lagringsenheter på grund av toolegal krav eller organisationens principer kan inaktivera du geo-replikering. Men tänk på att om du aktiverar senare geo-replikering kan du debiteras en gång dataöverföring avgift tooreplicate din befintliga data toohello sekundär plats. Lagringstjänster utan geo-replikering erbjuds med rabatt. Mer information om hur du hanterar geo-replikering av lagringskonton finns här: [Azure Storage-replikering](../../../storage/common/storage-redundancy.md).

     ![Ange information om lagringskonto](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. Välj **skapa Lagringskonto**. hello konto nu visas under **lagring**.

    ![Storage-konto har skapats](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. Skapa sedan en behållare för dina överförda VHD-filer. Välj hello lagringskontonamn och välj sedan **behållare**.

    ![Kontoinformation för lagring](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. Välj **skapa en behållare**.

    ![Kontoinformation för lagring](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. I hello **namnet** skriver du ett namn för din behållare. Sedan hello **åtkomst** listrutan väljer du vilken typ av princip.

    ![Behållarens namn](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > Som standard hello behållare är privat och kan bara användas av hello Kontoägare. tooallow offentlig läsbehörighet toohello blobbar i behållaren hello, men inte toohello behållaregenskaperna och metadata, använda hello **offentlig Blob** alternativet. tooallow fullständig offentlig läsbehörighet för hello behållare och blobbar, använda hello **offentlig behållare** alternativet.
   >
   >

## <a name="step-3-prepare-hello-connection-tooazure"></a>Steg 3: Förbered hello anslutning tooAzure
Innan du kan ladda upp en VHD-fil, måste tooestablish en säker anslutning mellan datorn och din Azure-prenumeration. Du kan använda metoden för hello Azure Active Directory (AD Azure) eller hello certifikat metoden toodo den.

### <a name="use-hello-azure-ad-method-tooupload-a-vhd-file"></a>Använd hello Azure AD-metoden tooupload en VHD-fil
1. Öppna hello Azure PowerShell-konsolen.
2. Skriv följande kommando hello:  
    `Add-AzureAccount`

    Det här kommandot öppnas en inloggning där du kan logga in med ditt arbets- eller skolkonto.

    ![PowerShell-fönster](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. Azure autentiserar och sparar hello autentiseringsuppgifter. Sedan hello fönstret stängs.

### <a name="use-hello-certificate-method-tooupload-a-vhd-file"></a>Använd hello certifikat metoden tooupload en VHD-fil
1. Öppna hello Azure PowerShell-konsolen.
2. Typ: `Get-AzurePublishSettingsFile`.
3. Ett fönster i webbläsaren öppnas och uppmanar du toodownload hello .publishsettings-fil. Den här filen innehåller information och ett certifikat för din Azure-prenumeration.

    ![Hämtningssidan för webbläsare](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. Spara hello .publishsettings-fil.
5. Typ: `Import-AzurePublishSettingsFile <PathToFile>`, där `<PathToFile>` är hello fullständig sökväg toohello .publishsettings-fil.

   Mer information finns i [Kom igång med Azure-cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Mer information om hur du installerar och konfigurerar PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="step-4-upload-hello-vhd-file"></a>Steg 4: Överför hello VHD-filen
När du överför hello VHD-filen placerar du den någonstans i Blob storage. Följande är vissa termer som du ska använda när du överför hello-filen:

* **BlobStorageURL** är hello URL för hello storage-konto som du skapade i steg 2.
* **YourImagesFolder** är hello behållare i Blob storage där toostore bilderna.
* **VHDName** är hello etiketten som visas i hello Azure klassiska portal tooidentify hello virtuell hårddisk.
* **PathToVHDFile** hello sökvägen och namnet på hello VHD-filen.

Du använde i föregående steg i hello hello Azure PowerShell-fönster, skriv:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-hello-uploaded-vhd-file"></a>Steg 5: Skapa en virtuell dator med hello överförda VHD-filen
När du har överfört hello VHD-filen kan du lägga till som en bild toohello lista över anpassade avbildningar som är associerade med din prenumeration och skapa en virtuell dator med den här anpassade avbildningen.

1. Du använde i föregående steg i hello hello Azure PowerShell-fönster, skriv:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of hello VHD> -OS <Type of hello OS on hello VHD>

   > [!NOTE]
   > Använda Linux som hello OS-typen. hello nuvarande Azure PowerShell version accepterar endast ”Linux” eller ”Windows” som en parameter.
   >
   >
2. När du har slutfört föregående steg i hello hello ny avbildning visas när du väljer hello **bilder** fliken på hello klassiska Azure-portalen.  

    ![Välj en bild](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Skapa en virtuell dator från hello-galleriet. Den här nya avbildningen är nu tillgänglig under **Mina avbildningar**.
4. Välj hello ny avbildning. Gå sedan igenom hello prompter tooset ett värdnamn, lösenord, SSH-nyckeln och så vidare.

    ![Anpassad avbildning](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. När du har slutfört hello etablering ser du ditt FreeBSD VM körs i Azure.

    ![FreeBSD-avbildning i azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
