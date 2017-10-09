---
title: "aaaPortal prep för virtuell StorSimple-matrisen | Microsoft Docs"
description: "Första kursen toodeploy virtuella StorSimple-matris innebär att förbereda hello Azure-portalen"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5332b235e7296a9274f2e7dafcdf72f4b9cdadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-hello-azure-portal"></a>Distribuera StorSimple virtuell matris – förbereda hello Azure-portalen

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Översikt

Detta är hello första artikeln i hello serie självstudier för distribution krävs toocompletely distribuera virtuella matrisen som en filserver eller en iSCSI-server som använder hello Resource Manager-modellen. Den här artikeln beskriver hello förberedelser krävs toocreate och konfigurera din StorSimple Enhetshanteraren service tidigare tooprovisioning virtuella matris. Den här artikeln innehåller också länkar ut tooa configuration checklista och konfiguration kraven för distribution.

Du måste administratören privilegier toocomplete hello installationen och konfigurationen processen. Vi rekommenderar att du granskar hello checklista för distributionskonfiguration innan du börjar. hello portal förberedelse tar mindre än 10 minuter.

hello information som publicerats i den här artikeln gäller toohello distribution av virtuella StorSimple-matriser i hello Azure-portalen och Microsoft Azure Government-molnet.

### <a name="get-started"></a>Kom igång
arbetsflöde för distribution av hello består av förbereda hello portal, etablera en virtuell matris i din virtualiserade miljö och hello-installationen har slutförts. tooget igång med hello virtuella StorSimple-matris distribution som en filserver eller en iSCSI-server, måste toorefer toohello följande tabellform resurser.

#### <a name="deployment-articles"></a>Artiklar om distribution

toodeploy din virtuella StorSimple-matris finns toohello följande artiklar i hello föreskrivs sekvens.

