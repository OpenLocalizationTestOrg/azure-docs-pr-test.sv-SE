---
title: aaaContinuous levereras cloud services med TFS i Azure | Microsoft Docs
description: "Lär dig hur tooset in kontinuerlig leverans för Azure cloud appar. Kodexempel för kommandoraden MSBuild-satser och PowerShell-skript."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: c0e5e72ffbd3c05b84ce1733068e92c528bcc4b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Kontinuerlig leverans för molntjänster i Azure
hello process som beskrivs i den här artikeln beskrivs hur du tooset in kontinuerlig leverans för appar i Azure-molnet. Den här processen kan du automatiskt skapa paket och distribuera hello paketet tooAzure efter varje kod incheckning. hello paketet build process som beskrivs i den här artikeln är likvärdiga toohello **paketet** kommandot i Visual Studio och publishing stegen är likvärdiga toohello **publicera** i Visual Studio.
hello artikeln omfattar hello metoder som du skulle använda toocreate en build-server med kommandoradsverktyget MSBuild-instruktioner och Windows PowerShell-skript och det också visar hur toooptionally konfigurera Visual Studio Team Foundation Server - teamet skapa definitioner toouse hello MSBuild-kommandon och PowerShell-skript. hello process kan anpassas för build-miljön och Azure mål-miljöer.

Du kan också använda Visual Studio Team Services, en version av TFS som finns i Azure, toodo detta enklare. 

Innan du börjar bör du publicera programmet från Visual Studio.
Detta säkerställer att alla hello resurser är tillgängliga och initierad när du försöker tooautomate hello publiceringsprocessen.

## <a name="1-configure-hello-build-server"></a>1: Konfigurera hello skapa Server
Innan du kan skapa ett Azure-paket med hjälp av MSBuild, måste du installera hello som krävs för programvaran och verktyg på hello build-servern.

Visual Studio är inte obligatoriska toobe installerad på hello build-servern. Om du vill toouse Byggtjänsten för Team Foundation toomanage build-servern, följer du anvisningarna för hello i hello [Byggtjänsten för Team Foundation] [ Team Foundation Build Service] dokumentation.

