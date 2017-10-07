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
# <a name="how-tooconfigure-and-run-startup-tasks-for-a-cloud-service"></a>Hur tooconfigure och kör startade aktiviteter för en tjänst i molnet
Du kan använda Start uppgifter tooperform åtgärder innan du startar en roll. Åtgärder som du kanske vill tooperform inkluderar installerar en komponent, registrerar COM-komponenter, ange registernycklar eller starta en tidskrävande process.

> [!NOTE]
> Start uppgifter är inte tillämplig tooVirtual datorer, endast tooCloud Service Web- och arbetsroller.
> 
> 

## <a name="how-startup-tasks-work"></a>Så här fungerar Start uppgifter
Start-aktiviteter är åtgärder som vidtas innan dina roller definieras i hello och börja [ServiceDefinition.csdef] filen med hjälp av hello [aktivitet] element i hello [Start]element. Uppgifter för start är vanliga batch-filer, men de kan även vara konsolprogram eller kommandofiler som startar PowerShell-skript.

Miljövariabler skickar information till en startåtgärd och lokal lagring kan vara används toopass information från en startaktivitet. En miljövariabel kan till exempel ange hello sökvägen tooa program du vill tooinstall och filer kan skrivas toolocal lagring som kan läsas senare sedan av rollerna.

Din startaktivitet kan logga information och fel toohello katalogen som anges av hello **TEMP** miljövariabeln. Under hello startaktivitet hello **TEMP** miljövariabeln löser toohello *C:\\resurser\\temp\\[guid]. [ rolename]\\RoleTemp* katalogen när den körs på hello molnet.

Start-aktiviteter kan också köras flera gånger mellan olika omstarter. Exempelvis hello startaktivitet ska köras varje gång återanvänds hello roll och rollen återanvänder kanske inte alltid med en omstart. Start uppgifter ska skrivas på ett sätt som gör att de toorun flera gånger utan problem.

Start-aktiviteter måste avslutas med en **errorlevel** (eller slutkod) noll för hello startade processen toocomplete. Om en startåtgärd slutar med en icke-noll **errorlevel**, hello roll startar inte.

## <a name="role-startup-order"></a>Rollen startordningen
hello nedan visar hello rollen startade i Azure:

1. hello-instansen har markerats som **Start** och tar inte emot trafik.
2. Alla Start-aktiviteter utförs enligt tootheir **taskType** attribut.
   
   * Hej **enkel** åtgärder körs synkront, en i taget.
   * Hej **bakgrund** och **förgrunden** aktiviteter är igång asynkront, parallella toohello startaktivitet.  
     
     > [!WARNING]
     > IIS kan inte konfigureras fullständigt hello startade uppgiften etapp i hello startprocessen så rollspecifika data är inte kanske tillgänglig. Start-uppgifter som kräver att rollspecifika data ska använda [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).
     > 
     > 
