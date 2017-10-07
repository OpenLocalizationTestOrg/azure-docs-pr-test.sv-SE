---
title: "aaaLearn om säkerhetsprinciper i Azure mikrotjänster | Microsoft Docs"
description: "En översikt över hur toorun ett Service Fabric-program under system och för lokala säkerhetskonton, inklusive hello SetupEntry plats där ett program behöver tooperform vissa privilegierad åtgärd innan den börjar"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: f5afba69e09aa4f3c9efa4d3efc6995c813a1f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-security-policies-for-your-application"></a><span data-ttu-id="e3eaa-103">Konfigurera säkerhetsprinciper för ditt program</span><span class="sxs-lookup"><span data-stu-id="e3eaa-103">Configure security policies for your application</span></span>
<span data-ttu-id="e3eaa-104">Du kan skydda program som körs i hello kluster under olika användarkonton med hjälp av Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-104">By using Azure Service Fabric, you can secure applications that are running in hello cluster under different user accounts.</span></span> <span data-ttu-id="e3eaa-105">Service Fabric hjälper också till att säkra hello-resurser som används av program när hello distribution under hello användarkonton – till exempel, filer, kataloger och certifikat.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-105">Service Fabric also helps secure hello resources that are used by applications at hello time of deployment under hello user accounts--for example, files, directories, and certificates.</span></span> <span data-ttu-id="e3eaa-106">Det gör att program som körs, även i en delad värdmiljö säkrare från varandra.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-106">This makes running applications, even in a shared hosted environment, more secure from one another.</span></span>

<span data-ttu-id="e3eaa-107">Service Fabric-program körs under hello-konto som hello Fabric.exe processen körs under som standard.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-107">By default, Service Fabric applications run under hello account that hello Fabric.exe process runs under.</span></span> <span data-ttu-id="e3eaa-108">Service Fabric ger också hello kapaciteten toorun program under ett lokalt användarkonto eller kontot Lokalt system som anges i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-108">Service Fabric also provides hello capability toorun applications under a local user account or local system account, which is specified within hello application manifest.</span></span> <span data-ttu-id="e3eaa-109">Typer av lokala system som stöds är **Lokalanvändare**, **NetworkService**, **LocalService**, och **LocalSystem**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-109">Supported local system account types are **LocalUser**, **NetworkService**, **LocalService**, and **LocalSystem**.</span></span>

 <span data-ttu-id="e3eaa-110">När du kör Service Fabric på Windows Server i ditt datacenter med hjälp av hello fristående installationsprogram, använder du Active Directory-domänkonton, inklusive grupphanterade tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-110">When you're running Service Fabric on Windows Server in your datacenter by using hello standalone installer, you can use Active Directory domain accounts, including group managed service accounts.</span></span>

<span data-ttu-id="e3eaa-111">Du kan definiera och skapa användargrupper så att en eller flera användare kan läggas till tooeach grupp toobe hanteras tillsammans.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-111">You can define and create user groups so that one or more users can be added tooeach group toobe managed together.</span></span> <span data-ttu-id="e3eaa-112">Detta är användbart när det finns flera användare för olika startpunkter och de måste toohave vissa vanliga behörigheter som är tillgängliga på hello gruppnivå.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-112">This is useful when there are multiple users for different service entry points and they need toohave certain common privileges that are available at hello group level.</span></span>

## <a name="configure-hello-policy-for-a-service-setup-entry-point"></a><span data-ttu-id="e3eaa-113">Konfigurera hello princip för en startpunkt för installationen av tjänsten</span><span class="sxs-lookup"><span data-stu-id="e3eaa-113">Configure hello policy for a service setup entry point</span></span>
<span data-ttu-id="e3eaa-114">Enligt beskrivningen i hello [programmodell](service-fabric-application-model.md), hello installationsprogrammet startpunkten **SetupEntryPoint**, är en privilegierade startpunkt som körs med samma autentiseringsuppgifter som Service Fabric hello (vanligtvis hello *NetworkService* konto) innan andra startpunkt.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-114">As described in hello [application model](service-fabric-application-model.md), hello setup entry point, **SetupEntryPoint**, is a privileged entry point that runs with hello same credentials as Service Fabric (typically hello *NetworkService* account) before any other entry point.</span></span> <span data-ttu-id="e3eaa-115">hello körbar fil som anges av **EntryPoint** är vanligtvis hello tidskrävande tjänstvärden.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-115">hello executable that is specified by **EntryPoint** is typically hello long-running service host.</span></span> <span data-ttu-id="e3eaa-116">Med en separat installationsprogrammet startpunkten så slipper toorun hello tjänstvärden körbara med höga privilegier för längre tid.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-116">So having a separate setup entry point avoids having toorun hello service host executable with high privileges for extended periods of time.</span></span> <span data-ttu-id="e3eaa-117">hello körbara som **EntryPoint** anger körs efter **SetupEntryPoint** avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-117">hello executable that **EntryPoint** specifies is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="e3eaa-118">hello resulterande processen övervakas och startas om och börjar igen med **SetupEntryPoint** om den aldrig avslutar eller kraschar.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-118">hello resulting process is monitored and restarted, and begins again with **SetupEntryPoint** if it ever terminates or crashes.</span></span>

