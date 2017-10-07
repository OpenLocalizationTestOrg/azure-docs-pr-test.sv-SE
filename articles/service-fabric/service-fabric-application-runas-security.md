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
# <a name="configure-security-policies-for-your-application"></a>Konfigurera säkerhetsprinciper för ditt program
Du kan skydda program som körs i hello kluster under olika användarkonton med hjälp av Azure Service Fabric. Service Fabric hjälper också till att säkra hello-resurser som används av program när hello distribution under hello användarkonton – till exempel, filer, kataloger och certifikat. Det gör att program som körs, även i en delad värdmiljö säkrare från varandra.

Service Fabric-program körs under hello-konto som hello Fabric.exe processen körs under som standard. Service Fabric ger också hello kapaciteten toorun program under ett lokalt användarkonto eller kontot Lokalt system som anges i hello programmanifestet. Typer av lokala system som stöds är **Lokalanvändare**, **NetworkService**, **LocalService**, och **LocalSystem**.

 När du kör Service Fabric på Windows Server i ditt datacenter med hjälp av hello fristående installationsprogram, använder du Active Directory-domänkonton, inklusive grupphanterade tjänstkonton.

Du kan definiera och skapa användargrupper så att en eller flera användare kan läggas till tooeach grupp toobe hanteras tillsammans. Detta är användbart när det finns flera användare för olika startpunkter och de måste toohave vissa vanliga behörigheter som är tillgängliga på hello gruppnivå.

## <a name="configure-hello-policy-for-a-service-setup-entry-point"></a>Konfigurera hello princip för en startpunkt för installationen av tjänsten
Enligt beskrivningen i hello [programmodell](service-fabric-application-model.md), hello installationsprogrammet startpunkten **SetupEntryPoint**, är en privilegierade startpunkt som körs med samma autentiseringsuppgifter som Service Fabric hello (vanligtvis hello *NetworkService* konto) innan andra startpunkt. hello körbar fil som anges av **EntryPoint** är vanligtvis hello tidskrävande tjänstvärden. Med en separat installationsprogrammet startpunkten så slipper toorun hello tjänstvärden körbara med höga privilegier för längre tid. hello körbara som **EntryPoint** anger körs efter **SetupEntryPoint** avslutas korrekt. hello resulterande processen övervakas och startas om och börjar igen med **SetupEntryPoint** om den aldrig avslutar eller kraschar.

hello följande är en enkel service manifest exempel som visar hello SetupEntryPoint och hello huvudsakliga EntryPoint för hello-tjänsten.

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

### <a name="configure-hello-policy-by-using-a-local-account"></a>Konfigurera hello princip genom att använda ett lokalt konto
Du kan ändra hello säkerhetsbehörigheter som den körs i hello programmanifest när du har konfigurerat hello service toohave en startpunkt för installationen. hello som följande exempel visar hur tooconfigure hello service toorun under användare administratörsbehörighet.

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

Skapa först en **säkerhetsobjekt** avsnitt med ett användarnamn, t.ex SetupAdminUser. Detta anger hello användaren är medlem i systemgruppen för hello administratörer.

Sedan under hello **ServiceManifestImport** avsnittet, konfigurera en princip tooapply detta säkerhetsobjekt för**SetupEntryPoint**. Detta meddelar Service Fabric som när hello **MySetup.bat** fil körs, bör vara `RunAs` med administratörsbehörighet. Med hänsyn till att du har *inte* tillämpas en princip toohello startpunkten, hello koden i **MyServiceHost.exe** körs under hello system **NetworkService** konto. Detta är hello standardkonto som alla startpunkter för tjänsten körs.

Vi ska nu lägga till hello filen MySetup.bat toohello Visual Studio-projekt tootest hello administratörsbehörighet. Högerklicka på hello service-projekt i Visual Studio och lägga till en ny fil med namnet MySetup.bat.

Kontrollera därefter hello MySetup.bat filen ingår i hello service-paketet. Som standard är den inte. Välj hello-fil, högerklicka på tooget hello snabbmenyn och välj **egenskaper**. I egenskapsdialogrutan för hello, kontrollerar du att **kopiera tooOutput Directory** har angetts för**kopiera om nyare**. Se hello följande skärmbild.

