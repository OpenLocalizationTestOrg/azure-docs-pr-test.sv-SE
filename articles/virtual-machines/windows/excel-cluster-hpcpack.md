---
title: "aaaHPC Pack kluster för Excel och SOA | Microsoft Docs"
description: "Kom igång storskaliga Excel- och SOA-arbetsbelastningar som körs i ett HPC Pack-kluster i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 55b4b2c25fe65d06b75025cc23c3c13b8b764238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Kom igång Excel- och SOA-arbetsbelastningar som körs i ett HPC Pack-kluster i Azure
Den här artikeln visar hur toodeploy Microsoft HPC Pack 2012 R2-kluster på virtuella Azure-datorer med hjälp av en mall för Azure quickstart eller alternativt en Azure PowerShell-distributionsskriptet. hello klustret använder Azure Marketplace VM bilder som har utformats för toorun Microsoft Excel eller tjänstorienterad arkitektur (SOA) arbetsbelastningar med HPC Pack. Du kan använda hello klustret toorun Excel HPC och SOA-tjänster från en klientdator för lokalt. hello Excel HPC tjänster omfattar arbetsboksavlastning i Excel och Excel användardefinierade funktioner eller UDF: er.

> [!IMPORTANT] 
> Den här artikeln baseras på funktioner, mallar och skript för HPC Pack 2012 R2. Det här scenariot stöds inte för närvarande i HPC Pack 2016.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

På en hög nivå hello följande diagram visar hello HPC Pack kluster du skapar.

![HPC-kluster med noder som kör Excel arbetsbelastningar][scenario]