<span data-ttu-id="e3eaa-119">hello följande är en enkel service manifest exempel som visar hello SetupEntryPoint och hello huvudsakliga EntryPoint för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-119">hello following is a simple service manifest example that shows hello SetupEntryPoint and hello main EntryPoint for hello service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-hello-policy-by-using-a-local-account"></a><span data-ttu-id="e3eaa-120">Konfigurera hello princip genom att använda ett lokalt konto</span><span class="sxs-lookup"><span data-stu-id="e3eaa-120">Configure hello policy by using a local account</span></span>
<span data-ttu-id="e3eaa-121">Du kan ändra hello säkerhetsbehörigheter som den körs i hello programmanifest när du har konfigurerat hello service toohave en startpunkt för installationen.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-121">After you configure hello service toohave a setup entry point, you can change hello security permissions that it runs under in hello application manifest.</span></span> <span data-ttu-id="e3eaa-122">hello som följande exempel visar hur tooconfigure hello service toorun under användare administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-122">hello following example shows how tooconfigure hello service toorun under user administrator account privileges.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

<span data-ttu-id="e3eaa-123">Skapa först en **säkerhetsobjekt** avsnitt med ett användarnamn, t.ex SetupAdminUser.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-123">First, create a **Principals** section with a user name, such as SetupAdminUser.</span></span> <span data-ttu-id="e3eaa-124">Detta anger hello användaren är medlem i systemgruppen för hello administratörer.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-124">This indicates that hello user is a member of hello Administrators system group.</span></span>

<span data-ttu-id="e3eaa-125">Sedan under hello **ServiceManifestImport** avsnittet, konfigurera en princip tooapply detta säkerhetsobjekt för**SetupEntryPoint**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-125">Next, under hello **ServiceManifestImport** section, configure a policy tooapply this principal too**SetupEntryPoint**.</span></span> <span data-ttu-id="e3eaa-126">Detta meddelar Service Fabric som när hello **MySetup.bat** fil körs, bör vara `RunAs` med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-126">This tells Service Fabric that when hello **MySetup.bat** file is run, it should be `RunAs` with administrator privileges.</span></span> <span data-ttu-id="e3eaa-127">Med hänsyn till att du har *inte* tillämpas en princip toohello startpunkten, hello koden i **MyServiceHost.exe** körs under hello system **NetworkService** konto.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-127">Given that you have *not* applied a policy toohello main entry point, hello code in **MyServiceHost.exe** runs under hello system **NetworkService** account.</span></span> <span data-ttu-id="e3eaa-128">Detta är hello standardkonto som alla startpunkter för tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-128">This is hello default account that all service entry points are run as.</span></span>

<span data-ttu-id="e3eaa-129">Vi ska nu lägga till hello filen MySetup.bat toohello Visual Studio-projekt tootest hello administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-129">Let's now add hello file MySetup.bat toohello Visual Studio project tootest hello administrator privileges.</span></span> <span data-ttu-id="e3eaa-130">Högerklicka på hello service-projekt i Visual Studio och lägga till en ny fil med namnet MySetup.bat.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-130">In Visual Studio, right-click hello service project and add a new file called MySetup.bat.</span></span>

<span data-ttu-id="e3eaa-131">Kontrollera därefter hello MySetup.bat filen ingår i hello service-paketet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-131">Next, ensure that hello MySetup.bat file is included in hello service package.</span></span> <span data-ttu-id="e3eaa-132">Som standard är den inte.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-132">By default, it is not.</span></span> <span data-ttu-id="e3eaa-133">Välj hello-fil, högerklicka på tooget hello snabbmenyn och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-133">Select hello file, right-click tooget hello context menu, and choose **Properties**.</span></span> <span data-ttu-id="e3eaa-134">I egenskapsdialogrutan för hello, kontrollerar du att **kopiera tooOutput Directory** har angetts för**kopiera om nyare**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-134">In hello Properties dialog box, ensure that **Copy tooOutput Directory** is set too**Copy if newer**.</span></span> <span data-ttu-id="e3eaa-135">Se hello följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-135">See hello following screenshot.</span></span>