![Visual Studio-CopyToOutput för SetupEntryPoint kommandofil][image1]

Öppna hello MySetup.bat filen och Lägg till hello följande kommandon:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set too> out.txt
echo %TestVariable% >> out.txt

REM toodelete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Sedan skapa och distribuera hello lösning tooa lokal utveckling kluster. När hello-tjänsten har startats, som visas i Service Fabric Explorer, kan du se hello MySetup.bat filen lyckades på två sätt. Öppna ett PowerShell-Kommandotolken och skriv:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Anteckna hello namnet hello noden där hello-tjänsten har distribuerats och startas i Service Fabric Explorer – till exempel nod 2. Nu ska du navigera toohello programmet instans mappen toofind hello out.txt arbetsfil som visar hello värdet för **TestVariable**. Till exempel om den här tjänsten har distribuerats tooNode 2, sedan kan du gå toothis sökvägen för hello **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-hello-policy-by-using-local-system-accounts"></a>Konfigurera hello princip genom att använda lokala systemkontona
Det är ofta bättre toorun hello startskript med hjälp av ett lokalt systemkonto i stället för ett administratörskonto. Kör hello RunAs-principen som en medlem i hello administratörsgruppen vanligtvis inte fungerar väl eftersom datorer har User Access Control (UAC) är aktiverat som standard. I sådana fall **hello rekommendation är toorun hello SetupEntryPoint som LocalSystem, i stället för som en lokal användargrupp för tillagda tooAdministrators**. hello följande exempel visar inställningen hello SetupEntryPoint toorun som LocalSystem:

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

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>Starta PowerShell-kommandon från en startpunkt för installationen
toorun PowerShell från hello **SetupEntryPoint** punkt, kan du köra **PowerShell.exe** i en batchfil som pekar tooa PowerShell fil. Lägg först till en PowerShell-filen toohello service-projekt – till exempel **MySetup.ps1**. Kom ihåg tooset hello *kopiera om nyare* egenskapen så att hello-filen ingår också i hello servicepaket. hello följande exempel visas en exempelfil batch som startar en PowerShell-fil som heter MySetup.ps1 som anger en systemmiljövariabler kallas **TestVariable**.

MySetup.bat toostart en PowerShell-fil:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

Lägg till hello följande tooset en systemmiljövariabler i hello PowerShell-filen:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Som standard när hello kommandofilen körs det ser ut i hello programmapp kallas **fungerar** för filer. I det här fallet när MySetup.bat körs, vi vill att den här toofind hello MySetup.ps1 filen i hello samma mapp som är programmet hello **kodpaketet** mapp. toochange den här mappen, ange hello arbetsmapp:
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

## <a name="use-console-redirection-for-local-debugging"></a>Använda omdirigering till konsolen för lokala felsökning
Ibland är det användbart toosee hello konsolens utdata från att köra ett skript för felsökning. toodo, kan du ange en konsol omdirigering av principen som skriver hello tooa utdatafilen. hello filen utdata kallas skriftliga toohello programmappen **loggen** på hello nod där hello programmet har distribuerats och kör. (Se var toofind detta i föregående exempel hello.)

> [!WARNING]
> Använd aldrig hello konsolen omdirigeringspolicyn i ett program som distribuerats i produktionsmiljön eftersom detta kan påverka hello programmet redundans. *Endast* använda detta för lokal utveckling och felsökning.  
> 
> 

hello visar följande exempel inställningen hello omdirigering av konsol med ett FileRetentionCount-värde:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Om du ändrar nu hello MySetup.ps1 filen toowrite en **Echo** kommandot detta skriver toohello utdatafilen för felsökning:

```
Echo "Test console redirection which writes toohello application log folder on hello node that hello application is deployed to"
```

**När du felsöka skriptet omedelbart ta bort den här konsolen omdirigeringspolicyn**.

## <a name="configure-a-policy-for-service-code-packages"></a>Konfigurera en princip för servicepaket för kod
I hello föregående steg, som du såg hur tooapply tooSetupEntryPoint en RunAs-principen. Nu ska vi titta lite djupare hur toocreate olika säkerhetsobjekt som kan användas som tjänsten principer.

