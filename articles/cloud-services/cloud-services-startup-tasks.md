---
title: aaaRun Start uppgifter i Azure Cloud Services | Microsoft Docs
description: "Start hjälper förbereda din molntjänstmiljö för din app. Det här lär du dig hur start uppgifter för arbete och toomake dem."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 3391a5d7434164f59972b8e497e5c34e33409543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-and-run-startup-tasks-for-a-cloud-service"></a><span data-ttu-id="848a1-104">Hur tooconfigure och kör startade aktiviteter för en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="848a1-104">How tooconfigure and run startup tasks for a cloud service</span></span>
<span data-ttu-id="848a1-105">Du kan använda Start uppgifter tooperform åtgärder innan du startar en roll.</span><span class="sxs-lookup"><span data-stu-id="848a1-105">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="848a1-106">Åtgärder som du kanske vill tooperform inkluderar installerar en komponent, registrerar COM-komponenter, ange registernycklar eller starta en tidskrävande process.</span><span class="sxs-lookup"><span data-stu-id="848a1-106">Operations that you might want tooperform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span>

> [!NOTE]
> <span data-ttu-id="848a1-107">Start uppgifter är inte tillämplig tooVirtual datorer, endast tooCloud Service Web- och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="848a1-107">Startup tasks are not applicable tooVirtual Machines, only tooCloud Service Web and Worker roles.</span></span>
> 
> 

## <a name="how-startup-tasks-work"></a><span data-ttu-id="848a1-108">Så här fungerar Start uppgifter</span><span class="sxs-lookup"><span data-stu-id="848a1-108">How startup tasks work</span></span>
<span data-ttu-id="848a1-109">Start-aktiviteter är åtgärder som vidtas innan dina roller definieras i hello och börja [ServiceDefinition.csdef] filen med hjälp av hello [aktivitet] element i hello [Start]element.</span><span class="sxs-lookup"><span data-stu-id="848a1-109">Startup tasks are actions that are taken before your roles begin and are defined in hello [ServiceDefinition.csdef] file by using hello [Task] element within hello [Startup] element.</span></span> <span data-ttu-id="848a1-110">Uppgifter för start är vanliga batch-filer, men de kan även vara konsolprogram eller kommandofiler som startar PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="848a1-110">Frequently startup tasks are batch files, but they can also be console applications, or batch files that start PowerShell scripts.</span></span>

<span data-ttu-id="848a1-111">Miljövariabler skickar information till en startåtgärd och lokal lagring kan vara används toopass information från en startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="848a1-111">Environment variables pass information into a startup task, and local storage can be used toopass information out of a startup task.</span></span> <span data-ttu-id="848a1-112">En miljövariabel kan till exempel ange hello sökvägen tooa program du vill tooinstall och filer kan skrivas toolocal lagring som kan läsas senare sedan av rollerna.</span><span class="sxs-lookup"><span data-stu-id="848a1-112">For example, an environment variable can specify hello path tooa program you want tooinstall, and files can be written toolocal storage that can then be read later by your roles.</span></span>

<span data-ttu-id="848a1-113">Din startaktivitet kan logga information och fel toohello katalogen som anges av hello **TEMP** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="848a1-113">Your startup task can log information and errors toohello directory specified by hello **TEMP** environment variable.</span></span> <span data-ttu-id="848a1-114">Under hello startaktivitet hello **TEMP** miljövariabeln löser toohello *C:\\resurser\\temp\\[guid]. [ rolename]\\RoleTemp* katalogen när den körs på hello molnet.</span><span class="sxs-lookup"><span data-stu-id="848a1-114">During hello startup task, hello **TEMP** environment variable resolves toohello *C:\\Resources\\temp\\[guid].[rolename]\\RoleTemp* directory when running on hello cloud.</span></span>