![Visual Studio-CopyToOutput för SetupEntryPoint kommandofil][image1]

<span data-ttu-id="e3eaa-137">Öppna hello MySetup.bat filen och Lägg till hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-137">Now open hello MySetup.bat file and add hello following commands:</span></span>

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set too> out.txt
echo %TestVariable% >> out.txt

REM toodelete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

<span data-ttu-id="e3eaa-138">Sedan skapa och distribuera hello lösning tooa lokal utveckling kluster.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-138">Next, build and deploy hello solution tooa local development cluster.</span></span> <span data-ttu-id="e3eaa-139">När hello-tjänsten har startats, som visas i Service Fabric Explorer, kan du se hello MySetup.bat filen lyckades på två sätt.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-139">After hello service has started, as shown in Service Fabric Explorer, you can see that hello MySetup.bat file was successful in a two ways.</span></span> <span data-ttu-id="e3eaa-140">Öppna ett PowerShell-Kommandotolken och skriv:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-140">Open a PowerShell command prompt and type:</span></span>

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

<span data-ttu-id="e3eaa-141">Anteckna hello namnet hello noden där hello-tjänsten har distribuerats och startas i Service Fabric Explorer – till exempel nod 2.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-141">Then, note hello name of hello node where hello service was deployed and started in Service Fabric Explorer--for example, Node 2.</span></span> <span data-ttu-id="e3eaa-142">Nu ska du navigera toohello programmet instans mappen toofind hello out.txt arbetsfil som visar hello värdet för **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-142">Next, navigate toohello application instance work folder toofind hello out.txt file that shows hello value of **TestVariable**.</span></span> <span data-ttu-id="e3eaa-143">Till exempel om den här tjänsten har distribuerats tooNode 2, sedan kan du gå toothis sökvägen för hello **MyApplicationType**:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-143">For example, if this service was deployed tooNode 2, then you can go toothis path for hello **MyApplicationType**:</span></span>

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-hello-policy-by-using-local-system-accounts"></a><span data-ttu-id="e3eaa-144">Konfigurera hello princip genom att använda lokala systemkontona</span><span class="sxs-lookup"><span data-stu-id="e3eaa-144">Configure hello policy by using local system accounts</span></span>
<span data-ttu-id="e3eaa-145">Det är ofta bättre toorun hello startskript med hjälp av ett lokalt systemkonto i stället för ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-145">Often, it's preferable toorun hello startup script by using a local system account rather than an administrator account.</span></span> <span data-ttu-id="e3eaa-146">Kör hello RunAs-principen som en medlem i hello administratörsgruppen vanligtvis inte fungerar väl eftersom datorer har User Access Control (UAC) är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-146">Running hello RunAs policy as a member of hello Administrators group typically doesn’t work well because machines have User Access Control (UAC) enabled by default.</span></span> <span data-ttu-id="e3eaa-147">I sådana fall **hello rekommendation är toorun hello SetupEntryPoint som LocalSystem, i stället för som en lokal användargrupp för tillagda tooAdministrators**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-147">In such cases, **hello recommendation is toorun hello SetupEntryPoint as LocalSystem, instead of as a local user added tooAdministrators group**.</span></span> <span data-ttu-id="e3eaa-148">hello följande exempel visar inställningen hello SetupEntryPoint toorun som LocalSystem:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-148">hello following example shows setting hello SetupEntryPoint toorun as LocalSystem:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

## <a name="start-powershell-commands-from-a-setup-entry-point"></a><span data-ttu-id="e3eaa-149">Starta PowerShell-kommandon från en startpunkt för installationen</span><span class="sxs-lookup"><span data-stu-id="e3eaa-149">Start PowerShell commands from a setup entry point</span></span>
<span data-ttu-id="e3eaa-150">toorun PowerShell från hello **SetupEntryPoint** punkt, kan du köra **PowerShell.exe** i en batchfil som pekar tooa PowerShell fil.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-150">toorun PowerShell from hello **SetupEntryPoint** point, you can run **PowerShell.exe** in a batch file that points tooa PowerShell file.</span></span> <span data-ttu-id="e3eaa-151">Lägg först till en PowerShell-filen toohello service-projekt – till exempel **MySetup.ps1**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-151">First, add a PowerShell file toohello service project--for example, **MySetup.ps1**.</span></span> <span data-ttu-id="e3eaa-152">Kom ihåg tooset hello *kopiera om nyare* egenskapen så att hello-filen ingår också i hello servicepaket.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-152">Remember tooset hello *Copy if newer* property so that hello file is also included in hello service package.</span></span> <span data-ttu-id="e3eaa-153">hello följande exempel visas en exempelfil batch som startar en PowerShell-fil som heter MySetup.ps1 som anger en systemmiljövariabler kallas **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-153">hello following example shows a sample batch file that starts a PowerShell file called MySetup.ps1, which sets a system environment variable called **TestVariable**.</span></span>