### <a name="create-local-user-groups"></a>Skapa lokala användargrupper
Du kan definiera och skapa användargrupper som tillåter att en eller flera användare toobe lägga tooa grupp. Detta är användbart om det finns flera användare för olika startpunkter och de måste toohave vissa vanliga behörigheter som är tillgängliga på hello gruppnivå. hello följande exempel visas en lokal grupp som kallas **LocalAdminGroup** som har administratörsbehörighet. Två användare, Customer1 och Customer2, som blir medlemmar i den lokala gruppen.

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

### <a name="create-local-users"></a>Skapa lokala användare
Du kan skapa en lokal användare som kan använda toohelp säker en tjänst i hello-programmet. När en **Lokalanvändare** kontotyp som anges under hello huvudobjekt Hej programmanifestet Service Fabric skapar lokala användarkonton på datorer där hello programmet distribueras. Som standard har dessa konton inte hello samma namn som de som anges i programmet hello manifest (till exempel Customer3 i följande exempel hello). I stället de genereras dynamiskt och ha slumpmässiga lösenord.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Om ett program kräver att hello användarkonto och lösenord vara samma på alla datorer (till exempel tooenable NTLM-autentisering), ange hello klustermanifestet NTLMAuthenticationEnabled tootrue. Hej klustermanifestet måste också ange en NTLMAuthenticationPasswordSecret som används för toogenerate hello samma lösenord på alla datorer.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-toohello-service-code-packages"></a>Tilldela principer toohello kod servicepaket
Hej **RunAsPolicy** avsnittet för en **ServiceManifestImport** anger hello konto från hello säkerhetsobjekt avsnitt som ska använda toorun en kodpaketet. Paket från hello service manifest kod associerar även med användarkonton i hello säkerhetsobjekt avsnitt. Du kan ange detta för hello installations- eller huvudsakliga startpunkter eller ange `All` tooapply den tooboth. hello visar följande exempel olika principer tillämpas:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Om **EntryPointType** anges hello standard anges tooEntryPointType = ”Main”. Ange **SetupEntryPoint** är särskilt användbar när du vill toorun vissa höga installationsprogrammet åtgärder under en system-kontot. hello faktiska Tjänstkod kan köras under ett konto med lägre behörighet.

### <a name="apply-a-default-policy-tooall-service-code-packages"></a>Tillämpa en standard princip tooall kod servicepaket
Du använder hello **DefaultRunAsPolicy** avsnittet toospecify ett användarkonto för standard för alla kod paket som inte har en specifik **RunAsPolicy** definieras. Om de flesta av hello kod paket som anges i hello tjänstmanifestet som används av ett program behöver toorun under hello samma användare hello program kan bara definiera en standardprincip RunAs med användarkontot. hello följande exempel anger att om en kodpaketet saknar en **RunAsPolicy** anges hello kodpaketet ska köras under hello **MyDefaultAccount** hello säkerhetsobjekt avsnitt.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Använda en Active Directory-domän, grupp eller användare
Du kan köra hello tjänsten hello autentiseringsuppgifterna för ett Active Directory-användare eller grupp för en instans av Service Fabric som har installerats på Windows Server med hjälp av hello fristående installationsprogram. Detta är Active Directory lokalt i din domän och är inte med Azure Active Directory (AD Azure). Genom att använda en domänanvändare eller grupp kan du sedan komma åt andra resurser i hello domän (till exempel filresurser) som har behörighet.

hello följande exempel visas en Active Directory-användare som kallas *TestUser* med sin domän lösenord krypteras med hjälp av ett certifikat kallas *MyCert*. Du kan använda hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello hemliga cipher Kommandotext. Se [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md) mer information.