3. värdprocess för hello rollen har startats och hello webbplatsen har skapats i IIS.
4. Hej [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoden anropas.
5. hello-instansen har markerats som **klar** och trafiken dirigeras toohello instans.
6. Hej [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden anropas.

## <a name="example-of-a-startup-task"></a>Exempel på en startåtgärd
Start-aktiviteter har definierats i hello [ServiceDefinition.csdef] -fil hello **aktivitet** element. Hej **commandLine** attribut anger hello namnet och parametrarna för hello Start batch filen eller konsolen kommandot hello **executionContext** attributet anger hello behörighetsnivå av hello Start aktiviteten och hello **taskType** attributet anger hur hello aktivitet körs.

I det här exemplet, en miljövariabel **MyVersionNumber**, är skapade för hello startaktivitet och toohello värdet ”**1.0.0.0**”.

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

I följande exempel hello, hello **Startup.cmd** kommandofil hello rad ”hello aktuella versionen är 1.0.0.0” toohello StartupLog.txt filen skrivs i hello-katalogen som anges av miljövariabeln TEMP för hello. Hej `EXIT /B 0` raden ser till att hello startaktivitet slutar med en **errorlevel** noll.

```cmd
ECHO hello current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> I Visual Studio hello **kopiera tooOutput Directory** egenskapen för kommandofilen start ska anges för**Kopiera alltid** toobe att kommandofilen start är korrekt distribuerad tooyour projektet på Azure (**approot\\bin** för Web-roller och **approot** för arbetsroller).
> 
> 

## <a name="description-of-task-attributes"></a>Beskrivning av uppgiften attribut
hello nedan beskrivs hello attribut för hello **aktivitet** element i hello [ServiceDefinition.csdef] fil:

**commandLine** -anger hello kommandorad för hello startaktivitet:

* hello-kommandot, valfria kommandoradsparametrar, vilket börjar hello startaktivitet.
* Det här är ofta hello namn på en batchfil .cmd och .bat.
* hello uppgift är relativ toohello AppRoot\\Bin-mappen för hello-distribution. Miljövariabler är inte expanderas för att fastställa hello sökvägen och filnamnet för hello-aktivitet. Du kan skapa en liten cmd-skript som anropar din startaktivitet om miljön expansion krävs.
* Kan vara ett konsolprogram eller en batchfil som startar en [PowerShell-skriptet](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -anger hello behörighetsnivå för hello startaktivitet. hello behörighetsnivå kan begränsad eller utökade:

* **begränsad**  
  hello startaktivitet körs med hello samma privilegier som hello roll. När hello **executionContext** attribut för hello [Runtime] element är också **begränsad**, och sedan användarprivilegier som används.
* **upphöjd**  
  hello startaktivitet körs med administratörsbehörighet. Detta gör att starten uppgifter tooinstall program, göra konfigurationsändringar i IIS, utföra ändringar i registret och andra nivån administratörsåtgärder, utan att öka hello behörighetsnivå hello rollen sig själv.  

> [!NOTE]
> hello behörighetsnivå för en startåtgärd behöver inte toobe hello samma som hello roll sig själv.
> 
> 

**taskType** -anger hello sätt startaktivitet körs.

* **enkel**  
  Åtgärder körs synkront, en i taget i hello ordning enligt hello [ServiceDefinition.csdef] fil. När en **enkel** startaktivitet slutar med en **errorlevel** noll hello bredvid **enkel** startaktivitet körs. Om det inte finns några **enkel** Start uppgifter tooexecute, och sedan hello rollen själva startas.   
  
  > [!NOTE]
  > Om hello **enkel** aktivitet slutar med en icke-noll **errorlevel**, hello-instansen kommer att blockeras. Efterföljande **enkel** Start uppgifter och hello roll, startar inte.
  > 
  > 
  
    tooensure batch-fil som slutar med en **errorlevel** noll, köra hello kommandot `EXIT /B 0` hello slutet av din fil batchprocess.
* **bakgrund**  
  Åtgärder körs asynkront, parallellt med hello start av hello roll.
* **förgrunden**  
  Åtgärder körs asynkront, parallellt med hello start av hello roll. Hej viktigaste skillnaden mellan en **förgrunden** och en **bakgrund** uppgift är att en **förgrunden** aktivitet förhindrar hello roll från återvinning eller avslutas förrän hello aktiviteten har avslutades. Hej **bakgrund** uppgifter har inte den här begränsningen.

## <a name="environment-variables"></a>Miljövariabler
Miljövariabler är en startåtgärd för sätt toopass information tooa. Exempelvis kan du placera hello sökvägen tooa blob som innehåller en program-tooinstall eller portnummer som används i din roll eller inställningar toocontrol funktioner i din startaktivitet.

Det finns två typer av miljövariabler för start aktiviteter. statisk miljövariabler och miljövariabler baserat på medlemmar i hello [ RoleEnvironment] klass. Båda värdena i hello [miljö] avsnitt i hello [ServiceDefinition.csdef] fil- och båda Använd hello [variabeln] element och **namn** attribut.

Statisk miljö variabler använder hello **värdet** attribut för hello [variabeln] element. hello-exemplet ovan skapar hello miljövariabeln **MyVersionNumber** som har ett statiskt värde för ”**1.0.0.0**”. Ett annat exempel är toocreate en **StagingOrProduction** miljövariabel som du kan manuellt ange toovalues av ”**mellanlagring**” eller ”**produktion**” tooperform olika start åtgärder baserat på hello värdet för hello **StagingOrProduction** miljövariabeln.

Miljövariabler som baseras på medlemmar av hello RoleEnvironment klassen inte använder hello **värdet** attribut för hello [variabeln] element. I stället hello [RoleInstanceValue] underordnat element med lämpliga hello **XPath** attributvärdet är används toocreate en miljövariabel baserat på en viss medlem i hello [ RoleEnvironment] klass. Värden för hello **XPath** attributet tooaccess olika [ RoleEnvironment] värden finns [här](cloud-services-role-config-xpath.md).

Till exempel toocreate en miljövariabel som är ”**true**” när hello-instansen körs i hello beräkningsemulatorn, och ”**FALSKT**” när den körs i molnet hello använder hello följande [variabeln] och [RoleInstanceValue] element:

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

## <a name="next-steps"></a>Nästa steg
Lär dig hur tooperform vissa [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md) med Molntjänsten.

[Paketet](cloud-services-model-and-package.md) Molntjänsten.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[aktivitet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Start]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[miljö]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[variabeln]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[ RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