<span data-ttu-id="e3eaa-154">MySetup.bat toostart en PowerShell-fil:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-154">MySetup.bat toostart a PowerShell file:</span></span>

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

<span data-ttu-id="e3eaa-155">Lägg till hello följande tooset en systemmiljövariabler i hello PowerShell-filen:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-155">In hello PowerShell file, add hello following tooset a system environment variable:</span></span>

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> <span data-ttu-id="e3eaa-156">Som standard när hello kommandofilen körs det ser ut i hello programmapp kallas **fungerar** för filer.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-156">By default, when hello batch file runs, it looks at hello application folder called **work** for files.</span></span> <span data-ttu-id="e3eaa-157">I det här fallet när MySetup.bat körs, vi vill att den här toofind hello MySetup.ps1 filen i hello samma mapp som är programmet hello **kodpaketet** mapp.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-157">In this case, when MySetup.bat runs, we want this toofind hello MySetup.ps1 file in hello same folder, which is hello application **code package** folder.</span></span> <span data-ttu-id="e3eaa-158">toochange den här mappen, ange hello arbetsmapp:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-158">toochange this folder, set hello working folder:</span></span>
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a><span data-ttu-id="e3eaa-159">Använda omdirigering till konsolen för lokala felsökning</span><span class="sxs-lookup"><span data-stu-id="e3eaa-159">Use console redirection for local debugging</span></span>
<span data-ttu-id="e3eaa-160">Ibland är det användbart toosee hello konsolens utdata från att köra ett skript för felsökning.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-160">Occasionally, it's useful toosee hello console output from running a script for debugging purposes.</span></span> <span data-ttu-id="e3eaa-161">toodo, kan du ange en konsol omdirigering av principen som skriver hello tooa utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-161">toodo this, you can set a console redirection policy, which writes hello output tooa file.</span></span> <span data-ttu-id="e3eaa-162">hello filen utdata kallas skriftliga toohello programmappen **loggen** på hello nod där hello programmet har distribuerats och kör.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-162">hello file output is written toohello application folder called **log** on hello node where hello application is deployed and run.</span></span> <span data-ttu-id="e3eaa-163">(Se var toofind detta i föregående exempel hello.)</span><span class="sxs-lookup"><span data-stu-id="e3eaa-163">(See where toofind this in hello preceding example.)</span></span>

> [!WARNING]
> <span data-ttu-id="e3eaa-164">Använd aldrig hello konsolen omdirigeringspolicyn i ett program som distribuerats i produktionsmiljön eftersom detta kan påverka hello programmet redundans.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-164">Never use hello console redirection policy in an application that is deployed in production because this can affect hello application failover.</span></span> <span data-ttu-id="e3eaa-165">*Endast* använda detta för lokal utveckling och felsökning.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-165">*Only* use this for local development and debugging purposes.</span></span>  
> 
> 

<span data-ttu-id="e3eaa-166">hello visar följande exempel inställningen hello omdirigering av konsol med ett FileRetentionCount-värde:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-166">hello following example shows setting hello console redirection with a FileRetentionCount value:</span></span>

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

<span data-ttu-id="e3eaa-167">Om du ändrar nu hello MySetup.ps1 filen toowrite en **Echo** kommandot detta skriver toohello utdatafilen för felsökning:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-167">If you now change hello MySetup.ps1 file toowrite an **Echo** command, this will write toohello output file for debugging purposes:</span></span>

```
Echo "Test console redirection which writes toohello application log folder on hello node that hello application is deployed to"
```

<span data-ttu-id="e3eaa-168">**När du felsöka skriptet omedelbart ta bort den här konsolen omdirigeringspolicyn**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-168">**After you debug your script, immediately remove this console redirection policy**.</span></span>

## <a name="configure-a-policy-for-service-code-packages"></a><span data-ttu-id="e3eaa-169">Konfigurera en princip för servicepaket för kod</span><span class="sxs-lookup"><span data-stu-id="e3eaa-169">Configure a policy for service code packages</span></span>
<span data-ttu-id="e3eaa-170">I hello föregående steg, som du såg hur tooapply tooSetupEntryPoint en RunAs-principen.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-170">In hello preceding steps, you saw how tooapply a RunAs policy tooSetupEntryPoint.</span></span> <span data-ttu-id="e3eaa-171">Nu ska vi titta lite djupare hur toocreate olika säkerhetsobjekt som kan användas som tjänsten principer.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-171">Let's look a little deeper into how toocreate different principals that can be applied as service policies.</span></span>