| **#** | **I det här steget** | **Det gör du...** | **Och använda dessa dokument.** |
| --- | --- | --- | --- |
| 1. |**Ställ in hello Azure-portalen** |Skapa och konfigurera din StorSimple Enhetshanteraren service tidigare tooprovisioning en virtuell StorSimple-matris. |[Förbereda hello-portalen](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Etablera hello virtuella matris** |Etablera för Hyper-V och Anslut tooa virtuella StorSimple-matris på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2. <br></br> <br></br> Etablera för VMware, och Anslut tooa virtuella StorSimple-matris på ett värdsystem som kör VMware ESXi 5.5 och senare.<br></br> |[Etablera en virtuell matris i Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [Etablera en virtuell VMware-matris](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Ställ in hello virtuella matris** |Utföra installationen för din filserver, registrera din StorSimple-filserver och slutför hello Enhetsinställningar. Du kan sedan etablera SMB-resurser. <br></br> <br></br> Utföra installationen för iSCSI-server, registrera StorSimple iSCSI-servern och slutför hello Enhetsinställningar. Du kan sedan etablera iSCSI-volymer. |[Konfigurera virtuella matris som filserver](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Konfigurera virtuella matris som iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Nu kan du börja tooset in hello Azure-portalen.

## <a name="configuration-checklist"></a>Checklista för konfiguration

Checklista för konfiguration av hello beskriver hello-information som du behöver toocollect innan du konfigurerar hello programvara på din virtuella StorSimple-matrisen. Förbereder den här informationen i tid hjälper förenklad hello processen för att distribuera hello StorSimple-enheten i din miljö. Beroende på om din virtuella StorSimple-matris distribueras som en filserver eller en iSCSI-server, behöver du något av följande checklistor hello.

* Hämta hello [StorSimple virtuell matris File Server checklista för distributionskonfiguration](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Hämta hello [virtuella StorSimple-matris iSCSI checklista för konfiguration av servern](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Krav

Här hittar du hello konfigurationskraven för din StorSimple Enhetshanteraren tjänst din virtuella StorSimple-matris och hello datacenternätverket.

### <a name="for-hello-storsimple-device-manager-service"></a>För hello StorSimple enheten Manager-tjänsten

Innan du börjar bör du kontrollera att:

* Du har ditt Microsoft-konto med autentiseringsuppgifter.
* Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.
* Microsoft Azure-prenumerationen ska aktiveras för StorSimple enheten Manager-tjänsten.

### <a name="for-hello-storsimple-virtual-array"></a>För hello virtuella StorSimple-matris

Innan du distribuerar en virtuell matris, kontrollerar du att:

* Du har åtkomst tooa värdsystem som kör Hyper-V på Windows Server 2008 R2 eller senare eller VMware (ESXi 5.5 eller senare) som kan använda tooa etablera en enhet.
* hello värdsystem är kan toodedicate hello följande resurser tooprovision virtuella matrisen:
  
  * Minst 4 kärnor.
  * Minst 8 GB RAM-minne. Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB 2 miljoner filer. Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.
  * Ett nätverksgränssnitt.
  * En virtuell disk 500 GB efter systemdata.

### <a name="for-hello-datacenter-network"></a>För hello datacenternätverket

Innan du börjar bör du kontrollera att:

* hello nätverket i datacentret har konfigurerats enligt hello nätverkskrav för din StorSimple-enhet. Mer information finns i hello [StorSimple virtuell matris systemkrav](storsimple-ova-system-requirements.md).
* Din virtuella StorSimple-matris har en dedikerad 5 Mbit/s internetbandbredd (eller mer) vid alla tidpunkter. Den här bandbredden ska inte delas med andra program.

## <a name="step-by-step-preparation"></a>Förberedelse av steg

Använd hello följa instruktionerna tooprepare din portal för hello StorSimple enheten Manager-tjänsten.

## <a name="step-1-create-a-new-service"></a>Steg 1: Skapa en ny tjänst

En instans av hello StorSimple enheten Manager-tjänsten kan hantera flera virtuella StorSimple-matriser. Utföra hello följande steg toocreate en instans av hello StorSimple Device Manager-tjänsten. Om du har en befintlig StorSimple Enhetshanteraren service toomanage din virtuella matriser, hoppa över det här steget och gå för[steg 2: hämta hello nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst måste toocreate minst ett lagringskonto efter att du har skapat en tjänst.
> 
> * Om du inte har skapat ett lagringskonto automatiskt, gå för[konfigurera ett nytt lagringskonto för tjänsten hello](#optional-step-configure-a-new-storage-account-for-the-service) detaljerade anvisningar.
> * Om du har aktiverat hello automatiskt skapande av lagringskonton går för[steg 2: hämta hello nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>Steg 2: Hämta nyckel för tjänstregistrering hello

När hello StorSimple enheten Manager-tjänsten är igång kan behöver du tooget hello nyckel för tjänstregistrering. Den här nyckeln används tooregister och ansluta din StorSimple-enhet med hello-tjänsten.

Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/).

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> hello nyckel för tjänstregistrering är används tooregister alla hello Enhetshanteraren för StorSimple-enheter som behöver tooregister med din StorSimple Device Manager-tjänst.
> 
> 

## <a name="step-3-download-hello-virtual-array-image"></a>Steg 3: Hämta hello virtuella matris bild

När du har hello nyckel för tjänstregistrering måste toodownload hello lämpliga virtuella matris avbildningen tooprovision virtuella matrisen på värdsystemet. hello virtuella matris bilder är specifika för operativsystemet och kan hämtas från hello snabbstartsidan i hello Azure-portalen.

> [!IMPORTANT]
> hello-program som körs på hello virtuella StorSimple-matrisen får endast användas med hello StorSimple enheten Manager-tjänsten.
> 
> 

Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/).

#### <a name="tooget-hello-virtual-array-image"></a>tooget hello virtuella matris bild

1. Logga in på hello [Azure-portalen](https://portal.azure.com/). 
2. I hello Azure-portalen klickar du på **Bläddra > StorSimple enhetshanterare**.
3. Välj en befintlig StorSimple Device Manager-tjänst. I hello **StorSimple Enhetshanteraren** bladet, klickar du på **Snabbstart**. 
4. Klicka på hello länken motsvarande toohello bild som du vill toodownload från hello Microsoft Download Center. hello bildfiler är ungefär 4,8 GB.
   
   * VHDX för Hyper-V på Windows Server 2012 och senare
   * VHD för Hyper-V på Windows Server 2008 R2 och senare
   * VMDK för VMWare ESXi 5.5 och senare
5. Hämta och packa upp hello filen tooa lokal enhet, och ett meddelande om där hello uppackade filen finns.

## <a name="optional-step-configure-a-new-storage-account-for-hello-service"></a>Valfritt steg: konfigurera ett nytt lagringskonto för hello-tjänsten

Det här steget är valfritt och bör bara genomföras om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst.

Om du behöver toocreate ett Azure storage-konto i en annan region finns [hur toocreate ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) stegvisa instruktioner.

Utför följande steg i hello hello [Azure-portalen](https://ms.portal.azure.com/) på hello StorSimple Enhetshanteraren service sidan tooadd ett befintligt Microsoft Azure-lagringskonto.

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a>tooadd storage-konto autentiseringsuppgifter som har hello samma Azure-prenumeration som hello Enhetshanteraren tjänst

1. Navigera tooyour Device Manager-tjänsten, väljer och dubbelklicka på den. Då öppnas hello **översikt** bladet.
2. Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt.
3. Klicka på **Lägg till**.
4. I hello **Lägg till ett lagringskonto** bladet hello följande:
   
    1. För **prenumeration**väljer **aktuella**.
   
    2. Ange hello namnet på ditt Azure storage-konto.
   
    3. Välj **aktivera** toocreate en säker kanal för nätverkskommunikation mellan din StorSimple-enhet och hello moln. Välj **inaktivera** endast om du arbetar inom ett privat moln.
   
    4. Klicka på **Lägg till**. Du meddelas när hello storage-konto har skapats.<br></br>
   
     ![Lägg till en befintlig lagring konto autentiseringsuppgift](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Nästa steg

hello nästa steg är tooprovision en virtuell dator för din virtuella StorSimple-matris. Beroende på värdoperativsystemet finns hello detaljerade instruktioner i:

* [Etablera en virtuell StorSimple-matris i Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [Etablera en virtuell StorSimple-matris på VMware](storsimple-virtual-array-deploy2-provision-vmware.md)