## <a name="prerequisites"></a>Krav
* **Klientdatorn** -du behöver ett Windows-baserad klient datorn toosubmit exempel Excel- och SOA-jobb toohello kluster. Du måste också en Windows datorn toorun hello Azure PowerShell distributionsskriptet för klustret (om du väljer att distributionsmetoden).
* **Azure-prenumeration** -om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) på bara några minuter.
* **Kärnor kvoten** – du kan behöva tooincrease hello kvoten för kärnor, särskilt om du distribuerar flera klusternoder med flera kärnor VM-storlekar. Om du använder en mall för Azure quickstart är hello kärnor kvot i Resource Manager per Azure-region. I så fall kan behöva du tooincrease hello kvot i en viss region. Se [Azure-prenumeration gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md). tooincrease en kvot [öppna en supportbegäran online customer](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) utan kostnad.
* **Microsoft Office-licens** - om du distribuerar compute-noder som använder en Marketplace HPC Pack 2012 R2 VM-avbildning med Microsoft Excel, en 30-dagars utvärderingsversion av Microsoft Excel Professional Plus 2013 har installerats. När du hello utvärderingsperioden måste tooprovide en giltig Microsoft Office-licens tooactivate Excel toocontinue toorun arbetsbelastningar. Se [Excel aktivering](#excel-activation) senare i den här artikeln. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Steg 1. Konfigurera ett HPC Pack kluster i Azure
Visar vi två alternativ tooset in hello HPC Pack 2012 R2-kluster: första, med hjälp av en mall för Azure quickstart och hello Azure-portalen; och andra som använder ett skript för distribution av Azure PowerShell.

### <a name="option-1-use-a-quickstart-template"></a>Alternativ 1. Använd en mall för Snabbstart
Använd en mall för Azure quickstart-tooquickly distribuera ett HPC Pack kluster i hello Azure-portalen. När du öppnar hello mallen i hello-portalen kan du få ett enkelt gränssnitt där du kan ange hello inställningar för klustret. Här är hello åtgärder. 

> [!TIP]
> Om du vill använda en [Azure Marketplace mallen](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) som skapar ett liknande kluster specifikt för Excel-arbetsbelastningar. hello stegen skiljer sig från hello följande.
> 
> 

1. Besök hello [skapa HPC-kluster mallsida på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Om du vill granska information om hello mallen och hello källkoden.
2. Klicka på **distribuera tooAzure** toostart en distribution med hello mallen i hello Azure-portalen.
   
   ![Distribuera mallen tooAzure][github]
3. Följ dessa steg tooenter hello parametrar för hello HPC-kluster mallen i hello-portalen.
   
   a. På hello **parametrar** anger eller ändrar värdena för hello mallparametrar. (Klicka på hello ikonen nästa tooeach inställning för hjälp.) I hello följande skärm visas exempelvärden. Det här exemplet skapar ett kluster med namnet *hpc01* i hello *hpc.local* domän som består av en huvudnod och 2 compute-noder. hello compute-noder har skapats från en HPC Pack VM-avbildning som omfattar Microsoft Excel.
   
   ![Ange parametrar][parameters-new-portal]
   
   > [!NOTE]
   > hello huvudnod VM skapas automatiskt från hello [senaste Marketplace-avbildning](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) av HPC Pack 2012 R2 på Windows Server 2012 R2. För närvarande baseras hello på HPC Pack 2012 R2 uppdatering 3.
   > 
   > Compute-nod virtuella datorer skapas från hello senaste bild av hello valt beräkning nod familj. Välj hello **ComputeNodeWithExcel** alternativ för hello senaste HPC Pack compute-nod avbildning som omfattar en utvärderingsversion av Microsoft Excel Professional Plus 2013. toodeploy ett kluster för allmän SOA-sessioner eller Excel-UDF-avlastning, Välj hello **ComputeNode** alternativ (utan Excel installerat).
   > 
   > 
   
   b. Välj hello prenumeration.
   
   c. Skapa en resursgrupp för hello-kluster som *hpc01RG*.
   
   d. Välj en plats för hello resursgrupp, till exempel centrala USA.
   
   e. På hello **juridiska villkor** granskar hello villkoren. Om du godkänner villkoren klickar du på **inköp**. När du är klar inställningsvärden hello hello mallen Klicka **skapa**.
4. När hello distribution har slutförts (det tar vanligtvis cirka 30 minuter), exportera hello klustret certifikatfilen från hello klustrets huvudnod. I ett senare steg kan du importera den här offentliga certifikat på hello tooprovide hello serversidan klientdatorautentisering för säker HTTP-bindning.
   
   a. I hello Azure-portalen, går toohello instrumentpanelen, väljer hello huvudnod, och klickar på **Anslut** hello överst i hello sidan tooconnect med hjälp av fjärrskrivbord.
   
    <!-- ![Connect toohello head node][connect] -->
   
   b. Använd standard procedurer i Certifikathanteraren tooexport hello huvudnod certifikat (finns under Cert: \LocalMachine\My) utan hello privat nyckel. Exportera i det här exemplet *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Exportera hello certifikat][cert]

### <a name="option-2-use-hello-hpc-pack-iaas-deployment-script"></a>Alternativ 2. Använd hello HPC Pack IaaS distributionsskriptet
hello HPC Pack IaaS distributionsskriptet innehåller en annan flexibel sätt toodeploy ett HPC Pack-kluster. Den skapar ett kluster i hello klassiska distributionsmodellen medan hello mallen använder hello Azure Resource Manager-distributionsmodellen. Hello skript är också kompatibla med en prenumeration i Azure globala hello eller Azure Kina service.

**Ytterligare krav**

* **Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.
* **HPC Pack IaaS distributionsskriptet** – hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Kontrollera hello version av hello skriptet genom att köra `New-HPCIaaSCluster.ps1 –Version`. Den här artikeln är baserat på version 4.5.0 eller senare av hello skript.

**Skapa hello-konfigurationsfil**

 hello HPC Pack IaaS distributionsskriptet använder en XML-konfigurationsfil som indata som beskriver hello infrastruktur hello HPC-kluster. toodeploy ett kluster som består av en huvudnod och 18 compute-noder som skapas från hello compute-nod avbildning som omfattar Microsoft Excel Ersätt värdena för din miljö till hello följande exempel konfigurationsfil. Mer information om hello-konfigurationsfilen finns hello Manual.rtf fil i mappen för hello-skript och [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Information om hello-konfigurationsfil**

* Hej **VMName** av hello huvudnod **måste** hello vara samma som hello **ServiceName**, SOA-jobb misslyckas toorun.
* Kontrollera att du anger **EnableWebPortal** så att hello huvudnod certifikatet genereras och exporteras.
* hello-filen anger ett efterkonfiguration PowerShell-skript PostConfig.ps1 som körs på hello huvudnod. hello följande exempelskript konfigurerar hello anslutningssträngen för Azure storage, tar bort hello beräkningsrollen noden från hello huvudnod och ger alla noder online när de distribueras. 

```
    # add hello HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set hello Azure storage connection string for hello cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove hello compute node role for head node toomake sure hello Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in hello deployment including hello head node and compute nodes, which should match hello number specified in hello XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Kör hello-skript**

1. Öppna hello PowerShell-konsol på hello klientdatorn som administratör.
2. Ändra katalogmapp toohello skript (E:\IaaSClusterScript i det här exemplet).
   
   ```
   cd E:\IaaSClusterScript
   ```
3. toodeploy hello HPC Pack klustret, kör följande kommando hello. Det här exemplet förutsätter att hello-konfigurationsfilen finns i E:\HPCDemoConfig.xml.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

hello HPC Pack distributionsskriptet körs under en viss tid. En sak hello skriptet gör är tooexport och hämta hello klustret certifikatet och spara det i hello den aktuella användarens dokument-mappen på hello-klientdator. hello skriptet genererar ett meddelande liknande toohello följande. I följande steg ska importera du hello certifikat i certifikatarkivet för hello lämplig.    

    You have enabled REST API or web portal on HPC Pack head node. Please import hello following certificate in hello Trusted Root Certification Authorities certificate store on hello computer where you are submitting job or accessing hello HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Steg 2. Avlastning Excel-arbetsböcker och köra UDF: er från en lokal klient
### <a name="excel-activation"></a>Excel-aktivering
När du använder hello ComputeNodeWithExcel VM-avbildning för produktionsarbetsbelastningar måste tooprovide en giltig Microsoft Office-licens viktiga tooactivate Excel på hello compute-noder. Annars hello utvärderingsversionen av Microsoft Excel upphör att gälla efter 30 dagar och kör Excel-arbetsböcker misslyckas med hello COMException (0x800AC472). 

Du kan återställa Excel med ytterligare 30 dagar utvärdering tid: Logga in toohello huvudnod och clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` på alla Excel datornoder via HPC Cluster Manager. Du kan återställa högst två gånger. Därefter måste du ange en giltig licensnyckel Office.

hello Office Professional Plus 2013 har installerats på hello VM-avbildning är en volymutgåva med en allmän nyckel GVLK (Volume License). Du kan aktivera den via nyckelhanteringstjänsten (KMS) / Active Directory-Based aktivering (AD BA) eller fleraktiveringsnyckel (MAK). 

    * toouse KMS/AD-BA, Använd en befintlig KMS-server eller Ställ in en ny med hjälp av hello licenspaketet för Microsoft Office 2013 volym. (Om du vill ställa in hello server i hello huvudnod.) Aktivera sedan hello KMS-värdnyckeln via hello Internet eller per telefon. Sedan clusrun `ospp.vbs` tooset hello KMS-servern och porten och aktivera Office på alla hello Excel compute-noder. 

    * toouse MAK första clusrun `ospp.vbs` tooinput hello nyckeln och sedan aktivera alla hello Excel datornoderna via hello Internet eller per telefon. 

> [!NOTE]
> Retail produktnycklar för Office Professional Plus 2013 kan inte användas med den här Virtuella datorn. Om du har giltiga nycklar och installationsmediet för Office eller Excel-versioner än den här Office Professional Plus 2013 volym-versionen kan använda du dem i stället. Först avinstallera den här volymen-versionen och installera hello-version som du har. hello ominstalleras Excel compute-nod kan sparas som en anpassad VM avbildningen toouse i en distribution i stor skala.
> 
> 

### <a name="offload-excel-workbooks"></a>Avlastning Excel-arbetsböcker
Följ dessa steg toooffload en Excel-arbetsbok så att den körs på hello HPC Pack klustret i Azure. toodo, måste du ha Excel 2010 eller 2013 redan installerad på hello klientdatorn.

1. Använd en av hello alternativ i steg 1 toodeploy ett HPC Pack kluster med hello Excel compute-nod avbildningen. Hämta hello klustret certifikatfil (.cer) och klustret användarnamn och lösenord.
2. Importera hello klustret certifikat under Cert: \CurrentUser\Root på hello-klientdatorn.
3. Kontrollera att Excel har installerats. Skapa en Excel.exe.config-fil med följande innehåll i hello hello samma mapp som Excel.exe på hello-klientdator. Det här steget säkerställer att hello HPC Pack 2012 R2 Excel COM-tillägg har lästs in.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. Ställ in hello klient toosubmit jobb toohello HPC Pack kluster. Ett alternativ är toodownload hello fullständig [HPC Pack 2012 R2 uppdatering 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) och installera hello HPC Pack-klienten. Du kan också hämta och installera hello [HPC Pack 2012 R2 uppdatering 3 klientverktyg](https://www.microsoft.com/download/details.aspx?id=49923) och hello lämpligt Visual C++ 2010 redistributable för datorn ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).
5. Vi använder en exempel Excel-arbetsbok med namnet ConvertiblePricing_Complete.xlsb i det här exemplet. Du kan ladda ned det [här](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Kopiera arbetsmappen för hello Excel-arbetsboken tooa, till exempel D:\Excel\Run.
7. Öppna hello Excel-arbetsbok. På hello **utveckla** band klickar du på **COM-tillägg** och bekräfta att hello HPC Pack Excel COM-tillägg har lästs in.
   
   ![Excel-tillägg för HPC Pack][addin]
8. Redigera hello VBA makrot HPCControlMacros i Excel genom att ändra hello kommenterade rader som visas i hello följande skript. Ersätt lämpliga värden för din miljö.
   
   ![Excel-makro för HPC Pack][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. Kopiera hello Excel-arbetsboken tooan överför katalog, t.ex D:\Excel\Upload. Den här katalogen har angetts i hello HPC_DependsFiles konstant i hello VBA makro.
10. toorun hello arbetsboken på hello klustret i Azure, klicka på hello **klustret** knappen hello kalkylblad.

### <a name="run-excel-udfs"></a>Kör Excel-UDF: er
toorun Excel-UDF: er, följ hello föregående steg 1 – 3 tooset in hello-klientdator. För Excel-UDF: er behöver du inte toohave hello Excel installerat program på compute-noder. Så när du skapar klustret compute-noder, kan du välja en normal beräkning nod avbildning i stället för hello compute-nod avbildningen med Excel.

> [!NOTE]
> Det finns en 34 tecken i hello Excel 2010 och 2013 klustret connector dialogrutan. Du använder den här dialogrutan rutan toospecify hello-kluster som kör hello UDF: er. Om hello fullständig klustrets namn är längre (till exempel hpcexcelhn01.southeastasia.cloudapp.azure.com) passar inte hello i dialogrutan. hello lösning är tooset en datoromfattande variabel som *CCP_IAASHN* med hello värdet hello lång klusternamnet. Ange sedan *CCP_IAASHN %* hello i dialogrutan som hello klustrets huvudnod namn. 
> 
> 

När hello klustret har distribuerats, fortsätter du med följande steg toorun hello exempel inbyggda Excel-UDF. Anpassade Excel-UDF finns dessa [resurser](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLL och distribuera dem på hello IaaS-klustret.

1. Öppna en ny Excel-arbetsbok. På hello **utveckla** band klickar du på **tillägg**. Klicka i dialogrutan hello **Bläddra**, navigera toohello %CCP_HOME%Bin\XLL32 mapp och välj hello exempel ClusterUDF32.xll. Om hello ClusterUDF32 inte finns på hello klientdatorn måste du kopiera den från hello %CCP_HOME%Bin\XLL32 mapp på hello huvudnod.
   
   ![Välj hello UDF][udf]
2. Klicka på **filen** > **alternativ** > **avancerade**. Under **formler**, kontrollera **Tillåt användardefinierade XLL funktioner toorun ett beräkningskluster**. Klicka på **alternativ** och ange hello fullständig klusternamnet i **klustrets huvudnod namn**. (Som anges tidigare inkommande rutan är begränsad too34 tecken så långt klusternamnet inte kanske får plats. Du kan använda den datoromfattande här för ett långt klusternamn.)
   
   ![Konfigurera hello UDF][options]
3. toorun hello UDF beräkning på hello kluster, klicka på hello cellen med värdet =XllGetComputerNameC() och tryck på RETUR. hello funktionen hämtar bara hello namnet på hello compute-nod på vilka hello UDF körs. För hello först köra, en autentiseringsuppgifter i dialogrutan för hello användarnamn och lösenord tooconnect toohello IaaS-kluster.
   
   ![Kör UDF][run]
   
   När det finns många celler toocalculate, trycker du på Alt-Skift-Ctrl + F9 toorun hello beräkningen på alla celler.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Steg 3. Kör en SOA-arbetsbelastning från en lokal klient
toorun Allmänt SOA-program på hello HPC Pack IaaS-kluster med någon av hello metoder i steg 1 toodeploy hello kluster först. Ange en allmän compute-nod avbildning i det här fallet, eftersom du inte behöver Excel på hello compute-noder. Följ dessa steg.

1. När du har hämtat hello klustret certifikat att importera den på hello-klientdatorn under Cert: \CurrentUser\Root.
2. Installera hello [HPC Pack 2012 R2 uppdatering 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) och [HPC Pack 2012 R2 uppdatering 3 klientverktyg](https://www.microsoft.com/download/details.aspx?id=49923). Dessa verktyg kan du toodevelop och kör SOA-klientprogram.
3. Hämta hello HelloWorldR2 [exempelkoden](https://www.microsoft.com/download/details.aspx?id=41633). Öppna hello HelloWorldR2.sln i Visual Studio 2010 eller 2012. (Det här exemplet är inte kompatibelt med nyare versioner av Visual Studio.)
4. Skapa hello EchoService projekt först. Sedan kan du distribuera hello service toohello IaaS-kluster i hello samma sätt som du distribuerar tooan lokala klustret. Detaljerade anvisningar finns i hello viktigt.doc i HelloWordR2. Ändra och skapa hello HellWorldR2 och andra projekt som beskrivs i hello följande avsnitt toogenerate hello SOA-klientprogram som körs på ett Azure IaaS-kluster.

### <a name="use-http-binding-with-azure-storage-queue"></a>Använd Http-bindning med Azure storage-kö
toouse Http-bindning med ett Azure storage-kö gör några ändringar toohello exempelkod.

* Uppdatera hello klusternamnet.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* Du kan också använda hello standard TransportScheme i SessionStartInfo eller uttryckligen ange tooHttp.

```
    info.TransportScheme = TransportScheme.Http;
```

* Använd standard bindning för hello BrokerClient.
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    Eller ange uttryckligen med hjälp av basicHttpBinding hello.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* Du kan också ange hello UseAzureQueue flaggan tootrue i SessionStartInfo. Om inte mängd sätts tootrue som standard när hello klusternamnet har Azure domänsuffix och hello TransportScheme är Http.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Använd Http-bindning utan Azure storage-kö
toouse Http-bindning utan att explicit ange hello UseAzureQueue flaggan toofalse i hello SessionStartInfo en Azure storage-kö.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Använda NetTcp bindning
toouse NetTcp bindning, hello konfigurationen är liknande tooconnecting tooan lokala kluster. Du måste tooopen några slutpunkter i hello huvudnod VM. Om du använde hello HPC Pack IaaS distribution skriptet toocreate hello klustret, till exempel hello set hello slutpunkter i Azure-portalen på följande sätt.

1. Stoppa hello VM.
2. Lägg till hello TCP-portar 9090, 9087, 9091, 9094 för hello Session Broker, respektive Broker worker och Data services
   
    ![Konfigurera slutpunkter][endpoint-new-portal]
3. Starta hello VM.

hello SOA-klientprogrammet kräver inga ändringar förutom ändra hello head toohello IaaS klustret fullständiga namn.

## <a name="next-steps"></a>Nästa steg
* Se [resurserna](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) för mer information om hur du kör Excel arbetsbelastningar med HPC Pack.
* Se [hantera SOA-tjänster i Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) mer information om att distribuera och hantera SOA-tjänster med HPC Pack.

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