### <a name="create-local-user-groups"></a><span data-ttu-id="e3eaa-172">Skapa lokala användargrupper</span><span class="sxs-lookup"><span data-stu-id="e3eaa-172">Create local user groups</span></span>
<span data-ttu-id="e3eaa-173">Du kan definiera och skapa användargrupper som tillåter att en eller flera användare toobe lägga tooa grupp.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-173">You can define and create user groups that allow one or more users toobe added tooa group.</span></span> <span data-ttu-id="e3eaa-174">Detta är användbart om det finns flera användare för olika startpunkter och de måste toohave vissa vanliga behörigheter som är tillgängliga på hello gruppnivå.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-174">This is useful if there are multiple users for different service entry points and they need toohave certain common privileges that are available at hello group level.</span></span> <span data-ttu-id="e3eaa-175">hello följande exempel visas en lokal grupp som kallas **LocalAdminGroup** som har administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-175">hello following example shows a local group called **LocalAdminGroup** that has administrator privileges.</span></span> <span data-ttu-id="e3eaa-176">Två användare, Customer1 och Customer2, som blir medlemmar i den lokala gruppen.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-176">Two users, Customer1 and Customer2, are made members of this local group.</span></span>

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a><span data-ttu-id="e3eaa-177">Skapa lokala användare</span><span class="sxs-lookup"><span data-stu-id="e3eaa-177">Create local users</span></span>
<span data-ttu-id="e3eaa-178">Du kan skapa en lokal användare som kan använda toohelp säker en tjänst i hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-178">You can create a local user that can be used toohelp secure a service within hello application.</span></span> <span data-ttu-id="e3eaa-179">När en **Lokalanvändare** kontotyp som anges under hello huvudobjekt Hej programmanifestet Service Fabric skapar lokala användarkonton på datorer där hello programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-179">When a **LocalUser** account type is specified in hello principals section of hello application manifest, Service Fabric creates local user accounts on machines where hello application is deployed.</span></span> <span data-ttu-id="e3eaa-180">Som standard har dessa konton inte hello samma namn som de som anges i programmet hello manifest (till exempel Customer3 i följande exempel hello).</span><span class="sxs-lookup"><span data-stu-id="e3eaa-180">By default, these accounts do not have hello same names as those specified in hello application manifest (for example, Customer3 in hello following sample).</span></span> <span data-ttu-id="e3eaa-181">I stället de genereras dynamiskt och ha slumpmässiga lösenord.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-181">Instead, they are dynamically generated and have random passwords.</span></span>

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

<span data-ttu-id="e3eaa-182">Om ett program kräver att hello användarkonto och lösenord vara samma på alla datorer (till exempel tooenable NTLM-autentisering), ange hello klustermanifestet NTLMAuthenticationEnabled tootrue.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-182">If an application requires that hello user account and password be same on all machines (for example, tooenable NTLM authentication), hello cluster manifest must set NTLMAuthenticationEnabled tootrue.</span></span> <span data-ttu-id="e3eaa-183">Hej klustermanifestet måste också ange en NTLMAuthenticationPasswordSecret som används för toogenerate hello samma lösenord på alla datorer.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-183">hello cluster manifest must also specify an NTLMAuthenticationPasswordSecret that is used toogenerate hello same password across all machines.</span></span>

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-toohello-service-code-packages"></a><span data-ttu-id="e3eaa-184">Tilldela principer toohello kod servicepaket</span><span class="sxs-lookup"><span data-stu-id="e3eaa-184">Assign policies toohello service code packages</span></span>
<span data-ttu-id="e3eaa-185">Hej **RunAsPolicy** avsnittet för en **ServiceManifestImport** anger hello konto från hello säkerhetsobjekt avsnitt som ska använda toorun en kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-185">hello **RunAsPolicy** section for a **ServiceManifestImport** specifies hello account from hello principals section that should be used toorun a code package.</span></span> <span data-ttu-id="e3eaa-186">Paket från hello service manifest kod associerar även med användarkonton i hello säkerhetsobjekt avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-186">It also associates code packages from hello service manifest with user accounts in hello principals section.</span></span> <span data-ttu-id="e3eaa-187">Du kan ange detta för hello installations- eller huvudsakliga startpunkter eller ange `All` tooapply den tooboth.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-187">You can specify this for hello setup or main entry points, or you can specify `All` tooapply it tooboth.</span></span> <span data-ttu-id="e3eaa-188">hello visar följande exempel olika principer tillämpas:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-188">hello following example shows different policies being applied:</span></span>

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

