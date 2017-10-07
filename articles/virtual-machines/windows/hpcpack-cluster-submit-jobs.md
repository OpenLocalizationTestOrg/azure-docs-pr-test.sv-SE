---
title: aaaSubmit jobb tooan HPC Pack kluster i Azure | Microsoft Docs
description: "Lär dig hur tooset upp en lokal dator toosubmit jobb tooan HPC Pack kluster i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 2918cf633917d8730487152e6a5ddb863eb8bb5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-tooan-hpc-pack-cluster-deployed-in-azure"></a>Skicka HPC-jobb från en lokal dator tooan HPC Pack kluster distribuerade i Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Konfigurera en lokal klient datorn toosubmit jobb tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kluster i Azure. Den här artikeln visar hur tooset upp en lokal dator med klienten verktygen toosubmit jobb via HTTPS toohello kluster i Azure. På så sätt kan flera kluster användarna kan skicka jobb tooa molnbaserade HPC Pack kluster, men utan att ansluta direkt toohello huvudnod VM eller få åtkomst till en Azure-prenumeration.

![Skicka ett jobb tooa kluster i Azure][jobsubmit]

## <a name="prerequisites"></a>Krav
* **HPC Pack huvudnod distribueras i en Azure VM** -rekommenderar vi att du använder automatiserade verktyg som en [Azure quickstart mallen](https://azure.microsoft.com/documentation/templates/) eller en [Azure PowerShell-skript](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello huvudnod och kluster. Du behöver hello DNS-namnet på hello huvudnod och hello autentiseringsuppgifterna för en Klusteradministratör för att slutföra hello stegen i den här artikeln.
* **Klientdatorn** -du behöver en Windows- eller Windows Server-klientdator som kan köra HPC Pack klientverktyg (se [systemkrav](https://technet.microsoft.com/library/dn535781.aspx)). Om du bara vill toouse hello HPC Pack webbportal eller REST API toosubmit jobb kan du använda alla klientdatorer som du väljer.
* **HPC Pack installationsmediet** -tooinstall hello HPC Pack klientverktyg, hello ledigt installationspaketet för den senaste versionen av HPC Pack (HPC Pack 2012 R2) är tillgängliga från den [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Kontrollera att du hämtar hello samma version av HPC Pack som är installerad på hello huvudnod VM.

## <a name="step-1-install-and-configure-hello-web-components-on-hello-head-node"></a>Steg 1: Installera och konfigurera hello Webbkomponenter på hello huvudnod
tooenable ett REST-gränssnittet toosubmit jobb toohello kluster via HTTPS, se till att hello HPC Pack Webbkomponenter konfigureras i hello HPC Pack huvudnod. Om de inte redan har installerats måste du först installera hello webbkomponenterna genom att köra hello HpcWebComponents.msi installationsfilen. Konfigurera sedan hello komponenter genom att köra hello HPC PowerShell-skript **Set HPCWebComponents.ps1**.

Detaljerade anvisningar finns [installera hello Microsoft HPC Pack Webbkomponenter](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> Vissa mallar för Azure quickstart för HPC Pack installera och konfigurera hello Webbkomponenter automatiskt. Om du använder hello [HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello klustret, du kan alternativt installera och konfigurera hello webbkomponenter som en del av hello distribution.
> 
> 

**tooinstall hello Webbkomponenter**

1. Anslut toohello huvudnod VM via hello autentiseringsuppgifterna för en Klusteradministratör.
2. Kör HpcWebComponents.msi från hello HPC Pack installationsmapp, i hello huvudnod.
3. Följ stegen i hello i hello guiden tooinstall hello Webbkomponenter

**tooconfigure hello Webbkomponenter**

1. Starta HPC PowerShell som administratör på hello huvudnod.
2. toochange toohello katalogplatsen för hello konfigurationsskript, typen hello följande kommando:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. tooconfigure hello REST-gränssnittet och starta hello HPC-webbtjänsten, Skriv hello följande kommando:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. När begärd tooselect ett certifikat väljer hello certifikat motsvarar som toohello offentliga DNS-namnet på hello huvudnod. Till exempel om du distribuerar hello huvudnod VM som använder hello klassiska distributionsmodellen, certifikatnamnet hello ser ut som CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Om du använder hello Resource Manager-distributionsmodellen hello certifikatnamn ser ut som CN =&lt;*HeadNodeDnsName*&gt;.&lt; *region*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > Väljer du det här certifikatet senare när du skickar jobb toohello huvudnoden från en lokal dator. Inte markera eller konfigurera ett certifikat som motsvarar toohello datornamnet på hello huvudnod i hello Active Directory-domän (till exempel CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
5. tooconfigure hello webbportalen för jobbet, typen hello följande kommando:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. När hello skriptet har slutförts hello stoppa och starta om tjänsten för Rapportjobbschemaläggning HPC genom att skriva följande kommandon hello:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-hello-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Steg 2: Installera hello HPC Pack klientverktyg på en lokal dator
Om du vill tooinstall hello HPC Pack klientverktyg på datorn kan ladda ned installationsfilerna HPC Pack (fullständig installation) från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). När du börjar installationen hello väljer hello installationsalternativ för hello **HPC Pack klientverktyg**.

toouse hello HPC Pack klienten verktygen toosubmit jobb toohello huvudnod VM kan du även behöver tooexport ett certifikat från hello huvudnod och installera den på hello-klientdator. hello certifikatet måste vara i. CER-format.

**tooexport hello certifikat från hello huvudnod**

1. Lägg till hello certifikat snapin-modulen tooa Microsoft Management Console för hello lokalt datorkonto på hello huvudnod. Anvisningar för hur tooadd hello snapin-modulen finns [lägga till hello snapin-modulen Certifikat tooan MMC](https://technet.microsoft.com/library/cc754431.aspx).
2. Hello-konsolträdet, expandera **certifikat – lokal dator** > **personliga**, och klicka sedan på **certifikat**.
3. Hitta hello-certifikat som har konfigurerats för hello HPC Pack webbkomponenter i [steg 1: Installera och konfigurera hello Webbkomponenter på hello huvudnod](#step-1:-install-and-configure-the-web-components-on-the-head-node) (exempelvis, CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
4. Högerklicka på hello certifikatet och klicka på **alla aktiviteter** > **exportera**.
5. I hello guiden Exportera certifikat, klickar du på **nästa**, och kontrollera att **Nej, exportera inte privat nyckel för hello** är markerad.
6. Följ hello återstående steg av hello guiden tooexport hello certifikat i DER-kodad binärfil X.509 (. CER)-format.

**tooimport hello certifikatet på hello-klientdatorn**

1. Kopiera hello-certifikat som du exporterade från hello huvudnod tooa mapp på hello-klientdator.
2. Kör certmgr.msc på hello klientdatorn.
3. Expandera i Certifikathanteraren **certifikat – aktuell användare** > **betrodda rotcertifikatutfärdare**, högerklicka på **certifikat**, och sedan Klicka på **alla aktiviteter** > **importera**.
4. I hello guiden Importera certifikat klickar du på **nästa** och följ hello steg tooimport hello certifikat som du exporterade från hello huvudnod toohello betrodda rotcertifikatutfärdare.

> [!TIP]
> Du kan se en säkerhetsvarning, eftersom hello certifikatutfärdare på hello huvudnod kan inte identifiera hello klientdatorn. I testsyfte kan ignorera du denna varning och fullständig hello Importera certifikat.
> 
> 

## <a name="step-3-run-test-jobs-on-hello-cluster"></a>Steg 3: Kör testjobb på hello klustret
tooverify konfigurationen, försök jobb som körs på hello-kluster i Azure från hello lokal dator. Du kan till exempel använda HPC Pack GUI verktyg och kommandon på kommandoraden toosubmit jobb toohello klustret. Du kan också använda en webbaserad portal toosubmit jobb.

**toorun jobbet skicka kommandon på hello klientdator**

1. Starta Kommandotolken på en klientdator där hello HPC Pack klientverktyg är installerade.
2. Ange en Exempelkommando. Skriv till exempel toolist alla jobb på hello klustret ett kommando liknande tooone av följande, beroende på hello fullständiga DNS-namnet på hello huvudnod hello:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    eller
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Använd hello fullständiga DNS-namnet på hello huvudnod, inte hello IP-adress i hello scheduler-URL. Om du anger hello IP-adress, visas ett felmeddelande liknande för ”hello servercertifikat måste tooeither har en giltig kedja med förtroende eller toobe placeras i arkivet Betrodda rotcertifikatutfärdare för hello”.
   > 
   > 
3. När du uppmanas ange hello användarnamn (i form av hello &lt;DomainName&gt;\\&lt;användarnamn&gt;) och lösenord för hello HPC Klusteradministratören eller en annan användare i klustret som du har konfigurerat. Du kan välja toostore hello autentiseringsuppgifter lokalt för flera jobbåtgärder.
   
    En lista över jobb visas.

**toouse HPC Job Manager på hello klientdator**

1. Om du inte tidigare sparar autentiseringsuppgifter för domänen för en användare i klustret när du skickar in ett jobb, du kan lägga till hello autentiseringsuppgifter i Autentiseringshanteraren.
   
    a. Starta Autentiseringshanteraren i Kontrollpanelen på klientdatorn hello.
   
    b. Klicka på **Windows-autentiseringsuppgifter** > **lägga till en allmän autentiseringsuppgift**.
   
    c. Ange hello Internet-adress (till exempel https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler eller https://&lt;HeadNodeDnsName&gt;.&lt; region&gt;.cloudapp.azure.com/HpcScheduler), och hello användarnamn (&lt;DomainName&gt;\\&lt;användarnamn&gt;) och lösenord för hello Klusteradministratören eller någon annan kluster-användare som du har konfigurerat.
2. Starta HPC Job Manager på hello klientdatorn.
3. I hello **Välj huvudnod** dialogrutan, typen hello URL toohello huvudnod i Azure (till exempel https://&lt;HeadNodeDnsName&gt;. cloudapp.net eller https://&lt;HeadNodeDnsName&gt;. &lt;region&gt;. cloudapp.azure.com).
   
    HPC Job Manager öppnas och visar en lista över jobb på hello huvudnod.

**toouse hello webbportal som körs på hello huvudnod**

1. Starta en webbläsare på hello klientdatorn och ange något av följande adresser, beroende på hello fullständiga DNS-namnet på hello huvudnod hello:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    eller
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Ange hello domänautentiseringsuppgifter för hello HPC Klusteradministratören hello säkerhet dialogrutan som visas. (Du kan också lägga till andra användare i klustret i olika roller. Se [hantera klustret användare](https://technet.microsoft.com/library/ff919335.aspx).)
   
    hello webbportalen öppnas toohello jobbet listvy.
3. toosubmit ett exempel jobb som lämnar hello strängen ”Hello World” hello klustret, klicka på **nytt jobb** i hello vänstra navigeringsfönstret.
4. På hello **nytt jobb** sidan under **från skicka sidor**, klickar du på **HelloWorld**. hello jobbet Skicka sida visas.
5. Klicka på **skicka**. Om du uppmanas ange hello domänautentiseringsuppgifter för hello HPC Klusteradministratören. hello jobbet har skickats och hello jobb-ID som visas på hello **Mina jobb** sidan.
6. tooview hello resultaten av hello jobb som du har skickat klickar du på hello jobb-ID och klicka sedan på **uppgiftsvyn** tooview hello kommandoutdata (under **utdata**).

## <a name="next-steps"></a>Nästa steg
* Du kan också skicka jobb toohello Azure kluster med hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Om du vill toosubmit kluster-jobb från en Linux-klient, finns hello Python exempel i hello [HPC Pack 2012 R2 SDK och exempelkod](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