<span data-ttu-id="848a1-115">Start-aktiviteter kan också köras flera gånger mellan olika omstarter.</span><span class="sxs-lookup"><span data-stu-id="848a1-115">Startup tasks can also be executed several times between reboots.</span></span> <span data-ttu-id="848a1-116">Exempelvis hello startaktivitet ska köras varje gång återanvänds hello roll och rollen återanvänder kanske inte alltid med en omstart.</span><span class="sxs-lookup"><span data-stu-id="848a1-116">For example, hello startup task will be run each time hello role recycles, and role recycles may not always include a reboot.</span></span> <span data-ttu-id="848a1-117">Start uppgifter ska skrivas på ett sätt som gör att de toorun flera gånger utan problem.</span><span class="sxs-lookup"><span data-stu-id="848a1-117">Startup tasks should be written in a way that allows them toorun several times without problems.</span></span>

<span data-ttu-id="848a1-118">Start-aktiviteter måste avslutas med en **errorlevel** (eller slutkod) noll för hello startade processen toocomplete.</span><span class="sxs-lookup"><span data-stu-id="848a1-118">Startup tasks must end with an **errorlevel** (or exit code) of zero for hello startup process toocomplete.</span></span> <span data-ttu-id="848a1-119">Om en startåtgärd slutar med en icke-noll **errorlevel**, hello roll startar inte.</span><span class="sxs-lookup"><span data-stu-id="848a1-119">If a startup task ends with a non-zero **errorlevel**, hello role will not start.</span></span>

## <a name="role-startup-order"></a><span data-ttu-id="848a1-120">Rollen startordningen</span><span class="sxs-lookup"><span data-stu-id="848a1-120">Role startup order</span></span>
<span data-ttu-id="848a1-121">hello nedan visar hello rollen startade i Azure:</span><span class="sxs-lookup"><span data-stu-id="848a1-121">hello following lists hello role startup procedure in Azure:</span></span>

