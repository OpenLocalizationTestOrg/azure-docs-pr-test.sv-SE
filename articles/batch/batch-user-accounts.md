---
title: "aaaRun uppgifter under användarkonton i Azure Batch | Microsoft Docs"
description: "Konfigurera användarkonton för att köra uppgifter i Azure Batch"
services: batch
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.openlocfilehash: 13d7d76451d89a3cca090c4ef24ed0ed781bbf09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a>Köra uppgifter under användarkonton i Batch

En aktivitet i Azure Batch körs alltid under ett användarkonto. Som standard körs uppgifter under standardanvändarkonton utan administratörsbehörighet. Dessa standardinställningarna räcker oftast. För vissa scenarier men är det användbart toobe kan tooconfigure hello användarkonto under vilket du vill att en aktivitet toorun. Den här artikeln beskrivs hello typerna av användarkonton och hur du kan konfigurera dem för ditt scenario.

## <a name="types-of-user-accounts"></a>Typer av användarkonton

Azure Batch tillhandahåller två typer av användarkonton för pågående aktiviteter:

- **Automatisk användarkonton.** Auto-användarkonton är inbyggda användarkonton som skapas automatiskt av hello Batch-tjänsten. Som standard körs aktiviteter under en automatisk användarkonto. Du kan konfigurera hello automatiskt användaren specifikationen för en aktivitet tooindicate konto en aktivitet ska köras under vilken auto-användare. hello automatiskt användaren specifikation kan du toospecify hello höjning nivå och omfattningen av hello automatiskt-användarkonto som ska köra hello-aktivitet. 

- **En namngiven användarkonto.** Du kan ange en eller flera namngivna användarkonton för en pool när du skapar hello-adresspool. Varje användarkonto skapas på varje nod i hello poolen. Dessutom toohello kontonamn kan du ange hello lösenord, höjning nivå och, för Linux pooler hello SSH privat nyckel. När du lägger till en aktivitet kan du ange hello med namnet konto som aktiviteten ska köras under.