Du måste distribuera hello privata nyckeln för hello certifikat toodecrypt hello lösenord toohello lokala datorn med hjälp av en out-of-band-metoden (i Azure, är detta via Azure Resource Manager). Sedan när Service Fabric distribuerar hello paketet toohello maskin, är kan toodecrypt hello hemlighet och autentisera med Active Directory toorun enligt dessa autentiseringsuppgifter (tillsammans med hello användarnamn).

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
### <a name="use-a-group-managed-service-account"></a>Använda en grupp för Hanterat tjänstkonto.
Du kan köra hello service för en instans av Service Fabric som har installerats på Windows Server med hjälp av hello fristående installationsprogram som ett grupphanterat tjänstkonto (gMSA). Observera att detta är Active Directory lokalt i din domän och är inte med Azure Active Directory (AD Azure). Genom att använda ett gMSA det finns ingen lösenord eller krypterade lösenordet lagras i hello `Application Manifest`.

hello följande exempel visas hur toocreate ett konto för gMSA kallas *svc-Test$*; hur toodeploy som hanterar service-kontot toohello klusternoder, och hur tooconfigure hello UPN.

##### <a name="prerequisites"></a>Förutsättningar.
- hello-domänen måste en KDS-rotnyckel.
- hello-domänen måste toobe på en Windows Server 2012 eller senare funktionsnivå.

##### <a name="example"></a>Exempel
1. Active directory-domän som administratör skapa en grupp hanteras tjänstkonto som använder hello `New-ADServiceAccount` cmdleten igen och se till att hello `PrincipalsAllowedToRetrieveManagedPassword` innehåller alla klusternoder för hello service fabric. Observera att `AccountName`, `DnsHostName`, och `ServicePrincipalName` måste vara unika.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. På varje hello service fabric noder (till exempel `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), installera och testa hello gMSA.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. Konfigurera hello UPN och konfigurera hello RunAsPolicy tooreference hello användare.
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

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Tilldela en säkerhetsprincip åtkomst för HTTP och HTTPS-slutpunkter
Om du använder en RunAs-princip tooa tjänst och hello tjänstmanifestet deklarerar endpoint resurser med hello HTTP-protokollet, måste du ange en **SecurityAccessPolicy** tooensure att portar allokeras toothese slutpunkter är korrekt åtkomstkontroll listan för hello RunAs-konto som hello-tjänsten körs under. Annars **http.sys** inte har toohello-tjänsten för dataåtkomst och du få fel med anrop från hello-klienten. hello följande exempel gäller hello Customer1 konto tooan slutpunkt kallas **EndpointName**, vilket ger fullständig behörighet.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

För hello HTTPS-slutpunkt har du också tooindicate hello namnet på hello certifikatklienten tooreturn toohello. Du kan göra detta med hjälp av **EndpointBindingPolicy**, med hello-certifikat som har angetts i en certifikat-avsnittet i hello programmanifestet.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if hello EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Uppgradera flera program med https-slutpunkter
Du behöver toobe noggrann inte toouse hello **samma port** för olika instanser av hello samma program när du använder http**s**. hello beror på att Service Fabric inte kan tooupgrade hello certifikat för en hello programinstanser. Om exempelvis programmet båda 1 eller ett program 2 vill tooupgrade sina certifikat 1 toocert 2. När hello uppgraderingen sker kan Service Fabric ha rensas hello certifikat 1 registrering med http.sys även om hello andra programmet fortfarande använder den. tooprevent, Service Fabric upptäcker att det finns redan en annan programinstansen registrerad på hello port med hello-certifikat (på grund av toohttp.sys) och misslyckas hello igen.

Därför Service Fabric stöder inte uppgradering två olika tjänster med hjälp av **hello samma port** i olika programinstanser. Med andra ord, du kan inte använda samma certifikat hello på olika tjänster på hello samma port. Om du behöver toohave en delad certifikat på hello samma port tooensure måste den hello services placeras på olika datorer med placeringen. Eller Överväg att använda dynamiska portar för Service Fabric om möjligt för varje tjänst i varje instans av programmet. 

Om du ser en uppgradering misslyckas med https, ett fel varning som säger ”hello Windows http-Server API inte stöder flera certifikat för program som delar en port”.

## <a name="a-complete-application-manifest-example"></a>Ett manifest när hela appen-exempel
hello följande programmanifestet visar många hello olika inställningar:

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
## <a name="next-steps"></a>Nästa steg
* [Förstå hello programmodell](service-fabric-application-model.md)
* [Ange resurser i en tjänstmanifestet](service-fabric-service-manifest-resources.md)
* [Distribuera ett program](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