1. Hello build-servern installerar hello [.NET Framework 4.5.2][.NET Framework 4.5.2], som innehåller MSBuild.
2. Installera hello senaste [Azure redigering av verktyg för .NET](https://azure.microsoft.com/develop/net/).
3. Installera hello [Azure-bibliotek för .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Kopiera hello Microsoft.WebApplication.targets fil från en toohello för installation av Visual Studio skapa server.

   Den här filen finns på en dator med Visual Studio installerat i hello directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Du bör kopiera den toohello samma katalog på hello build-servern.
5. Installera hello [Azure Tools för Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: skapa ett paket med hjälp av MSBuild-kommandon
Det här avsnittet beskrivs hur tooconstruct en MSBuild kommando som skapar ett Azure-paket. Kör det här steget på hello build server tooverify att allt är korrekt konfigurerat och att hello MSBuild-kommandot har vad du vill toodo. Du kan antingen lägga till den här kommandoraden tooexisting skapa skript i hello build-server eller så kan du använda hello kommandoraden i en TFS skapa Definition, enligt beskrivningen i nästa avsnitt om hello. Mer information om kommandoradsparametrar och MSBuild finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Om Visual Studio är installerat på hello build-server, leta upp och välj **Kommandotolken Visual Studio** i hello **Visual Studio Tools** -mappen i Windows.

   Om Visual Studio inte är installerat på hello build-servern, öppna en kommandotolk och kontrollera att MSBuild.exe är tillgängligt på sökvägen. MSBuild installeras med hello .NET Framework i hello sökvägen % WINDIR %\\Microsoft.NET\\Framework\\*Version*. Om du vill lägga till MSBuild.exe toohello PATH-miljövariabeln när du har .NET Framework 4 har installerats, exempelvis följande kommando vid kommandotolken hello hello:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. Navigera toohello mapp som innehåller filen Azure-projekt som du vill toobuild Kommandotolken hello.
3. Kör MSBuild med hello/target: publicera alternativet som i följande exempel hello:

       MSBuild /target:Publish

   Det här alternativet kan vara förkortas /t: publicera. Hej /t:Publish alternativ i MSBuild ska inte förväxlas med hello publicera kommandon i Visual Studio när du har hello Azure SDK har installerats. Hej /t: publicera alternativet endast versioner hello Azure-paket. Den distribuerar inte hello-paket som hello publicera kommandon i Visual Studio.

   Alternativt kan ange du hello projektnamn som en MSBuild-parameter. Om inget anges används hello aktuell katalog. Mer information om MSBuild kommandoradsalternativ finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Leta upp hello utdata. Som standard det här kommandot skapar en katalog i relationen toohello rotmapp för hello projekt som *ProjectDir*\\bin\\*Configuration* \\ App.publish\\. När du skapar ett Azure-projekt kan generera du två filer, hello paketfilen sig själv och hello tillhörande konfigurationsfil:

   * Project.cspkg
   * ServiceConfiguration. *TargetProfile*.cscfg

   Som standard varje Azure-projekt inkluderar en service konfigurationsfilen (.cscfg-filen) för lokala (felsökning) versioner och ett annat för molnet (mellanlagring eller produktion) versioner, men du kan lägga till eller ta bort service configuration-filer efter behov. När du skapar ett paket i Visual Studio kan blir du ombedd vilka service configuration file tooinclude tillsammans med hello paketet.
5. Ange hello service konfigurationsfil. När du skapar ett paket med hjälp av MSBuild med hello lokal tjänst-konfigurationsfilen som standard. tooinclude en annan tjänstkonfigurationsfil egenskapen TargetProfile för hello MSBuild-kommandot, som i följande exempel hello:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Ange hello plats för hello utdata. Ange hello sökväg med hjälp av /p:PublishDir =*Directory* \\ alternativ, inklusive hello avslutande omvänt avgränsare, som i följande exempel hello:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   När du har skapats och testa en lämplig MSBuild rad toobuild projekt och kombineras till ett Azure-paket, du kan lägga till den här kommandoraden tooyour build-skript. Om din build-servern använder anpassade skript, beror den här processen på specifika anpassade build-processen. Om du använder TFS som en kompileringsmiljö, kan du följa hello instruktioner i hello nästa steg tooadd hello Azure-paketet build tooyour build.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: skapa ett paket med TFS-teamet skapa
Om du har servern TFS (Team Foundation) som en build domänkontrollant och hello skapa server har angetts som en TFS build-dator och du kan du konfigurera en automatisk build för Azure-paket. Mer information om hur tooset in och använda Team Foundation server som ett build-system, se [skala ut ditt system][Scale out your build system]. I synnerhet följande procedur förutsätter att du har konfigurerat build-server enligt beskrivningen i [distribuera och konfigurera en build-server][Deploy and configure a build server], och att du har skapat ett grupprojekt skapas ett moln Service-projekt i hello grupprojekt.

tooconfigure TFS toobuild Azure paket, utföra hello följande steg:

1. I Visual Studio på utvecklingsdatorn hello Visa-menyn, Välj **Team Explorer**, eller välj Ctrl +\\, Ctrl + M. I fönstret Team Explorer expanderar du hello **bygger** nod eller välj hello **bygger** , och väljer **ny skapa Definition**.

   ![Nytt skapa Definition alternativ][0]
2. Välj hello **utlösaren** och ange hello önskad villkor för när du vill att hello paketet toobe inbyggda. Till exempel ange **kontinuerlig Integration** toobuild hello paketet när ett källkontroll checka in inträffar.
3. Välj hello **inställningar för datakälla** fliken och kontrollera att din projektmapp visas i hello **kontroll källmapp** kolumn, och hello status är **Active**.
4. Välj hello **skapa standardvärden** och kontrollera hello namnet på hello build-servern under Build-styrenhet.  Dessutom välja hello **kopiera build utdata toohello följande avlämningsmapp** och ange önskade hello målplatsen.
5. Välj hello **processen** fliken. Hello Process, Välj fliken hello standardmallen under **skapa**, Välj hello projektet om den inte redan är valt, och expandera hello **Avancerat** avsnitt i hello **skapa**avsnitt i hello rutnätet.
6. Välj **MSBuild-argument**, och ange hello lämpliga MSBuild kommandoradsargument som beskrivs i steg 2 ovan. Ange till exempel **/t: Publicera /p:PublishDir =\\\\minserver\\släpper\\**  toobuild ett paket och kopiera hello-paket filer toohello plats \\ \\minserver\\släpper\\:

   ![MSBuild-argument][2]

   > [!NOTE]
   > Kopiera hello filer tooa offentlig resurs gör det enklare toomanually distribuerar hello-paket på utvecklingsdatorn.
7. Testa hello lyckats build-steg genom att kontrollera i ändra tooyour projekt eller Köa en ny version. tooqueue in en ny version i teamet Explorer, högerklicka på **alla skapa definitioner** och välj sedan **kön nya skapa**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: publicera ett paket med hjälp av ett PowerShell-skript
Det här avsnittet beskrivs hur tooconstruct ett Windows PowerShell-skript som ska publicera hello Cloud app-paket utdata tooAzure med valfria parametrar. Det här skriptet kan anropas efter hello build steg i din anpassade build-automation. Det kan också anropas från processmall arbetsflödesaktiviteter i Visual Studio TFS Team skapa.

1. Installera hello [Azure PowerShell-cmdlets] [ Azure PowerShell cmdlets] (v0.6.1 eller högre).
   Under installationsfasen för hello cmdlet, välja tooinstall som en snapin-modul. Observera att den versionen som stöds officiellt ersätter hello äldre version som erbjuds via CodePlex, även om tidigare versioner av hello har numrerade 2.x.x.
2. Starta Azure PowerShell med hjälp av hello Start-menyn eller startsidan. Om du startar i det här sättet kommer att laddas hello Azure PowerShell-cmdlets.
3. I hello PowerShell-Kommandotolken, kontrollera att PowerShell-cmdlets för hello laddas genom att ange hello partiella kommando `Get-Azure` och sedan trycka på hello tabbtangenten för instruktionen slutfördes.

   Om du trycker på tabbtangenten hello upprepade gånger, bör du se olika Azure PowerShell-kommandon.
4. Kontrollera att du kan ansluta tooyour Azure-prenumeration genom att importera din prenumerationsinformation från hello .publishsettings-fil.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Ange hello kommando

   `Get-AzureSubscription`

   Detta visar information om din prenumeration. Kontrollera att allt är korrekt.
5. Spara hello skript-mall hello slutet av den här artikeln till mappen skript c:\\skript\\WindowsAzure\\**PublishCloudService.ps1**.
6. Granska hello parametrar avsnitt av hello skriptet. Lägg till eller ändra alla standardvärden. Dessa värden kan alltid åsidosättas genom att passera i explicit parametrar.
7. Se till att det finns giltiga Molntjänsten och lagringskonton som skapats i din prenumeration som kan vara mål för hello publicera skript. Storage-konto (blob storage) kommer att använda tooupload och tillfälligt lagra hello distribution av paket och config-filen medan distributionen skapas.

   * toocreate en ny molntjänst, kan du anropa den här skript eller Använd hello [Azure-portalen](https://portal.azure.com). Hej molntjänstnamnet kommer att användas som ett prefix i ett fullständigt kvalificerat domännamn och därför det måste vara unikt.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * toocreate ett nytt lagringskonto, kan du anropa den här skript eller Använd hello [Azure-portalen](https://portal.azure.com). Hej lagringskontonamn används som ett prefix i ett fullständigt kvalificerat domännamn och därför det måste vara unikt. Du kan försöka med hello samma namn som Molntjänsten.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Anropa hello skript direkt från Azure PowerShell eller tråd in det här skriptet tooyour värden build automation toooccur efter hello paketet build.

   > [!IMPORTANT]
   > hello-skriptet kommer alltid att ta bort eller ersätta din befintliga distributioner som standard om de upptäcks. Detta är nödvändigt för att aktivera kontinuerlig leverans från automation när inga användare att fråga är möjligt.
   >
   >

   **Exempelscenario 1:** kontinuerlig distribution toohello mellanlagring miljö för en tjänst:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   Detta är vanligtvis följas upp av test kör verifieringen och en VIP-växling. hello VIP-växlingen kan göras via hello [Azure-portalen](https://portal.azure.com) eller med cmdleten hello Move-distribution.

   **Exempelscenario 2:** kontinuerlig distribution toohello produktionsmiljön för en dedikerad test-tjänst

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Fjärrskrivbord:**

   Om Fjärrskrivbord har aktiverats i Azure-projekt behöver tooperform ytterligare enstaka steg tooensure hello rätt moln Tjänstcertifikatet har överförts tooall molntjänster som mål för det här skriptet.

   Leta upp hello certifikat-tumavtryck värden förväntades av rollerna. Tumavtryck för värden visas under hello certifikat i molnet config-fil (d.v.s. ServiceConfiguration.Cloud.cscfg). Det är också synliga i dialogrutan för hello konfiguration av fjärrskrivbord i Visual Studio när du visa alternativ och visa hello valt certifikat.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   Överför certifikat för fjärrskrivbord som ett enstaka installationen steg med hjälp av hello följande cmdlet-skript:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Exempel:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   Du kan också exportera hello certifikatfil PFX med privata nyckeln och överför certifikat tooeach mål molnbaserad tjänst med hjälp av den [Azure-portalen](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM tooDOCS. I'm unable toofind a replacement links, so I'm commenting out this reference for now. hello author can investigate in hello future. "Read hello following article toolearn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Uppgradera jämfört med distributionen. Ta bort distributionen -\> ny distribution**

   hello skript kommer som standard att utföra en Uppgraderingsdistribution ($enableDeploymentUpgrade = 1) när ingen parameter som skickas eller värdet 1 skickas explicit. Detta har fördelen med att göra kortare tid än en fullständig distribution för enskild instanser. För instanser som kräver hög tillgänglighet har också hello fördelen att lämna vissa instanser körs samtidigt som andra uppgraderas (går din uppdateringsdomän), plus din VIP tas inte bort.

   Uppgradera distributionen kan inaktiveras i hello skript ($enableDeploymentUpgrade = 0) eller genom att skicka *- enableDeploymentUpgrade 0* som en parameter som ändrar skriptet beteende toofirst bort alla befintliga distributionen och sedan skapa en ny distribution.

   > [!IMPORTANT]
   > hello-skriptet kommer alltid att ta bort eller ersätta din befintliga distributioner som standard om de upptäcks. Detta är nödvändigt för att aktivera kontinuerlig leverans från automation om ingen användare/operator fråga är möjligt.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: publicera ett paket med TFS-teamet skapa
Det här valfria steget ansluter TFS Team skapa toohello skript skapade i steg 4, som hanterar publicering av hello paketet build tooAzure. Detta innebär att ändra hello processmall som används av build-definition så att det körs en publicera aktivitet hello slutet av hello arbetsflöde. hello publicera aktivitet körs i PowerShell-kommandot Skicka parametrar från hello build. Utdata från hello MSBuild mål och publicera skript ska skickas till hello standardversion utdata.

1. Redigera hello skapa Definition ansvarar för kontinuerlig distribution.
2. Välj hello **processen** fliken.
3. Följ [instruktionerna](http://msdn.microsoft.com/library/dd647551.aspx) tooadd en aktivitet projekt för hello skapa processmall ladda ned hello standardmallen, Lägg till den hello projektet och kontrollera i. Ge hello build processmall ett nytt namn, till exempel AzureBuildProcessTemplate.
4. Returnera toohello **processen** fliken och använda **Visa detaljer** tooshow en lista över tillgängliga build processmallar. Välj hello **ny...**  knappen och navigera toohello projekt du lagt till och checkats in. Leta upp hello-mallen som du just skapade och välj **OK**.
5. Öppna hello valt processmall för redigering. Du kan öppna direkt i hello Workflow designer eller hello XML-redigerare toowork med hello XAML.
6. Lägg till hello efter nya argumentlista som separata poster hello argument-fliken i hello Arbetsflödesdesignern. Alla argument bör ha riktningen = i och Skriv = sträng. Dessa kommer att använda tooflow parametrar från hello build-definition i hello-arbetsflöde, vilken sedan get används toocall hello publicera skript.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Listan med argument][3]

   hello motsvarande XAML ser ut så här:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Lägg till en ny sekvens hello slutet av körs på agenten:

   1. Starta genom att lägga till en instruktion om aktiviteten toocheck efter en giltigt skriptfil. Ange hello villkorsvärdet toothis:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Hello sedan fall av hello om instruktionen, lägga till en ny sekvensaktivitet. Ange hello visa namnet too'Start publicera '
   3. Lägg till följande lista över nya variabler som separata poster på fliken variabler av hello workflow designer med hello Start publicera sekvens fortfarande markerat. Alla variabler ska ha variabeltyp = sträng och omfånget = Start publicera. Dessa kommer att använda tooflow parametrar från hello build-definition i arbetsflödet, vilken sedan get används toocall hello publicera skript.

      * SubscriptionDataFilePath av typen String
      * PublishScriptFilePath av typen String

        ![Nya variabler][4]
   4. Om du använder TFS 2012 eller tidigare, lägga till en ConvertWorkspaceItem aktivitet hello början av hello ny sekvens. Om du använder TFS 2013 eller senare kan du lägga till en GetLocalPath aktivitet hello början av hello ny sekvens. För en ConvertWorkspaceItem ange hello egenskaper på följande sätt: riktning = ServerToLocal, visningsnamn = 'Konvertera publicera skript filename', indata = PublishScriptLocation, resultat = PublishScriptFilePath, arbetsytan = 'Arbetsytan'. Ange hello egenskapen IncomingPath too'PublishScriptLocation för en GetLocalPath aktivitet ', och resultatet too'PublishScriptFilePath hello ”. Den här aktiviteten konverterar hello sökvägen toohello publicera skript från platser för TFS-server (om tillämpligt) tooa standard lokal disk sökväg.
   5. Om du använder TFS 2012 eller tidigare, lägga till en annan ConvertWorkspaceItem aktivitet hello slutet av hello ny sekvens. Riktning = ServerToLocal DisplayName = konvertera prenumerationen filnamn, indata = SubscriptionDataFileLocation, resultat = SubscriptionDataFilePath, arbetsytan = 'Arbetsytan'. Om du använder TFS 2013 eller senare, lägga till en annan GetLocalPath. IncomingPath = SubscriptionDataFileLocation, och resultatet = SubscriptionDataFilePath.
   6. Lägga till en InvokeProcess aktivitet hello slutet av hello ny sekvens.
      Den här aktiviteten anrop PowerShell.exe med hello argument skickades av hello skapa Definition.

      + Argument = String.Format (”-filen” ”{0}” ”- serviceName {1} - storageAccountName {2} - anges i packageLocation” ”{3}” ”- cloudConfigLocation” ”{4}” ”- subscriptionDataFile” ”{5}” ”- selectedSubscription {6}-miljön” ”{7}” ””, PublishScriptFilePath, ServiceName, StorageAccountName, anges i PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, miljön)
      + DisplayName = Execute publicera skript
      + FileName = ”PowerShell” (citattecken för hello)
      + OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)
   7. I hello **hantera standardutdata** avsnittet textruta för InvokeProcess genom att ange hello textruta värdet too'data'. Här är en variabel toostore hello standardutdata data.
   8. Lägga till en WriteBuildMessage aktivitet under hello **hantera standardutdata** avsnitt. Ange hello vikten = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' och hello Message = ”data”. Detta säkerställer att hello standard utdata från skriptet hämta skrivs toohello build-utdata.
   9. I hello **hantera Felutdata** avsnittet textruta för InvokeProcess genom att ange hello textruta värdet too'data'. Här är en variabel toostore hello standardfel data.
   10. Lägga till en WriteBuildError aktivitet under hello **hantera Felutdata** avsnitt. Ange hello Message = ”data”. Detta säkerställer att hello standardfel hello skriptet hämta skrivs utdata för toohello build-fel.
   11. Korrigera eventuella fel identifieras med blå utropstecken märken. Hovra över utropstecken märken tooget en ledtråd om hello felet. Spara hello arbetsflöde om du vill rensa fel.

   hello slutresultatet av hello publicera arbetsflödet aktiviteter ser ut så här i hello-designer:

   ![Arbetsflödesaktiviteter][5]

   hello slutresultatet av hello publicera arbetsflödet aktiviteter ser ut så här i XAML:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. Spara hello build processen mall arbetsflödet och checka In den här filen.
9. Redigera definition av hello build (Stäng den om den redan är öppen), och välj hello **ny** knappen om du inte ser hello mall i hello lista över processmallar ännu.
10. Ange egenskapsvärden för hello-parametern i hello övrigt avsnittet enligt följande:

    1. CloudConfigLocation = ”c:\\släpper\\app.publish\\ServiceConfiguration.Cloud.cscfg' *det här värdet är härledd från: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. Anges i PackageLocation = ' c:\\släpper\\app.publish\\ContactManager.Azure.cspkg' *det här värdet är härledd från: ($PublishDir)($ProjectName) .cspkg*
    3. PublishScriptLocation = ”c:\\skript\\WindowsAzure\\PublishCloudService.ps1'
    4. ServiceName = 'mycloudservicename' *Använd hello lämpliga molntjänstnamnet här*
    5. Miljö = ”mellanlagring”
    6. StorageAccountName = 'mystorageaccountname' *Använd hello lämpliga lagringskontonamnet här*
    7. SubscriptionDataFileLocation = ”c:\\skript\\WindowsAzure\\Subscription.xml'
    8. SubscriptionName = 'default'

    ![Parametern egenskapsvärden][6]
11. Spara hello ändringar toohello skapa Definition.
12. Kö ett Build-tooexecute både hello paketet build och publicera. Om du har en utlösare ange tooContinuous Integration, körs det här problemet på varje incheckning.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 skript mall
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy too$servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according too$alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress tooactivity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Nästa steg
tooenable fjärrfelsökning när du använder kontinuerlig leverans finns [aktivera fjärrfelsökning när du använder kontinuerlig leverans toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[hello .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