> [!IMPORTANT] 
> hello Batch-tjänstversion 2017-01-01.4.0 introducerar en ny ändring som kräver att du uppdaterar din kod toocall den här versionen. Om du migrerar kod från en äldre version av Batch, Observera att hello **runElevated** egenskapen stöds inte längre i hello REST API eller Batch klientbibliotek. Använd hello nya **userIdentity** -egenskapen för en aktivitet toospecify höjning nivå. Finns avsnittet hello [uppdatera din kod toohello senaste Batch klientbiblioteket](#update-your-code-to-the-latest-batch-client-library) snabb riktlinjer för uppdatering av Batch-kod om du använder en av hello klientbibliotek.
>
>

> [!NOTE] 
> hello användarkonton i den här artikeln stöder inte Remote Desktop Protocol (RDP) eller SSH (Secure Shell), av säkerhetsskäl. 
>
> tooconnect tooa körs hello Linux virtuella nodkonfiguration via SSH, se [Använd Fjärrskrivbord tooa Linux VM i Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). tooconnect toonodes som kör Windows via RDP, se [ansluta tooa Windows Server VM](../virtual-machines/windows/connect-logon.md).<br /><br />
> tooconnect tooa noden körs hello cloud service configuration via RDP, se [aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>

## <a name="user-account-access-toofiles-and-directories"></a>Användarens konto åtkomst toofiles och kataloger

Både ett automatiskt användarkonto och ett namngivet användarkonto har arbetskatalog för läsning och skrivning åtkomst toohello aktivitetens, delad katalog och flera instanser uppgifter directory. Båda typer av konton har läsbehörighet toohello start- och förberedelse av kataloger.

Om en aktivitet körs under hello samma konto som användes för att köra en startuppgift, hello aktivitet har läs-/ skrivåtkomst toohello starta uppgiften katalog. På liknande sätt, om en aktivitet körs under hello samma konto som användes för att köra ett jobb jobbförberedelseuppgift, hello aktivitet har läs-/ skrivåtkomst toohello jobbet förberedelse uppgiften katalog. Om en aktivitet körs under ett annat konto än hello startuppgift eller förberedelse projektaktivitet har hello uppgiften bara läsbehörighet toohello respektive directory.

Mer information om hur du använder filer och kataloger från en aktivitet finns [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Utökad åtkomst för aktiviteter 

hello användarkontots höjning nivå anger om en aktivitet körs med förhöjd åtkomst. Både ett automatiskt användarkonto och ett konto för namngiven användare kan köra med högre behörighet. hello två alternativ för höjning nivå är:

- **NonAdmin:** hello aktiviteten körs som en vanlig användare utan utökad åtkomst. hello höjning standardnivå för ett användarkonto för Batch är alltid **NonAdmin**.
- **Admin:** hello aktiviteten körs som en användare med högre behörighet och fungerar med fullständig administratörsbehörighet. 

## <a name="auto-user-accounts"></a>Auto-användarkonton

Som standard köra uppgifter i Batch under en auto-användarkonto som en vanlig användare utan utökad åtkomst och med aktiviteten omfattning. När hello automatiskt användaren specifikationen konfigureras för omfånget för aktiviteten skapar hello Batch-tjänsten en automatisk användarkontot för aktiviteten endast.

hello alternativa tootask scope är poolen omfång. När hello automatiskt användaren specifikationen för en aktivitet har konfigurerats för poolen scope körs hello aktiviteten under en auto-användarkonto som är tillgängliga tooany uppgift i hello pool. Mer information om poolen scope finns i avsnittet för hello [köra en aktivitet som hello automatiskt användare med poolen scope](#run-a-task-as-the-autouser-with-pool-scope).   

hello standardscope skiljer sig på Windows- och Linux-noder:

- På Windows-noder körs aktiviteter under scope på uppgift som standard.
- Linux-noder kan alltid köras under poolen omfång.

Det finns fyra konfigurationer för hello automatiskt användaren specifikationen som motsvarar tooa unikt auto-användarkonto:

- Icke-administratörsåtkomst med uppgiften omfattning (hello standard automatiskt användaren specifikation)
- Administratörsåtkomst (förhöjd) med uppgiften scope
- Icke-administratörsåtkomst med poolen scope
- Administratörsåtkomst med poolen scope

> [!IMPORTANT] 
> Aktiviteter som körs under aktiviteten scope har inte faktisk tooother aktiviteter på en nod. Men kan en obehörig användare med åtkomst toohello konto undvika den här begränsningen genom att skicka en aktivitet som körs med administratörsbehörighet och har åtkomst till andra kataloger som aktiviteten. En obehörig användare kan också använda RDP eller SSH tooconnect tooa nod. Det är viktigt tooprotect åtkomst tooyour Batch-kontot nycklar tooprevent sådant scenario. Om du misstänker att ditt konto komprometteras vara säker på att tooregenerate dina nycklar.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Köra en aktivitet som automatiskt-användare med högre behörighet

Du kan konfigurera hello automatiskt användaren specifikationen för administratörsbehörighet när du behöver toorun en aktivitet med högre behörighet. Till exempel behöva Startuppgiften utökade tooinstall programvara på hello-nod.

> [!NOTE] 
> I allmänhet är det bäst toouse utökad åtkomst bara när det behövs. Bästa praxis rekommenderar bevilja hello minsta privilegium nödvändiga tooachieve hello önskat utfall. Till exempel om en startuppgift installerar programvara för hello aktuell användare, i stället för för alla användare kanske kan tooavoid ge högre behörighet tootasks. Du kan konfigurera hello automatiskt användaren specifikation för poolen scope och icke-administratörer för alla aktiviteter som behöver toorun under hello samma konto, inklusive hello startuppgift. 
>
>

hello följande kodavsnitt visar hur tooconfigure hello automatiskt användare-specifikationen. hello exempel set hello höjning nivå för`Admin` och hello omfånget för`Task`. Uppgiften scope är hello standardinställningen, men finns här för hello saké exemplet.

#### <a name="batch-net"></a>.NET för Batch

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a>Batch Java

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a>Python för Batch

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Köra en aktivitet som automatiskt-användare med poolen scope

När en nod har etablerats skapas två poolen hela auto-användarkonton på varje nod i hello-pool med högre behörighet och utan utökad åtkomst. Ange hello auto-användarens omfång toopool scope för en viss aktivitet körs hello under någon av dessa två poolen hela auto-användarkonton. 

När du anger poolen omfattning för hello auto-användare och alla aktiviteter som körs med administratörsåtkomst köras under hello samma pool hela auto-användarkonto. Aktiviteter som körs utan administratörsbehörighet körs på samma sätt även under samma pool hela auto-konto. 

> [!NOTE] 
> hello två poolen hela auto-användarkonton är separata konton. Aktiviteter som körs under hello poolen hela administrativt konto kan inte dela data med aktiviteter som körs under hello standardkonto och vice versa. 
>
>

Hej nytta toorunning under samma auto-användarkonto är att aktiviteter kan tooshare data med andra uppgifter som körs på hello hello samma nod.

Dela hemligheter mellan aktiviteter är ett scenario där pågående aktiviteter under någon av hello två poolen hela auto-användarkonton är användbart. Anta exempelvis att en start-aktivitet måste tooprovision en hemlighet till hello-noden som kan använda för andra aktiviteter. Du kan använda hello Windows Data Protection API (DPAPI), men det krävs administratörsbehörighet. I stället kan du skydda hello hemligheten på användarnivå hello. Aktiviteter som körs under samma användarkonto kan komma åt hello hello hemlighet utan utökad åtkomst.

Dela ett annat scenario där du kanske vill toorun uppgifter under en auto-användarkonto med poolen scope är en Message Passing Interface (MPI)-fil. En filresurs för MPI är användbart när hello noder i hello MPI aktiviteten behöver toowork på hello samma fildata. hello huvudnod skapar en filresurs som hello underordnade noder har åtkomst till om de körs under hello samma auto-användarkonto. 

följande kodstycke hello anger hello auto-användarens omfång toopool omfattningen för en aktivitet i Batch .NET. hello höjning nivå utelämnas så hello aktiviteten körs under hello poolen hela auto-konto.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Namngiven användare

Du kan definiera namngivna användarkonton när du skapar en pool. En namngiven användarkonto har ett namn och lösenord som du anger. Du kan ange hello höjning nivå för en namngiven användarkontot. Du kan också tillhandahålla en privata SSH-nyckel för Linux-noder.

En namngiven användarkontot finns på alla noder i hello pool och är tillgängliga tooall aktiviteter som körs på dessa noder. Du kan definiera valfritt antal namngivna användare för en pool. Du kan ange hello uppgiften körs under en hello med namnet användarkonton som definierats i hello poolen när du lägger till en aktivitet eller en uppgift samling.

Ett namngivet användarkonto är användbart när du vill toorun alla aktiviteter i ett jobb under hello samma användarkonto, men isolera dem från aktiviteter som körs i andra jobb på hello samtidigt. Du kan till exempel skapa en namngiven användare för varje jobb och köra varje jobb uppgifter under det namngivna användarkontot. Varje jobb kan sedan delar en hemlighet med egna aktiviteter, men inte med aktiviteter som körs i andra jobb.

Du kan också använda en namngiven användare konto toorun en aktivitet som anger behörigheter på externa resurser, till exempel filresurser. Med en namngiven användarkonto styra hello användarens identitet och kan använda de identitet tooset behörigheterna.  

Namngiven användare aktivera lösenord mindre SSH mellan Linux-noder. Du kan använda en namngiven användarkonto med Linux-noder som behöver toorun uppgifter för flera instanser. Varje nod i hello pool kan köra uppgifter under ett användarkonto som har definierats i hello hela poolen. Mer information om flera instanser uppgifter finns [använder flera\-instans uppgifter toorun MPI program](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Skapa namngiven användarkonton

toocreate med namnet användarkonton i batchen, lägga till en samling användare konton toohello pool. hello visar följande kodavsnitt hur toocreate med namnet användarkonton i .NET, Java och Python. Dessa code kodavsnitt visar hur toocreate både admin och icke-administratörer med namnet konton i en pool. hello exempel skapa pooler med konfiguration av hello cloud service, men du använder hello samma närmar sig när du skapar en Windows- eller Linux-pool med hello konfiguration av virtuell dator.

#### <a name="batch-net-example-windows"></a>Batch .NET-exempel (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using hello cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                                         
    virtualMachineSize: "small",                                                
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit hello pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a>Batch .NET-exempel (Linux)

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello virtual machine configuration toouse toocreate hello pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create hello unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit hello pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a>Batch Java-exempel

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a>Batch Python-exempel

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.nonadmin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Köra en aktivitet för en namngiven användarkonto med högre behörighet

toorun en aktivitet som en förhöjd användarens, ange hello uppgiften **UserIdentity** tooa för egenskapen med namnet konto som har skapats med dess **ElevationLevel** egenskapsuppsättning för`Admin`.

Det här kodstycket anger hello aktiviteten ska köras under ett konto för namngiven användare. Namngivna användarkontot har definierats för hello pool när hello poolen har skapats. I det här fallet har hello med namnet konto skapats med administratörsbehörigheter:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-toohello-latest-batch-client-library"></a>Uppdatera din kod toohello senaste Batch-klientbibliotek

hello Batch-tjänstversion 2017-01-01.4.0 introducerar en ny ändring som ersätter hello **runElevated** egenskap som är tillgänglig i tidigare versioner med hello **userIdentity** egenskapen. följande tabeller hello ger en enkel mappning som du kan använda tooupdate koden från tidigare versioner av hello klientbibliotek.

### <a name="batch-net"></a>.NET för Batch

| Om din kod använder...                  | Uppdatera det till...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated`inte angetts | Ingen uppdatering krävs                                                                                               |

### <a name="batch-java"></a>Batch Java

| Om din kod använder...                      | Uppdatera det till...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated`inte angetts | Ingen uppdatering krävs                                                                                                                     |

### <a name="batch-python"></a>Python för Batch

| Om din kod använder...                      | Uppdatera det till...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, där <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | `user_identity=user`, där <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| `run_elevated`inte angetts | Ingen uppdatering krävs                                                                                                                                  |


## <a name="next-steps"></a>Nästa steg

### <a name="batch-forum"></a>Batch-Forum

Hej [Azure Batch-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN. HEAD på över för användbara Fäst inlägg och publicera dina frågor när de uppstår när du skapar Batch-lösningar.