<span data-ttu-id="e3eaa-189">Om **EntryPointType** anges hello standard anges tooEntryPointType = ”Main”.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-189">If **EntryPointType** is not specified, hello default is set tooEntryPointType=”Main”.</span></span> <span data-ttu-id="e3eaa-190">Ange **SetupEntryPoint** är särskilt användbar när du vill toorun vissa höga installationsprogrammet åtgärder under en system-kontot.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-190">Specifying **SetupEntryPoint** is especially useful when you want toorun certain high-privilege setup operations under a system account.</span></span> <span data-ttu-id="e3eaa-191">hello faktiska Tjänstkod kan köras under ett konto med lägre behörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-191">hello actual service code can run under a lower-privilege account.</span></span>

### <a name="apply-a-default-policy-tooall-service-code-packages"></a><span data-ttu-id="e3eaa-192">Tillämpa en standard princip tooall kod servicepaket</span><span class="sxs-lookup"><span data-stu-id="e3eaa-192">Apply a default policy tooall service code packages</span></span>
<span data-ttu-id="e3eaa-193">Du använder hello **DefaultRunAsPolicy** avsnittet toospecify ett användarkonto för standard för alla kod paket som inte har en specifik **RunAsPolicy** definieras.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-193">You use hello **DefaultRunAsPolicy** section toospecify a default user account for all code packages that don’t have a specific **RunAsPolicy** defined.</span></span> <span data-ttu-id="e3eaa-194">Om de flesta av hello kod paket som anges i hello tjänstmanifestet som används av ett program behöver toorun under hello samma användare hello program kan bara definiera en standardprincip RunAs med användarkontot.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-194">If most of hello code packages that are specified in hello service manifest used by an application need toorun under hello same user, hello application can just define a default RunAs policy with that user account.</span></span> <span data-ttu-id="e3eaa-195">hello följande exempel anger att om en kodpaketet saknar en **RunAsPolicy** anges hello kodpaketet ska köras under hello **MyDefaultAccount** hello säkerhetsobjekt avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-195">hello following example specifies that if a code package does not have a **RunAsPolicy** specified, hello code package should run under hello **MyDefaultAccount** specified in hello principals section.</span></span>

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a><span data-ttu-id="e3eaa-196">Använda en Active Directory-domän, grupp eller användare</span><span class="sxs-lookup"><span data-stu-id="e3eaa-196">Use an Active Directory domain group or user</span></span>
<span data-ttu-id="e3eaa-197">Du kan köra hello tjänsten hello autentiseringsuppgifterna för ett Active Directory-användare eller grupp för en instans av Service Fabric som har installerats på Windows Server med hjälp av hello fristående installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-197">For an instance of Service Fabric that was installed on Windows Server by using hello standalone installer, you can run hello service under hello credentials for an Active Directory user or group account.</span></span> <span data-ttu-id="e3eaa-198">Detta är Active Directory lokalt i din domän och är inte med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e3eaa-198">This is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e3eaa-199">Genom att använda en domänanvändare eller grupp kan du sedan komma åt andra resurser i hello domän (till exempel filresurser) som har behörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-199">By using a domain user or group, you can then access other resources in hello domain (for example, file shares) that have been granted permissions.</span></span>