1. <span data-ttu-id="848a1-122">hello-instansen har markerats som **Start** och tar inte emot trafik.</span><span class="sxs-lookup"><span data-stu-id="848a1-122">hello instance is marked as **Starting** and does not receive traffic.</span></span>
2. <span data-ttu-id="848a1-123">Alla Start-aktiviteter utförs enligt tootheir **taskType** attribut.</span><span class="sxs-lookup"><span data-stu-id="848a1-123">All startup tasks are executed according tootheir **taskType** attribute.</span></span>
   
   * <span data-ttu-id="848a1-124">Hej **enkel** åtgärder körs synkront, en i taget.</span><span class="sxs-lookup"><span data-stu-id="848a1-124">hello **simple** tasks are executed synchronously, one at a time.</span></span>
   * <span data-ttu-id="848a1-125">Hej **bakgrund** och **förgrunden** aktiviteter är igång asynkront, parallella toohello startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="848a1-125">hello **background** and **foreground** tasks are started asynchronously, parallel toohello startup task.</span></span>  
     
     > [!WARNING]
     > <span data-ttu-id="848a1-126">IIS kan inte konfigureras fullständigt hello startade uppgiften etapp i hello startprocessen så rollspecifika data är inte kanske tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="848a1-126">IIS may not be fully configured during hello startup task stage in hello startup process, so role-specific data may not be available.</span></span> <span data-ttu-id="848a1-127">Start-uppgifter som kräver att rollspecifika data ska använda [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span><span class="sxs-lookup"><span data-stu-id="848a1-127">Startup tasks that require role-specific data should use [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span></span>
     > 
     > 
3. <span data-ttu-id="848a1-128">värdprocess för hello rollen har startats och hello webbplatsen har skapats i IIS.</span><span class="sxs-lookup"><span data-stu-id="848a1-128">hello role host process is started and hello site is created in IIS.</span></span>
4. <span data-ttu-id="848a1-129">Hej [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="848a1-129">hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method is called.</span></span>
5. <span data-ttu-id="848a1-130">hello-instansen har markerats som **klar** och trafiken dirigeras toohello instans.</span><span class="sxs-lookup"><span data-stu-id="848a1-130">hello instance is marked as **Ready** and traffic is routed toohello instance.</span></span>
6. <span data-ttu-id="848a1-131">Hej [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="848a1-131">hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method is called.</span></span>

## <a name="example-of-a-startup-task"></a><span data-ttu-id="848a1-132">Exempel på en startåtgärd</span><span class="sxs-lookup"><span data-stu-id="848a1-132">Example of a startup task</span></span>
<span data-ttu-id="848a1-133">Start-aktiviteter har definierats i hello [ServiceDefinition.csdef] -fil hello **aktivitet** element.</span><span class="sxs-lookup"><span data-stu-id="848a1-133">Startup tasks are defined in hello [ServiceDefinition.csdef] file, in hello **Task** element.</span></span> <span data-ttu-id="848a1-134">Hej **commandLine** attribut anger hello namnet och parametrarna för hello Start batch filen eller konsolen kommandot hello **executionContext** attributet anger hello behörighetsnivå av hello Start aktiviteten och hello **taskType** attributet anger hur hello aktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="848a1-134">hello **commandLine** attribute specifies hello name and parameters of hello startup batch file or console command, hello **executionContext** attribute specifies hello privilege level of hello startup task, and hello **taskType** attribute specifies how hello task will be executed.</span></span>

<span data-ttu-id="848a1-135">I det här exemplet, en miljövariabel **MyVersionNumber**, är skapade för hello startaktivitet och toohello värdet ”**1.0.0.0**”.</span><span class="sxs-lookup"><span data-stu-id="848a1-135">In this example, an environment variable, **MyVersionNumber**, is created for hello startup task and set toohello value "**1.0.0.0**".</span></span>

<span data-ttu-id="848a1-136">**ServiceDefinition.csdef**:</span><span class="sxs-lookup"><span data-stu-id="848a1-136">**ServiceDefinition.csdef**:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

<span data-ttu-id="848a1-137">I följande exempel hello, hello **Startup.cmd** kommandofil hello rad ”hello aktuella versionen är 1.0.0.0” toohello StartupLog.txt filen skrivs i hello-katalogen som anges av miljövariabeln TEMP för hello.</span><span class="sxs-lookup"><span data-stu-id="848a1-137">In hello following example, hello **Startup.cmd** batch file writes hello line "hello current version is 1.0.0.0" toohello StartupLog.txt file in hello directory specified by hello TEMP environment variable.</span></span> <span data-ttu-id="848a1-138">Hej `EXIT /B 0` raden ser till att hello startaktivitet slutar med en **errorlevel** noll.</span><span class="sxs-lookup"><span data-stu-id="848a1-138">hello `EXIT /B 0` line ensures that hello startup task ends with an **errorlevel** of zero.</span></span>

```cmd
ECHO hello current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> <span data-ttu-id="848a1-139">I Visual Studio hello **kopiera tooOutput Directory** egenskapen för kommandofilen start ska anges för**Kopiera alltid** toobe att kommandofilen start är korrekt distribuerad tooyour projektet på Azure (**approot\\bin** för Web-roller och **approot** för arbetsroller).</span><span class="sxs-lookup"><span data-stu-id="848a1-139">In Visual Studio, hello **Copy tooOutput Directory** property for your startup batch file should be set too**Copy Always** toobe sure that your startup batch file is properly deployed tooyour project on Azure (**approot\\bin** for Web roles, and **approot** for worker roles).</span></span>
> 
> 

## <a name="description-of-task-attributes"></a><span data-ttu-id="848a1-140">Beskrivning av uppgiften attribut</span><span class="sxs-lookup"><span data-stu-id="848a1-140">Description of task attributes</span></span>
<span data-ttu-id="848a1-141">hello nedan beskrivs hello attribut för hello **aktivitet** element i hello [ServiceDefinition.csdef] fil:</span><span class="sxs-lookup"><span data-stu-id="848a1-141">hello following describes hello attributes of hello **Task** element in hello [ServiceDefinition.csdef] file:</span></span>

<span data-ttu-id="848a1-142">**commandLine** -anger hello kommandorad för hello startaktivitet:</span><span class="sxs-lookup"><span data-stu-id="848a1-142">**commandLine** - Specifies hello command line for hello startup task:</span></span>

* <span data-ttu-id="848a1-143">hello-kommandot, valfria kommandoradsparametrar, vilket börjar hello startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="848a1-143">hello command, with optional command line parameters, which begins hello startup task.</span></span>
* <span data-ttu-id="848a1-144">Det här är ofta hello namn på en batchfil .cmd och .bat.</span><span class="sxs-lookup"><span data-stu-id="848a1-144">Frequently this is hello filename of a .cmd or .bat batch file.</span></span>
* <span data-ttu-id="848a1-145">hello uppgift är relativ toohello AppRoot\\Bin-mappen för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="848a1-145">hello task is relative toohello AppRoot\\Bin folder for hello deployment.</span></span> <span data-ttu-id="848a1-146">Miljövariabler är inte expanderas för att fastställa hello sökvägen och filnamnet för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="848a1-146">Environment variables are not expanded in determining hello path and file of hello task.</span></span> <span data-ttu-id="848a1-147">Du kan skapa en liten cmd-skript som anropar din startaktivitet om miljön expansion krävs.</span><span class="sxs-lookup"><span data-stu-id="848a1-147">If environment expansion is required, you can create a small .cmd script that calls your startup task.</span></span>
* <span data-ttu-id="848a1-148">Kan vara ett konsolprogram eller en batchfil som startar en [PowerShell-skriptet](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span><span class="sxs-lookup"><span data-stu-id="848a1-148">Can be a console application or a batch file that starts a [PowerShell script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span></span>

<span data-ttu-id="848a1-149">**executionContext** -anger hello behörighetsnivå för hello startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="848a1-149">**executionContext** - Specifies hello privilege level for hello startup task.</span></span> <span data-ttu-id="848a1-150">hello behörighetsnivå kan begränsad eller utökade:</span><span class="sxs-lookup"><span data-stu-id="848a1-150">hello privilege level can be limited or elevated:</span></span>

* <span data-ttu-id="848a1-151">**begränsad**</span><span class="sxs-lookup"><span data-stu-id="848a1-151">**limited**</span></span>  
  <span data-ttu-id="848a1-152">hello startaktivitet körs med hello samma privilegier som hello roll.</span><span class="sxs-lookup"><span data-stu-id="848a1-152">hello startup task runs with hello same privileges as hello role.</span></span> <span data-ttu-id="848a1-153">När hello **executionContext** attribut för hello [Runtime] element är också **begränsad**, och sedan användarprivilegier som används.</span><span class="sxs-lookup"><span data-stu-id="848a1-153">When hello **executionContext** attribute for hello [Runtime] element is also **limited**, then user privileges are used.</span></span>
* <span data-ttu-id="848a1-154">**upphöjd**</span><span class="sxs-lookup"><span data-stu-id="848a1-154">**elevated**</span></span>  
  <span data-ttu-id="848a1-155">hello startaktivitet körs med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="848a1-155">hello startup task runs with administrator privileges.</span></span> <span data-ttu-id="848a1-156">Detta gör att starten uppgifter tooinstall program, göra konfigurationsändringar i IIS, utföra ändringar i registret och andra nivån administratörsåtgärder, utan att öka hello behörighetsnivå hello rollen sig själv.</span><span class="sxs-lookup"><span data-stu-id="848a1-156">This allows startup tasks tooinstall programs, make IIS configuration changes, perform registry changes, and other administrator level tasks, without increasing hello privilege level of hello role itself.</span></span>  

> [!NOTE]
> <span data-ttu-id="848a1-157">hello behörighetsnivå för en startåtgärd behöver inte toobe hello samma som hello roll sig själv.</span><span class="sxs-lookup"><span data-stu-id="848a1-157">hello privilege level of a startup task does not need toobe hello same as hello role itself.</span></span>
> 
> 

<span data-ttu-id="848a1-158">**taskType** -anger hello sätt startaktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="848a1-158">**taskType** - Specifies hello way a startup task is executed.</span></span>

* <span data-ttu-id="848a1-159">**enkel**</span><span class="sxs-lookup"><span data-stu-id="848a1-159">**simple**</span></span>  
  <span data-ttu-id="848a1-160">Åtgärder körs synkront, en i taget i hello ordning enligt hello [ServiceDefinition.csdef] fil.</span><span class="sxs-lookup"><span data-stu-id="848a1-160">Tasks are executed synchronously, one at a time, in hello order specified in hello [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="848a1-161">När en **enkel** startaktivitet slutar med en **errorlevel** noll hello bredvid **enkel** startaktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="848a1-161">When one **simple** startup task ends with an **errorlevel** of zero, hello next **simple** startup task is executed.</span></span> <span data-ttu-id="848a1-162">Om det inte finns några **enkel** Start uppgifter tooexecute, och sedan hello rollen själva startas.</span><span class="sxs-lookup"><span data-stu-id="848a1-162">If there are no more **simple** startup tasks tooexecute, then hello role itself will be started.</span></span>   
  
  > [!NOTE]
  > <span data-ttu-id="848a1-163">Om hello **enkel** aktivitet slutar med en icke-noll **errorlevel**, hello-instansen kommer att blockeras.</span><span class="sxs-lookup"><span data-stu-id="848a1-163">If hello **simple** task ends with a non-zero **errorlevel**, hello instance will be blocked.</span></span> <span data-ttu-id="848a1-164">Efterföljande **enkel** Start uppgifter och hello roll, startar inte.</span><span class="sxs-lookup"><span data-stu-id="848a1-164">Subsequent **simple** startup tasks, and hello role itself, will not start.</span></span>
  > 
  > 
  
    <span data-ttu-id="848a1-165">tooensure batch-fil som slutar med en **errorlevel** noll, köra hello kommandot `EXIT /B 0` hello slutet av din fil batchprocess.</span><span class="sxs-lookup"><span data-stu-id="848a1-165">tooensure that your batch file ends with an **errorlevel** of zero, execute hello command `EXIT /B 0` at hello end of your batch file process.</span></span>
* <span data-ttu-id="848a1-166">**bakgrund**</span><span class="sxs-lookup"><span data-stu-id="848a1-166">**background**</span></span>  
  <span data-ttu-id="848a1-167">Åtgärder körs asynkront, parallellt med hello start av hello roll.</span><span class="sxs-lookup"><span data-stu-id="848a1-167">Tasks are executed asynchronously, in parallel with hello startup of hello role.</span></span>
* <span data-ttu-id="848a1-168">**förgrunden**</span><span class="sxs-lookup"><span data-stu-id="848a1-168">**foreground**</span></span>  
  <span data-ttu-id="848a1-169">Åtgärder körs asynkront, parallellt med hello start av hello roll.</span><span class="sxs-lookup"><span data-stu-id="848a1-169">Tasks are executed asynchronously, in parallel with hello startup of hello role.</span></span> <span data-ttu-id="848a1-170">Hej viktigaste skillnaden mellan en **förgrunden** och en **bakgrund** uppgift är att en **förgrunden** aktivitet förhindrar hello roll från återvinning eller avslutas förrän hello aktiviteten har avslutades.</span><span class="sxs-lookup"><span data-stu-id="848a1-170">hello key difference between a **foreground** and a **background** task is that a **foreground** task prevents hello role from recycling or shutting down until hello task has ended.</span></span> <span data-ttu-id="848a1-171">Hej **bakgrund** uppgifter har inte den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="848a1-171">hello **background** tasks do not have this restriction.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="848a1-172">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="848a1-172">Environment variables</span></span>
<span data-ttu-id="848a1-173">Miljövariabler är en startåtgärd för sätt toopass information tooa.</span><span class="sxs-lookup"><span data-stu-id="848a1-173">Environment variables are a way toopass information tooa startup task.</span></span> <span data-ttu-id="848a1-174">Exempelvis kan du placera hello sökvägen tooa blob som innehåller en program-tooinstall eller portnummer som används i din roll eller inställningar toocontrol funktioner i din startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="848a1-174">For example, you can put hello path tooa blob that contains a program tooinstall, or port numbers that your role will use, or settings toocontrol features of your startup task.</span></span>

<span data-ttu-id="848a1-175">Det finns två typer av miljövariabler för start aktiviteter. statisk miljövariabler och miljövariabler baserat på medlemmar i hello [ RoleEnvironment] klass.</span><span class="sxs-lookup"><span data-stu-id="848a1-175">There are two kinds of environment variables for startup tasks; static environment variables and environment variables based on members of hello [RoleEnvironment] class.</span></span> <span data-ttu-id="848a1-176">Båda värdena i hello [miljö] avsnitt i hello [ServiceDefinition.csdef] fil- och båda Använd hello [variabeln] element och **namn** attribut.</span><span class="sxs-lookup"><span data-stu-id="848a1-176">Both are in hello [Environment] section of hello [ServiceDefinition.csdef] file, and both use hello [Variable] element and **name** attribute.</span></span>

<span data-ttu-id="848a1-177">Statisk miljö variabler använder hello **värdet** attribut för hello [variabeln] element.</span><span class="sxs-lookup"><span data-stu-id="848a1-177">Static environment variables uses hello **value** attribute of hello [Variable] element.</span></span> <span data-ttu-id="848a1-178">hello-exemplet ovan skapar hello miljövariabeln **MyVersionNumber** som har ett statiskt värde för ”**1.0.0.0**”.</span><span class="sxs-lookup"><span data-stu-id="848a1-178">hello example above creates hello environment variable **MyVersionNumber** which has a static value of "**1.0.0.0**".</span></span> <span data-ttu-id="848a1-179">Ett annat exempel är toocreate en **StagingOrProduction** miljövariabel som du kan manuellt ange toovalues av ”**mellanlagring**” eller ”**produktion**” tooperform olika start åtgärder baserat på hello värdet för hello **StagingOrProduction** miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="848a1-179">Another example would be toocreate a **StagingOrProduction** environment variable which you can manually set toovalues of "**staging**" or "**production**" tooperform different startup actions based on hello value of hello **StagingOrProduction** environment variable.</span></span>

<span data-ttu-id="848a1-180">Miljövariabler som baseras på medlemmar av hello RoleEnvironment klassen inte använder hello **värdet** attribut för hello [variabeln] element.</span><span class="sxs-lookup"><span data-stu-id="848a1-180">Environment variables based on members of hello RoleEnvironment class do not use hello **value** attribute of hello [Variable] element.</span></span> <span data-ttu-id="848a1-181">I stället hello [RoleInstanceValue] underordnat element med lämpliga hello **XPath** attributvärdet är används toocreate en miljövariabel baserat på en viss medlem i hello [ RoleEnvironment] klass.</span><span class="sxs-lookup"><span data-stu-id="848a1-181">Instead, hello [RoleInstanceValue] child element, with hello appropriate **XPath** attribute value, are used toocreate an environment variable based on a specific member of hello [RoleEnvironment] class.</span></span> <span data-ttu-id="848a1-182">Värden för hello **XPath** attributet tooaccess olika [ RoleEnvironment] värden finns [här](cloud-services-role-config-xpath.md).</span><span class="sxs-lookup"><span data-stu-id="848a1-182">Values for hello **XPath** attribute tooaccess various [RoleEnvironment] values can be found [here](cloud-services-role-config-xpath.md).</span></span>

<span data-ttu-id="848a1-183">Till exempel toocreate en miljövariabel som är ”**true**” när hello-instansen körs i hello beräkningsemulatorn, och ”**FALSKT**” när den körs i molnet hello använder hello följande [variabeln] och [RoleInstanceValue] element:</span><span class="sxs-lookup"><span data-stu-id="848a1-183">For example, toocreate an environment variable that is "**true**" when hello instance is running in hello compute emulator, and "**false**" when running in hello cloud, use hello following [Variable] and [RoleInstanceValue] elements:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create hello environment variable that informs hello startup task whether it is running
                in hello Compute Emulator or in hello cloud. "%ComputeEmulatorRunning%"=="true" when
                running in hello Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in hello cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a><span data-ttu-id="848a1-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="848a1-184">Next steps</span></span>
<span data-ttu-id="848a1-185">Lär dig hur tooperform vissa [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md) med Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="848a1-185">Learn how tooperform some [common startup tasks](cloud-services-startup-tasks-common.md) with your Cloud Service.</span></span>

<span data-ttu-id="848a1-186">[Paketet](cloud-services-model-and-package.md) Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="848a1-186">[Package](cloud-services-model-and-package.md) your Cloud Service.</span></span>  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[aktivitet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Start]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[miljö]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[variabeln]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[ RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
