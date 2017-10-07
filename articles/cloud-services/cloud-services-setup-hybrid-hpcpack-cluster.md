---
title: aaaSet upp en hybrid HPC Pack kluster i Azure | Microsoft Docs
description: "Lär dig hur toouse Microsoft HPC Pack och Azure tooset upp en liten, hybrid högpresterande databehandling HPC-kluster"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 5ad30d78dcd0c6a95d2a61f25015232edab3563c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Skapa en hybrid med höga prestanda HPC-kluster med Microsoft HPC Pack och på begäran Azure compute-noder
Använd Microsoft HPC Pack 2012 R2 och Azure tooset upp en liten, hybrid med höga prestanda HPC-kluster. hello klustret visas i den här artikeln består av en lokal HPC Pack huvudnod och vissa compute-noder som du distribuerar på begäran i ett Azure-Molntjänsten. Du kan sedan köra beräkning jobb på hello hybrid klustret.

![Hybrid HPC-kluster][Overview] 

Detta kallas ibland kursen visar en metod klustret ”burst toohello moln”, toouse skalbar, på begäran Azure-resurser toorun beräknings-intensiva program.

Den här kursen förutsätter några tidigare erfarenheter med beräkningskluster eller HPC Pack. Det är avsedd endast toohelp som du distribuerar ett hybrid beräkningskluster snabbt i exempelsyfte. Mer information och steg toodeploy hybrid HPC Pack kluster i större skala i en produktionsmiljö eller toouse HPC Pack 2016 finns hello [detaljerad vägledning](http://go.microsoft.com/fwlink/p/?LinkID=200493). För andra scenarier med HPC Pack, inklusive automatisk Klusterdistribution i virtuella Azure-datorer, se [HPC-klusteralternativ with Microsoft HPC Pack i Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Krav
* **Azure-prenumeration** -om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.
* **En lokal dator som kör Windows Server 2012 R2 eller Windows Server 2012** -använder den här datorn som hello huvudnod hello HPC-kluster. Om du inte redan kör Windows Server, kan du hämta och installera en [utvärderingsversion](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * hello datorn vara domänansluten tooan Active Directory-domän. Du kan konfigurera hello huvudnod dator för testning, som en domänkontrollant. tooadd hello serverrollen Active Directory Domain Services och främja hello huvudnod datorn som en domänkontrollant, hello i dokumentationen för Windows Server.
  * toosupport HPC Pack hello operativsystemet måste installeras i ett av dessa språk: engelska, japanska eller kinesiska (förenklad).
  * Kontrollera att viktiga och viktiga uppdateringar har installerats.
* **HPC Pack 2012 R2** - [hämta](http://go.microsoft.com/fwlink/p/?linkid=328024) hello installationspaket för hello senaste versionen utan kostnad och kopiera hello filer toohello huvudnod datorn. Välj installationsfilerna i hello samma språk som din installation av Windows Server.

    >[!NOTE]
    > Om du vill toouse HPC Pack 2016 i stället för HPC Pack 2012 R2, krävs ytterligare konfiguration. Se hello [detaljerad vägledning](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Domänkonto** -kontot måste vara konfigurerad med behörighet som lokal administratör på hello huvudnod tooinstall HPC Pack.
* **TCP-anslutningar på port 443** från hello huvudnod tooAzure.

## <a name="install-hpc-pack-on-hello-head-node"></a>Installera HPC Pack på hello huvudnod
Du först installera Microsoft HPC Pack på din lokala dator som kör Windows Server. Den här datorn blir hello huvudnod hello-klustret.

1. Logga in toohello huvudnod genom att använda ett domänkonto som har lokal administratörsbehörighet.

2. Starta hello HPC Pack installationsguiden genom att köra Setup.exe från hello HPC Pack installationsfiler.

3. På hello **installation av HPC Pack 2012 R2** klickar du på **ny installation eller Lägg till nya funktioner tooan befintlig installation**.

    ![HPC Pack 2012 installationen][install_hpc1]

4. På hello **användaravtal för Microsoft-programvara sidan**, klickar du på **nästa**.

5. På hello **Välj installationstyp** klickar du på **skapar ett nytt HPC-kluster genom att skapa en huvudnod**, och klicka sedan på **nästa**.

6. hello guiden körs flera förinstallation tester. Klicka på **nästa** på hello **installationsregler** om alla tester godkänns. Annars granskar du informationen hello och göra ändringar i din miljö. Kör sedan hello tester igen eller om nödvändigt start hello installationsguiden igen.
7. På hello **HPC DB Configuration** Kontrollera **huvudnod** har valts för alla HPC-databaser och klicka sedan på **nästa**. 

    ![DB-konfiguration][install_hpc4]

8. Standardinställningarna på hello återstående sidor i guiden hello. På hello **installera nödvändiga komponenter** klickar du på **installera**.
   
    ![Installera][install_hpc6]

9. När hello installationen är klar, avmarkera **starta HPC Cluster Manager** och klicka sedan på **Slutför**. (Du startar HPC Cluster Manager i ett senare steg.)
   
    ![Slutför][install_hpc7]

## <a name="prepare-hello-azure-subscription"></a>Förbereda hello Azure-prenumeration
Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com) med din Azure-prenumeration. När du har slutfört de här stegen kan du distribuera Azure-noder från hello lokalt huvudnod. 
  
  > [!NOTE]
  > Även anteckna ditt Azure prenumerations-ID som du behöver senare. Hitta hello-ID i **prenumerationer** hello-portalen.
  > 

### <a name="upload-hello-default-management-certificate"></a>Överför hello standardhanteringscertifikat
HPC Pack installerar ett självsignerat certifikat på hello huvudnod, kallas hello standard Microsoft HPC Azure Management-certifikat som du kan ladda upp som ett Azure-hanteringscertifikat. Det här certifikatet tillhandahålls för testning och proof of concept distributioner toosecure hello anslutning mellan hello huvudnod och Azure.

1. Från hello huvudnod dator, logga in toohello [Azure-portalen](https://portal.azure.com).

2. Klicka på **prenumerationer** > *your_subscription_name*.

3. Klicka på **hanteringscertifikat** > **överför**.4. Bläddra i hello huvudnod för hello filen C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer. Klicka på **överför**.

   
Hej **standard HPC Azure Management** certifikatet visas i hello listan över hanteringscertifikat.

### <a name="create-an-azure-cloud-service"></a>Skapa en Azure-molntjänst
> [!NOTE]
> För bästa prestanda bör du skapa hello Molntjänsten och hello storage-konto (i ett senare steg) i hello samma geografiska region.
> 
> 

1. I hello-portalen klickar du på **molntjänster (klassisk)** > **+ Lägg till**.

2.  Ange ett DNS-namn för hello service, välja en resursgrupp och en plats och klicka sedan på **skapa**.


### <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto
1. I hello-portalen klickar du på **Storage-konton (klassiska)** > **+ Lägg till**.

2. Ange ett namn för hello kontot och välj hello **klassiska** distributionsmodell.

3. Välj en resursgrupp och en plats och låta andra inställningar till standardvärden. Klicka sedan på **Skapa**.

## <a name="configure-hello-head-node"></a>Konfigurera hello huvudnod
toouse HPC Cluster Manager toodeploy Azure-noder och toosubmit jobb först göra konfigurationssteg krävs för klustret.

1. Starta HPC Cluster Manager på hello huvudnod. Om hello **Välj huvudnod** dialogrutan visas klickar du på **lokal dator**. Hej **distribution uppgiftslista** visas.

2. Under **krävs distributionsuppgifter**, klickar du på **konfigurera nätverket**.
   
    ![Konfigurera nätverket][config_hpc2]

3. Välj i hello guiden för konfiguration av nätverk, **alla noder i ett företagsnätverk endast** (topologi 5). Den här konfigurationen är hello enklaste i exempelsyfte.
   
    ![Topologi 5][config_hpc3]
   
4. Klicka på **nästa** tooaccept standardvärdena på hello återstående sidor i guiden hello. Klicka sedan på hello **granska** klickar du på **konfigurera** toocomplete hello nätverkskonfigurationen.

5. I hello **distribution uppgiftslista**, klickar du på **ange autentiseringsuppgifter för installation**.

6. I hello **autentiseringsuppgifter** dialogrutan Skriv hello autentiseringsuppgifter för hello domänkonto som du använde tooinstall HPC Pack. Klicka sedan på **OK**. 
   
    ![Installation av autentiseringsuppgifter][config_hpc6]
   
7. I hello **distribution uppgiftslista**, klickar du på **konfigurera hello namngivning av nya noder**.

8. I hello **ange nod Naming serien** dialogrutan godkänner hello standardbaserad namngivningskontext serien och klickar på **OK**. Slutför det här steget även om hello Azure noder som du lägger till i den här självstudiekursen namnges automatiskt.
   
    ![Namngivning av nod][config_hpc8]
   
9. I hello **distribution uppgiftslista**, klickar du på **skapar en mall för noden**. Senare i hello självstudiekursen använda hello nod mallen tooadd Azure-noder toohello kluster.

10. Hej i guiden Skapa nod, göra hello följande:
    
    a. På hello **välja mallen nodtypen** klickar du på **nodmallen för Windows Azure**, och klicka sedan på **nästa**.
    
    ![Noden mall][config_hpc10]
    
    b. Klicka på **nästa** tooaccept hello standard mallnamn.
    
    c. På hello **ange prenumerationsinformation** anger Azure-prenumeration-ID: T (tillgänglig i din Azure kontoinformation). I **hanteringscertifikat**, bläddra efter **standard Microsoft HPC Azure Management.** Klicka sedan på **Nästa**.
    
    ![Noden mall][config_hpc12]
    
    d. På hello **ange tjänstinformation** sidan, Välj hello Molntjänsten och hello storage-konto som du skapade i föregående steg. Klicka sedan på **Nästa**.
    
    ![Noden mall][config_hpc13]
    
    e. Klicka på **nästa** tooaccept standardvärdena på hello återstående sidor i guiden hello. Klicka sedan på hello **granska** klickar du på **skapa** toocreate hello nodmallen.
    
    > [!NOTE]
    > Som standard hello Azure nodmallen innehåller inställningar för toostart (tillhandahålla) och stoppa hello noder manuellt, använder HPC Cluster Manager. Du kan också konfigurera ett schema toostart och stoppa hello Azure-noder automatiskt.
    > 
    > 

## <a name="add-azure-nodes-toohello-cluster"></a>Lägga till Azure-noder toohello kluster
Nu använda hello nod mallen tooadd Azure-noder toohello kluster. Lägga till hello noder toohello klustret lagrar konfigurationsinformation om deras så att du kan starta (tillhandahålla) dem när som helst i hello-Molntjänsten. Din prenumeration hämtar endast debiteras för Azure-noder när hello-instanser körs i hello-Molntjänsten.

Följ dessa steg tooadd två Small noder.

1. HPC Cluster Manager klickar du på **hantering** (kallas **resurshantering** i aktuella versioner av HPC Pack) > **Lägg till nod**.
   
    ![Lägg till nod][add_node1]

2. I hello guiden för Lägg till nod på hello **väljer distributionsmetoden** klickar du på **lägga till Windows Azure-noder**, och klicka sedan på **nästa**.
   
    ![Lägg till Azure nod][add_node1_1]

3. På hello **ange nya noder** sidan, Välj hello Azure nod mall du skapade tidigare (kallas som standard **AzureNode standardmallen**). Ange **2** noder i storlek **små**, och klicka sedan på **nästa**.
   
    ![Ange noder][add_node2]
   
4. På hello **Slutför hello guiden Lägg till nod** klickar du på **Slutför**.
    
     Två Azure noder med namnet **AzureCN 0001** och **AzureCN 0002**, visas nu i HPC Cluster Manager. Båda värdena i hello **inte distribuerade** tillstånd.
   
    ![Tillagda noder][add_node3]

## <a name="start-hello-azure-nodes"></a>Starta hello Azure-noder
När du vill toouse hello klusterresurser i Azure, Använd HPC Cluster Manager toostart (tillhandahålla) hello Azure-noder och tar dem online.

1. HPC Cluster Manager klickar du på **hantering** (kallas **resurshantering** i aktuella versioner av HPC Pack), och välj hello Azure-noder.

2. Klicka på **starta**, och klicka sedan på **OK**.
   
   ![Starta noder][add_node4]
   
    hello noder övergång toohello **etablering** tillstånd. Visa hello etablering loggen tootrack hello etablering pågår.
   
    ![Etablera noder][add_node6]

3. Efter några minuter hello Azure-noder Slutför etablering och är i hello **Offline** tillstånd. I det här tillståndet hello rollinstanser kör men ännu Acceptera inte klustret jobb.

4. tooconfirm som hello rollinstanser körs i hello Azure-portalen klickar du på **molntjänster (klassisk)** > *your_cloud_service_name*.
   
   Du bör se två **HpcWorkerRole** instanser (noder) som körs i hello-tjänsten. HPC Pack också automatiskt distribuerar två **HpcProxy** instanser (storlek medel) toohandle kommunikation mellan hello huvudnod och Azure.

   ![Kör instanser][view_instances1]

5. toobring hello Azure-noder online toorun kluster-jobb, Välj hello noder, högerklicka och klicka sedan på **Anslut**.
   
    ![Offline-noder][add_node7]
   
    HPC Cluster Manager visar att hello noder i hello **Online** tillstånd.

## <a name="run-a-command-across-hello-cluster"></a>Köra ett kommando över hello kluster
toocheck hello installation, användning hello HPC Pack **clusrun** kommandot toorun ett kommando eller ett program på en eller flera klusternoder. Ett enkelt exempel använder **clusrun** tooget hello IP-konfiguration hello Azure-noder.

1. Öppna en kommandotolk som administratör på hello huvudnod.

2. Skriv följande kommando hello:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Om du uppmanas ange administratörslösenordet klustret. Du bör se utdata liknande toohello följande för kommandot.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Kör ett testjobb
Nu skicka ett testjobb som körs på hello hybrid klustret. Det här exemplet är ett enkelt parametrisk rensning jobb (en typ av filsystemen parallella beräkning). Det här exemplet körs underaktiviteter som lägger till ett heltal tooitself med hello **ange /a** kommando. Alla hello-noder i klustret hello bidrar toofinishing hello underaktiviteter för heltal från 1 too100.

1. HPC Cluster Manager klickar du på **jobbhantering** > **nytt parametrisk Sopa jobb**.
   
    ![Nytt jobb][test_job1]

2. I hello **nytt parametrisk Sopa jobb** i dialogrutan **kommandoraden**, typen `set /a *+*` (skriva över hello standardkommandoraden som visas). Lämna standardvärdena för hello återstående inställningar och klickar sedan på **skicka** toosubmit hello jobb.
   
    ![Parametrisk rensning][param_sweep1]

3. När hello jobbet är klart, dubbelklickar du på hello **Mina Sopa uppgiften** jobb.

4. Klicka på **uppgiftsvyn**, och klicka sedan på en delaktivitet tooview hello beräknade utdata på samma nivå.
   
    ![Uppgiften resultat][view_job361]

5. toosee vilken nod som utförs hello beräkning för den underaktiviteten klickar du på **allokerade noder**. (Klustret kan visa en annan nod-namn).
   
    ![Uppgiften resultat][view_job362]

## <a name="stop-hello-azure-nodes"></a>Stoppa hello Azure-noder
Stoppa hello Azure-noder tooavoid onödiga kostnader tooyour konto när du testa hello klustret. Det här steget stoppar hello Molntjänsten och tar bort hello Azure rollinstanser.

1. HPC Cluster Manager i **hantering** (kallas **resurshantering** i tidigare versioner av HPC Pack), Välj både Azure-noder. Klicka på **stoppa**.
   
    ![Stoppa noder][stop_node1]

2. I hello **stoppar Windows Azure-noder** dialogrutan klickar du på **stoppa**. 
   
3. hello noder övergång toohello **stoppar** tillstånd. Efter några minuter HPC Cluster Manager visar att hello noder är **inte distribuerade**.
   
    ![Inte distribuerad noder][stop_node4]

4. tooconfirm hello rollinstanser körs inte längre i Azure, i hello Azure klickar du på **molntjänster (klassisk)** > *your_cloud_service_name*. Inga instanser har distribuerats i hello-produktionsmiljö. 
   
    Detta avslutar hello kursen.

## <a name="next-steps"></a>Nästa steg
* Utforska hello dokumentationen för [HPC Pack](https://technet.microsoft.com/library/cc514029).
* tooset upp en hybrid HPC Pack Klusterdistribution i större skala, se [Burst tooAzure Worker Rollinstanser with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).
* Andra sätt toocreate ett HPC Pack kluster i Azure, inklusive med Azure Resource Manager-mallar finns i [HPC-klusteralternativ with Microsoft HPC Pack i Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