<span data-ttu-id="e3eaa-200">hello följande exempel visas en Active Directory-användare som kallas *TestUser* med sin domän lösenord krypteras med hjälp av ett certifikat kallas *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-200">hello following example shows an Active Directory user called *TestUser* with their domain password encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="e3eaa-201">Du kan använda hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello hemliga cipher Kommandotext.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-201">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text.</span></span> <span data-ttu-id="e3eaa-202">Se [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-202">See [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md) for details.</span></span>

<span data-ttu-id="e3eaa-203">Du måste distribuera hello privata nyckeln för hello certifikat toodecrypt hello lösenord toohello lokala datorn med hjälp av en out-of-band-metoden (i Azure, är detta via Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="e3eaa-203">You must deploy hello private key of hello certificate toodecrypt hello password toohello local machine by using an out-of-band method (in Azure, this is via Azure Resource Manager).</span></span> <span data-ttu-id="e3eaa-204">Sedan när Service Fabric distribuerar hello paketet toohello maskin, är kan toodecrypt hello hemlighet och autentisera med Active Directory toorun enligt dessa autentiseringsuppgifter (tillsammans med hello användarnamn).</span><span class="sxs-lookup"><span data-stu-id="e3eaa-204">Then, when Service Fabric deploys hello service package toohello machine, it is able toodecrypt hello secret and (along with hello user name) authenticate with Active Directory toorun under those credentials.</span></span>

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a><span data-ttu-id="e3eaa-205">Använda en grupp för Hanterat tjänstkonto.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-205">Use a Group Managed Service Account.</span></span>
<span data-ttu-id="e3eaa-206">Du kan köra hello service för en instans av Service Fabric som har installerats på Windows Server med hjälp av hello fristående installationsprogram som ett grupphanterat tjänstkonto (gMSA).</span><span class="sxs-lookup"><span data-stu-id="e3eaa-206">For an instance of Service Fabric that was installed on Windows Server by using hello standalone installer, you can run hello service as a group Managed Service Account (gMSA).</span></span> <span data-ttu-id="e3eaa-207">Observera att detta är Active Directory lokalt i din domän och är inte med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e3eaa-207">Note that this is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e3eaa-208">Genom att använda ett gMSA det finns ingen lösenord eller krypterade lösenordet lagras i hello `Application Manifest`.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-208">By using a gMSA there is no password or encrypted password stored in hello `Application Manifest`.</span></span>

<span data-ttu-id="e3eaa-209">hello följande exempel visas hur toocreate ett konto för gMSA kallas *svc-Test$*; hur toodeploy som hanterar service-kontot toohello klusternoder, och hur tooconfigure hello UPN.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-209">hello following example shows how toocreate a gMSA account called *svc-Test$*; how toodeploy that managed service account toohello cluster nodes; and how tooconfigure hello user principal.</span></span>

##### <a name="prerequisites"></a><span data-ttu-id="e3eaa-210">Förutsättningar.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-210">Prerequisites.</span></span>
- <span data-ttu-id="e3eaa-211">hello-domänen måste en KDS-rotnyckel.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-211">hello domain needs a KDS root key.</span></span>
- <span data-ttu-id="e3eaa-212">hello-domänen måste toobe på en Windows Server 2012 eller senare funktionsnivå.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-212">hello domain needs toobe at a Windows Server 2012 or later functional level.</span></span>

##### <a name="example"></a><span data-ttu-id="e3eaa-213">Exempel</span><span class="sxs-lookup"><span data-stu-id="e3eaa-213">Example</span></span>
1. <span data-ttu-id="e3eaa-214">Active directory-domän som administratör skapa en grupp hanteras tjänstkonto som använder hello `New-ADServiceAccount` cmdleten igen och se till att hello `PrincipalsAllowedToRetrieveManagedPassword` innehåller alla klusternoder för hello service fabric.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-214">Have an active directory domain administrator create a group managed service account using hello `New-ADServiceAccount` commandlet and ensure that hello `PrincipalsAllowedToRetrieveManagedPassword` includes all of hello service fabric cluster nodes.</span></span> <span data-ttu-id="e3eaa-215">Observera att `AccountName`, `DnsHostName`, och `ServicePrincipalName` måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-215">Note that `AccountName`, `DnsHostName`, and `ServicePrincipalName` must be unique.</span></span>
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. <span data-ttu-id="e3eaa-216">På varje hello service fabric noder (till exempel `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), installera och testa hello gMSA.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-216">On each of hello service fabric cluster nodes (for example, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), install and test hello gMSA.</span></span>
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. <span data-ttu-id="e3eaa-217">Konfigurera hello UPN och konfigurera hello RunAsPolicy tooreference hello användare.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-217">Configure hello User principal, and configure hello RunAsPolicy tooreference hello user.</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a><span data-ttu-id="e3eaa-218">Tilldela en säkerhetsprincip åtkomst för HTTP och HTTPS-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="e3eaa-218">Assign a security access policy for HTTP and HTTPS endpoints</span></span>
<span data-ttu-id="e3eaa-219">Om du använder en RunAs-princip tooa tjänst och hello tjänstmanifestet deklarerar endpoint resurser med hello HTTP-protokollet, måste du ange en **SecurityAccessPolicy** tooensure att portar allokeras toothese slutpunkter är korrekt åtkomstkontroll listan för hello RunAs-konto som hello-tjänsten körs under.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-219">If you apply a RunAs policy tooa service and hello service manifest declares endpoint resources with hello HTTP protocol, you must specify a **SecurityAccessPolicy** tooensure that ports allocated toothese endpoints are correctly access-control listed for hello RunAs user account that hello service runs under.</span></span> <span data-ttu-id="e3eaa-220">Annars **http.sys** inte har toohello-tjänsten för dataåtkomst och du få fel med anrop från hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-220">Otherwise, **http.sys** does not have access toohello service, and you get failures with calls from hello client.</span></span> <span data-ttu-id="e3eaa-221">hello följande exempel gäller hello Customer1 konto tooan slutpunkt kallas **EndpointName**, vilket ger fullständig behörighet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-221">hello following example applies hello Customer1 account tooan endpoint called **EndpointName**, which gives it full access rights.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

<span data-ttu-id="e3eaa-222">För hello HTTPS-slutpunkt har du också tooindicate hello namnet på hello certifikatklienten tooreturn toohello.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-222">For hello HTTPS endpoint, you also have tooindicate hello name of hello certificate tooreturn toohello client.</span></span> <span data-ttu-id="e3eaa-223">Du kan göra detta med hjälp av **EndpointBindingPolicy**, med hello-certifikat som har angetts i en certifikat-avsnittet i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-223">You can do this by using **EndpointBindingPolicy**, with hello certificate defined in a certificates section in hello application manifest.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if hello EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a><span data-ttu-id="e3eaa-224">Uppgradera flera program med https-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="e3eaa-224">Upgrading multiple applications with https endpoints</span></span>
<span data-ttu-id="e3eaa-225">Du behöver toobe noggrann inte toouse hello **samma port** för olika instanser av hello samma program när du använder http**s**.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-225">You need toobe careful not toouse hello **same port** for different instances of hello same application when using http**s**.</span></span> <span data-ttu-id="e3eaa-226">hello beror på att Service Fabric inte kan tooupgrade hello certifikat för en hello programinstanser.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-226">hello reason is that Service Fabric won't be able tooupgrade hello cert for one of hello application instances.</span></span> <span data-ttu-id="e3eaa-227">Om exempelvis programmet båda 1 eller ett program 2 vill tooupgrade sina certifikat 1 toocert 2.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-227">For example, if application 1 or application 2 both want tooupgrade their cert 1 toocert 2.</span></span> <span data-ttu-id="e3eaa-228">När hello uppgraderingen sker kan Service Fabric ha rensas hello certifikat 1 registrering med http.sys även om hello andra programmet fortfarande använder den.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-228">When hello upgrade happens, Service Fabric might have cleaned up hello cert 1 registration with http.sys even though hello other application is still using it.</span></span> <span data-ttu-id="e3eaa-229">tooprevent, Service Fabric upptäcker att det finns redan en annan programinstansen registrerad på hello port med hello-certifikat (på grund av toohttp.sys) och misslyckas hello igen.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-229">tooprevent this, Service Fabric detects that there is already another application instance registered on hello port with hello certificate (due toohttp.sys) and fails hello operation.</span></span>

<span data-ttu-id="e3eaa-230">Därför Service Fabric stöder inte uppgradering två olika tjänster med hjälp av **hello samma port** i olika programinstanser.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-230">Hence Service Fabric does not support upgrading two different services using **hello same port** in different application instances.</span></span> <span data-ttu-id="e3eaa-231">Med andra ord, du kan inte använda samma certifikat hello på olika tjänster på hello samma port.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-231">In other words, you cannot use hello same certificate on different services on hello same port.</span></span> <span data-ttu-id="e3eaa-232">Om du behöver toohave en delad certifikat på hello samma port tooensure måste den hello services placeras på olika datorer med placeringen.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-232">If you need toohave a shared certificate on hello same port, you need tooensure that hello services are placed on different machines with placement constraints.</span></span> <span data-ttu-id="e3eaa-233">Eller Överväg att använda dynamiska portar för Service Fabric om möjligt för varje tjänst i varje instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-233">Or consider using Service Fabric dynamic ports if possible for each service in each application instance.</span></span> 

<span data-ttu-id="e3eaa-234">Om du ser en uppgradering misslyckas med https, ett fel varning som säger ”hello Windows http-Server API inte stöder flera certifikat för program som delar en port”.</span><span class="sxs-lookup"><span data-stu-id="e3eaa-234">If you see an upgrade fail with https, an error warning saying "hello Windows HTTP Server API does not support multiple certificates for applications that share a port.”</span></span>

## <a name="a-complete-application-manifest-example"></a><span data-ttu-id="e3eaa-235">Ett manifest när hela appen-exempel</span><span class="sxs-lookup"><span data-stu-id="e3eaa-235">A complete application manifest example</span></span>
<span data-ttu-id="e3eaa-236">hello följande programmanifestet visar många hello olika inställningar:</span><span class="sxs-lookup"><span data-stu-id="e3eaa-236">hello following application manifest shows many of hello different settings:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed hello EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="e3eaa-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3eaa-237">Next steps</span></span>
* [<span data-ttu-id="e3eaa-238">Förstå hello programmodell</span><span class="sxs-lookup"><span data-stu-id="e3eaa-238">Understand hello application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="e3eaa-239">Ange resurser i en tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="e3eaa-239">Specify resources in a service manifest</span></span>](service-fabric-service-manifest-resources.md)
* [<span data-ttu-id="e3eaa-240">Distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="e3eaa-240">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
