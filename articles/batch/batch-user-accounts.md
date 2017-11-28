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
# <a name="run-tasks-under-user-accounts-in-batch"></a><span data-ttu-id="146b2-103">Köra uppgifter under användarkonton i Batch</span><span class="sxs-lookup"><span data-stu-id="146b2-103">Run tasks under user accounts in Batch</span></span>

<span data-ttu-id="146b2-104">En aktivitet i Azure Batch körs alltid under ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="146b2-104">A task in Azure Batch always runs under a user account.</span></span> <span data-ttu-id="146b2-105">Som standard körs uppgifter under standardanvändarkonton utan administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="146b2-105">By default, tasks run under standard user accounts, without administrator permissions.</span></span> <span data-ttu-id="146b2-106">Dessa standardinställningarna räcker oftast.</span><span class="sxs-lookup"><span data-stu-id="146b2-106">These default user account settings are typically sufficient.</span></span> <span data-ttu-id="146b2-107">För vissa scenarier men är det användbart toobe kan tooconfigure hello användarkonto under vilket du vill att en aktivitet toorun.</span><span class="sxs-lookup"><span data-stu-id="146b2-107">For certain scenarios, however, it's useful toobe able tooconfigure hello user account under which you want a task toorun.</span></span> <span data-ttu-id="146b2-108">Den här artikeln beskrivs hello typerna av användarkonton och hur du kan konfigurera dem för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="146b2-108">This article discusses hello types of user accounts and how you can configure them for your scenario.</span></span>

## <a name="types-of-user-accounts"></a><span data-ttu-id="146b2-109">Typer av användarkonton</span><span class="sxs-lookup"><span data-stu-id="146b2-109">Types of user accounts</span></span>

<span data-ttu-id="146b2-110">Azure Batch tillhandahåller två typer av användarkonton för pågående aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="146b2-110">Azure Batch provides two types of user accounts for running tasks:</span></span>

- <span data-ttu-id="146b2-111">**Automatisk användarkonton.**</span><span class="sxs-lookup"><span data-stu-id="146b2-111">**Auto-user accounts.**</span></span> <span data-ttu-id="146b2-112">Auto-användarkonton är inbyggda användarkonton som skapas automatiskt av hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="146b2-112">Auto-user accounts are built-in user accounts that are created automatically by hello Batch service.</span></span> <span data-ttu-id="146b2-113">Som standard körs aktiviteter under en automatisk användarkonto.</span><span class="sxs-lookup"><span data-stu-id="146b2-113">By default, tasks run under an auto-user account.</span></span> <span data-ttu-id="146b2-114">Du kan konfigurera hello automatiskt användaren specifikationen för en aktivitet tooindicate konto en aktivitet ska köras under vilken auto-användare.</span><span class="sxs-lookup"><span data-stu-id="146b2-114">You can configure hello auto-user specification for a task tooindicate under which auto-user account a task should run.</span></span> <span data-ttu-id="146b2-115">hello automatiskt användaren specifikation kan du toospecify hello höjning nivå och omfattningen av hello automatiskt-användarkonto som ska köra hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="146b2-115">hello auto-user specification allows you toospecify hello elevation level and scope of hello auto-user account that will run hello task.</span></span> 

- <span data-ttu-id="146b2-116">**En namngiven användarkonto.**</span><span class="sxs-lookup"><span data-stu-id="146b2-116">**A named user account.**</span></span> <span data-ttu-id="146b2-117">Du kan ange en eller flera namngivna användarkonton för en pool när du skapar hello-adresspool.</span><span class="sxs-lookup"><span data-stu-id="146b2-117">You can specify one or more named user accounts for a pool when you create hello pool.</span></span> <span data-ttu-id="146b2-118">Varje användarkonto skapas på varje nod i hello poolen.</span><span class="sxs-lookup"><span data-stu-id="146b2-118">Each user account is created on each node of hello pool.</span></span> <span data-ttu-id="146b2-119">Dessutom toohello kontonamn kan du ange hello lösenord, höjning nivå och, för Linux pooler hello SSH privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="146b2-119">In addition toohello account name, you specify hello user account password, elevation level, and, for Linux pools, hello SSH private key.</span></span> <span data-ttu-id="146b2-120">När du lägger till en aktivitet kan du ange hello med namnet konto som aktiviteten ska köras under.</span><span class="sxs-lookup"><span data-stu-id="146b2-120">When you add a task, you can specify hello named user account under which that task should run.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="146b2-121">hello Batch-tjänstversion 2017-01-01.4.0 introducerar en ny ändring som kräver att du uppdaterar din kod toocall den här versionen.</span><span class="sxs-lookup"><span data-stu-id="146b2-121">hello Batch service version 2017-01-01.4.0 introduces a breaking change that requires that you update your code toocall that version.</span></span> <span data-ttu-id="146b2-122">Om du migrerar kod från en äldre version av Batch, Observera att hello **runElevated** egenskapen stöds inte längre i hello REST API eller Batch klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="146b2-122">If you are migrating code from an older version of Batch, note that hello **runElevated** property is no longer supported in hello REST API or Batch client libraries.</span></span> <span data-ttu-id="146b2-123">Använd hello nya **userIdentity** -egenskapen för en aktivitet toospecify höjning nivå.</span><span class="sxs-lookup"><span data-stu-id="146b2-123">Use hello new **userIdentity** property of a task toospecify elevation level.</span></span> <span data-ttu-id="146b2-124">Finns avsnittet hello [uppdatera din kod toohello senaste Batch klientbiblioteket](#update-your-code-to-the-latest-batch-client-library) snabb riktlinjer för uppdatering av Batch-kod om du använder en av hello klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="146b2-124">See hello section titled [Update your code toohello latest Batch client library](#update-your-code-to-the-latest-batch-client-library) for quick guidelines for updating your Batch code if you are using one of hello client libraries.</span></span>
>
>

> [!NOTE] 
> <span data-ttu-id="146b2-125">hello användarkonton i den här artikeln stöder inte Remote Desktop Protocol (RDP) eller SSH (Secure Shell), av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="146b2-125">hello user accounts discussed in this article do not support Remote Desktop Protocol (RDP) or Secure Shell (SSH), for security reasons.</span></span> 
>
> <span data-ttu-id="146b2-126">tooconnect tooa körs hello Linux virtuella nodkonfiguration via SSH, se [Använd Fjärrskrivbord tooa Linux VM i Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="146b2-126">tooconnect tooa node running hello Linux virtual machine configuration via SSH, see [Use Remote Desktop tooa Linux VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span></span> <span data-ttu-id="146b2-127">tooconnect toonodes som kör Windows via RDP, se [ansluta tooa Windows Server VM](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="146b2-127">tooconnect toonodes running Windows via RDP, see [Connect tooa Windows Server VM](../virtual-machines/windows/connect-logon.md).</span></span><br /><br />
> <span data-ttu-id="146b2-128">tooconnect tooa noden körs hello cloud service configuration via RDP, se [aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span><span class="sxs-lookup"><span data-stu-id="146b2-128">tooconnect tooa node running hello cloud service configuration via RDP, see [Enable Remote Desktop Connection for a Role in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span></span>
>
>

## <a name="user-account-access-toofiles-and-directories"></a><span data-ttu-id="146b2-129">Användarens konto åtkomst toofiles och kataloger</span><span class="sxs-lookup"><span data-stu-id="146b2-129">User account access toofiles and directories</span></span>

<span data-ttu-id="146b2-130">Både ett automatiskt användarkonto och ett namngivet användarkonto har arbetskatalog för läsning och skrivning åtkomst toohello aktivitetens, delad katalog och flera instanser uppgifter directory.</span><span class="sxs-lookup"><span data-stu-id="146b2-130">Both an auto-user account and a named user account have read/write access toohello task’s working directory, shared directory, and multi-instance tasks directory.</span></span> <span data-ttu-id="146b2-131">Båda typer av konton har läsbehörighet toohello start- och förberedelse av kataloger.</span><span class="sxs-lookup"><span data-stu-id="146b2-131">Both types of accounts have read access toohello startup and job preparation directories.</span></span>

<span data-ttu-id="146b2-132">Om en aktivitet körs under hello samma konto som användes för att köra en startuppgift, hello aktivitet har läs-/ skrivåtkomst toohello starta uppgiften katalog.</span><span class="sxs-lookup"><span data-stu-id="146b2-132">If a task runs under hello same account that was used for running a start task, hello task has read-write access toohello start task directory.</span></span> <span data-ttu-id="146b2-133">På liknande sätt, om en aktivitet körs under hello samma konto som användes för att köra ett jobb jobbförberedelseuppgift, hello aktivitet har läs-/ skrivåtkomst toohello jobbet förberedelse uppgiften katalog.</span><span class="sxs-lookup"><span data-stu-id="146b2-133">Similarly, if a task runs under hello same account that was used for running a job preparation task, hello task has read-write access toohello job preparation task directory.</span></span> <span data-ttu-id="146b2-134">Om en aktivitet körs under ett annat konto än hello startuppgift eller förberedelse projektaktivitet har hello uppgiften bara läsbehörighet toohello respektive directory.</span><span class="sxs-lookup"><span data-stu-id="146b2-134">If a task runs under a different account than hello start task or job preparation task, then hello task has only read access toohello respective directory.</span></span>

<span data-ttu-id="146b2-135">Mer information om hur du använder filer och kataloger från en aktivitet finns [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md#files-and-directories).</span><span class="sxs-lookup"><span data-stu-id="146b2-135">For more information on accessing files and directories from a task, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#files-and-directories).</span></span>

## <a name="elevated-access-for-tasks"></a><span data-ttu-id="146b2-136">Utökad åtkomst för aktiviteter</span><span class="sxs-lookup"><span data-stu-id="146b2-136">Elevated access for tasks</span></span> 

<span data-ttu-id="146b2-137">hello användarkontots höjning nivå anger om en aktivitet körs med förhöjd åtkomst.</span><span class="sxs-lookup"><span data-stu-id="146b2-137">hello user account's elevation level indicates whether a task runs with elevated access.</span></span> <span data-ttu-id="146b2-138">Både ett automatiskt användarkonto och ett konto för namngiven användare kan köra med högre behörighet.</span><span class="sxs-lookup"><span data-stu-id="146b2-138">Both an auto-user account and a named user account can run with elevated access.</span></span> <span data-ttu-id="146b2-139">hello två alternativ för höjning nivå är:</span><span class="sxs-lookup"><span data-stu-id="146b2-139">hello two options for elevation level are:</span></span>

- <span data-ttu-id="146b2-140">**NonAdmin:** hello aktiviteten körs som en vanlig användare utan utökad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="146b2-140">**NonAdmin:** hello task runs as a standard user without elevated access.</span></span> <span data-ttu-id="146b2-141">hello höjning standardnivå för ett användarkonto för Batch är alltid **NonAdmin**.</span><span class="sxs-lookup"><span data-stu-id="146b2-141">hello default elevation level for a Batch user account is always **NonAdmin**.</span></span>
- <span data-ttu-id="146b2-142">**Admin:** hello aktiviteten körs som en användare med högre behörighet och fungerar med fullständig administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="146b2-142">**Admin:** hello task runs as a user with elevated access and operates with full Administrator permissions.</span></span> 

## <a name="auto-user-accounts"></a><span data-ttu-id="146b2-143">Auto-användarkonton</span><span class="sxs-lookup"><span data-stu-id="146b2-143">Auto-user accounts</span></span>

<span data-ttu-id="146b2-144">Som standard köra uppgifter i Batch under en auto-användarkonto som en vanlig användare utan utökad åtkomst och med aktiviteten omfattning.</span><span class="sxs-lookup"><span data-stu-id="146b2-144">By default, tasks run in Batch under an auto-user account, as a standard user without elevated access, and with task scope.</span></span> <span data-ttu-id="146b2-145">När hello automatiskt användaren specifikationen konfigureras för omfånget för aktiviteten skapar hello Batch-tjänsten en automatisk användarkontot för aktiviteten endast.</span><span class="sxs-lookup"><span data-stu-id="146b2-145">When hello auto-user specification is configured for task scope, hello Batch service creates an auto-user account for that task only.</span></span>

<span data-ttu-id="146b2-146">hello alternativa tootask scope är poolen omfång.</span><span class="sxs-lookup"><span data-stu-id="146b2-146">hello alternative tootask scope is pool scope.</span></span> <span data-ttu-id="146b2-147">När hello automatiskt användaren specifikationen för en aktivitet har konfigurerats för poolen scope körs hello aktiviteten under en auto-användarkonto som är tillgängliga tooany uppgift i hello pool.</span><span class="sxs-lookup"><span data-stu-id="146b2-147">When hello auto-user specification for a task is configured for pool scope, hello task runs under an auto-user account that is available tooany task in hello pool.</span></span> <span data-ttu-id="146b2-148">Mer information om poolen scope finns i avsnittet för hello [köra en aktivitet som hello automatiskt användare med poolen scope](#run-a-task-as-the-autouser-with-pool-scope).</span><span class="sxs-lookup"><span data-stu-id="146b2-148">For more information about pool scope, see hello section titled [Run a task as hello auto-user with pool scope](#run-a-task-as-the-autouser-with-pool-scope).</span></span>   

<span data-ttu-id="146b2-149">hello standardscope skiljer sig på Windows- och Linux-noder:</span><span class="sxs-lookup"><span data-stu-id="146b2-149">hello default scope is different on Windows and Linux nodes:</span></span>

- <span data-ttu-id="146b2-150">På Windows-noder körs aktiviteter under scope på uppgift som standard.</span><span class="sxs-lookup"><span data-stu-id="146b2-150">On Windows nodes, tasks run under task scope by default.</span></span>
- <span data-ttu-id="146b2-151">Linux-noder kan alltid köras under poolen omfång.</span><span class="sxs-lookup"><span data-stu-id="146b2-151">Linux nodes always run under pool scope.</span></span>

<span data-ttu-id="146b2-152">Det finns fyra konfigurationer för hello automatiskt användaren specifikationen som motsvarar tooa unikt auto-användarkonto:</span><span class="sxs-lookup"><span data-stu-id="146b2-152">There are four possible configurations for hello auto-user specification, each of which corresponds tooa unique auto-user account:</span></span>

- <span data-ttu-id="146b2-153">Icke-administratörsåtkomst med uppgiften omfattning (hello standard automatiskt användaren specifikation)</span><span class="sxs-lookup"><span data-stu-id="146b2-153">Non-admin access with task scope (hello default auto-user specification)</span></span>
- <span data-ttu-id="146b2-154">Administratörsåtkomst (förhöjd) med uppgiften scope</span><span class="sxs-lookup"><span data-stu-id="146b2-154">Admin (elevated) access with task scope</span></span>
- <span data-ttu-id="146b2-155">Icke-administratörsåtkomst med poolen scope</span><span class="sxs-lookup"><span data-stu-id="146b2-155">Non-admin access with pool scope</span></span>
- <span data-ttu-id="146b2-156">Administratörsåtkomst med poolen scope</span><span class="sxs-lookup"><span data-stu-id="146b2-156">Admin access with pool scope</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="146b2-157">Aktiviteter som körs under aktiviteten scope har inte faktisk tooother aktiviteter på en nod.</span><span class="sxs-lookup"><span data-stu-id="146b2-157">Tasks running under task scope do not have de facto access tooother tasks on a node.</span></span> <span data-ttu-id="146b2-158">Men kan en obehörig användare med åtkomst toohello konto undvika den här begränsningen genom att skicka en aktivitet som körs med administratörsbehörighet och har åtkomst till andra kataloger som aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="146b2-158">However, a malicious user with access toohello account could work around this restriction by submitting a task that runs with administrator privileges and accesses other task directories.</span></span> <span data-ttu-id="146b2-159">En obehörig användare kan också använda RDP eller SSH tooconnect tooa nod.</span><span class="sxs-lookup"><span data-stu-id="146b2-159">A malicious user could also use RDP or SSH tooconnect tooa node.</span></span> <span data-ttu-id="146b2-160">Det är viktigt tooprotect åtkomst tooyour Batch-kontot nycklar tooprevent sådant scenario.</span><span class="sxs-lookup"><span data-stu-id="146b2-160">It's important tooprotect access tooyour Batch account keys tooprevent such a scenario.</span></span> <span data-ttu-id="146b2-161">Om du misstänker att ditt konto komprometteras vara säker på att tooregenerate dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="146b2-161">If you suspect your account may have been compromised, be sure tooregenerate your keys.</span></span>
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a><span data-ttu-id="146b2-162">Köra en aktivitet som automatiskt-användare med högre behörighet</span><span class="sxs-lookup"><span data-stu-id="146b2-162">Run a task as an auto-user with elevated access</span></span>

<span data-ttu-id="146b2-163">Du kan konfigurera hello automatiskt användaren specifikationen för administratörsbehörighet när du behöver toorun en aktivitet med högre behörighet.</span><span class="sxs-lookup"><span data-stu-id="146b2-163">You can configure hello auto-user specification for administrator privileges when you need toorun a task with elevated access.</span></span> <span data-ttu-id="146b2-164">Till exempel behöva Startuppgiften utökade tooinstall programvara på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="146b2-164">For example, a start task may need elevated access tooinstall software on hello node.</span></span>

> [!NOTE] 
> <span data-ttu-id="146b2-165">I allmänhet är det bäst toouse utökad åtkomst bara när det behövs.</span><span class="sxs-lookup"><span data-stu-id="146b2-165">In general, it's best toouse elevated access only when necessary.</span></span> <span data-ttu-id="146b2-166">Bästa praxis rekommenderar bevilja hello minsta privilegium nödvändiga tooachieve hello önskat utfall.</span><span class="sxs-lookup"><span data-stu-id="146b2-166">Best practices recommend granting hello minimum privilege necessary tooachieve hello desired outcome.</span></span> <span data-ttu-id="146b2-167">Till exempel om en startuppgift installerar programvara för hello aktuell användare, i stället för för alla användare kanske kan tooavoid ge högre behörighet tootasks.</span><span class="sxs-lookup"><span data-stu-id="146b2-167">For example, if a start task installs software for hello current user, instead of for all users, you may be able tooavoid granting elevated access tootasks.</span></span> <span data-ttu-id="146b2-168">Du kan konfigurera hello automatiskt användaren specifikation för poolen scope och icke-administratörer för alla aktiviteter som behöver toorun under hello samma konto, inklusive hello startuppgift.</span><span class="sxs-lookup"><span data-stu-id="146b2-168">You can configure hello auto-user specification for pool scope and non-admin access for all tasks that need toorun under hello same account, including hello start task.</span></span> 
>
>

<span data-ttu-id="146b2-169">hello följande kodavsnitt visar hur tooconfigure hello automatiskt användare-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="146b2-169">hello following code snippets show how tooconfigure hello auto-user specification.</span></span> <span data-ttu-id="146b2-170">hello exempel set hello höjning nivå för`Admin` och hello omfånget för`Task`.</span><span class="sxs-lookup"><span data-stu-id="146b2-170">hello examples set hello elevation level too`Admin` and hello scope too`Task`.</span></span> <span data-ttu-id="146b2-171">Uppgiften scope är hello standardinställningen, men finns här för hello saké exemplet.</span><span class="sxs-lookup"><span data-stu-id="146b2-171">Task scope is hello default setting, but is included here for hello sake of example.</span></span>

#### <a name="batch-net"></a><span data-ttu-id="146b2-172">.NET för Batch</span><span class="sxs-lookup"><span data-stu-id="146b2-172">Batch .NET</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a><span data-ttu-id="146b2-173">Batch Java</span><span class="sxs-lookup"><span data-stu-id="146b2-173">Batch Java</span></span>

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a><span data-ttu-id="146b2-174">Python för Batch</span><span class="sxs-lookup"><span data-stu-id="146b2-174">Batch Python</span></span>

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

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a><span data-ttu-id="146b2-175">Köra en aktivitet som automatiskt-användare med poolen scope</span><span class="sxs-lookup"><span data-stu-id="146b2-175">Run a task as an auto-user with pool scope</span></span>

<span data-ttu-id="146b2-176">När en nod har etablerats skapas två poolen hela auto-användarkonton på varje nod i hello-pool med högre behörighet och utan utökad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="146b2-176">When a node is provisioned, two pool-wide auto-user accounts are created on each node in hello pool, one with elevated access, and one without elevated access.</span></span> <span data-ttu-id="146b2-177">Ange hello auto-användarens omfång toopool scope för en viss aktivitet körs hello under någon av dessa två poolen hela auto-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="146b2-177">Setting hello auto-user's scope toopool scope for a given task runs hello task under one of these two pool-wide auto-user accounts.</span></span> 

<span data-ttu-id="146b2-178">När du anger poolen omfattning för hello auto-användare och alla aktiviteter som körs med administratörsåtkomst köras under hello samma pool hela auto-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="146b2-178">When you specify pool scope for hello auto-user, all tasks that run with administrator access run under hello same pool-wide auto-user account.</span></span> <span data-ttu-id="146b2-179">Aktiviteter som körs utan administratörsbehörighet körs på samma sätt även under samma pool hela auto-konto.</span><span class="sxs-lookup"><span data-stu-id="146b2-179">Similarly, tasks that run without administrator permissions also run under a single pool-wide auto-user account.</span></span> 

> [!NOTE] 
> <span data-ttu-id="146b2-180">hello två poolen hela auto-användarkonton är separata konton.</span><span class="sxs-lookup"><span data-stu-id="146b2-180">hello two pool-wide auto-user accounts are separate accounts.</span></span> <span data-ttu-id="146b2-181">Aktiviteter som körs under hello poolen hela administrativt konto kan inte dela data med aktiviteter som körs under hello standardkonto och vice versa.</span><span class="sxs-lookup"><span data-stu-id="146b2-181">Tasks running under hello pool-wide administrative account cannot share data with tasks running under hello standard account, and vice versa.</span></span> 
>
>

<span data-ttu-id="146b2-182">Hej nytta toorunning under samma auto-användarkonto är att aktiviteter kan tooshare data med andra uppgifter som körs på hello hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="146b2-182">hello advantage toorunning under hello same auto-user account is that tasks are able tooshare data with other tasks running on hello same node.</span></span>

<span data-ttu-id="146b2-183">Dela hemligheter mellan aktiviteter är ett scenario där pågående aktiviteter under någon av hello två poolen hela auto-användarkonton är användbart.</span><span class="sxs-lookup"><span data-stu-id="146b2-183">Sharing secrets between tasks is one scenario where running tasks under one of hello two pool-wide auto-user accounts is useful.</span></span> <span data-ttu-id="146b2-184">Anta exempelvis att en start-aktivitet måste tooprovision en hemlighet till hello-noden som kan använda för andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="146b2-184">For example, suppose a start task needs tooprovision a secret onto hello node that other tasks can use.</span></span> <span data-ttu-id="146b2-185">Du kan använda hello Windows Data Protection API (DPAPI), men det krävs administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="146b2-185">You could use hello Windows Data Protection API (DPAPI), but it requires administrator privileges.</span></span> <span data-ttu-id="146b2-186">I stället kan du skydda hello hemligheten på användarnivå hello.</span><span class="sxs-lookup"><span data-stu-id="146b2-186">Instead, you can protect hello secret at hello user level.</span></span> <span data-ttu-id="146b2-187">Aktiviteter som körs under samma användarkonto kan komma åt hello hello hemlighet utan utökad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="146b2-187">Tasks running under hello same user account can access hello secret without elevated access.</span></span>

<span data-ttu-id="146b2-188">Dela ett annat scenario där du kanske vill toorun uppgifter under en auto-användarkonto med poolen scope är en Message Passing Interface (MPI)-fil.</span><span class="sxs-lookup"><span data-stu-id="146b2-188">Another scenario where you may want toorun tasks under an auto-user account with pool scope is a Message Passing Interface (MPI) file share.</span></span> <span data-ttu-id="146b2-189">En filresurs för MPI är användbart när hello noder i hello MPI aktiviteten behöver toowork på hello samma fildata.</span><span class="sxs-lookup"><span data-stu-id="146b2-189">An MPI file share is useful when hello nodes in hello MPI task need toowork on hello same file data.</span></span> <span data-ttu-id="146b2-190">hello huvudnod skapar en filresurs som hello underordnade noder har åtkomst till om de körs under hello samma auto-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="146b2-190">hello head node creates a file share that hello child nodes can access if they are running under hello same auto-user account.</span></span> 

<span data-ttu-id="146b2-191">följande kodstycke hello anger hello auto-användarens omfång toopool omfattningen för en aktivitet i Batch .NET.</span><span class="sxs-lookup"><span data-stu-id="146b2-191">hello following code snippet sets hello auto-user's scope toopool scope for a task in Batch .NET.</span></span> <span data-ttu-id="146b2-192">hello höjning nivå utelämnas så hello aktiviteten körs under hello poolen hela auto-konto.</span><span class="sxs-lookup"><span data-stu-id="146b2-192">hello elevation level is omitted, so hello task runs under hello standard pool-wide auto-user account.</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a><span data-ttu-id="146b2-193">Namngiven användare</span><span class="sxs-lookup"><span data-stu-id="146b2-193">Named user accounts</span></span>

<span data-ttu-id="146b2-194">Du kan definiera namngivna användarkonton när du skapar en pool.</span><span class="sxs-lookup"><span data-stu-id="146b2-194">You can define named user accounts when you create a pool.</span></span> <span data-ttu-id="146b2-195">En namngiven användarkonto har ett namn och lösenord som du anger.</span><span class="sxs-lookup"><span data-stu-id="146b2-195">A named user account has a name and password that you provide.</span></span> <span data-ttu-id="146b2-196">Du kan ange hello höjning nivå för en namngiven användarkontot.</span><span class="sxs-lookup"><span data-stu-id="146b2-196">You can specify hello elevation level for a named user account.</span></span> <span data-ttu-id="146b2-197">Du kan också tillhandahålla en privata SSH-nyckel för Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="146b2-197">For Linux nodes, you can also provide an SSH private key.</span></span>

<span data-ttu-id="146b2-198">En namngiven användarkontot finns på alla noder i hello pool och är tillgängliga tooall aktiviteter som körs på dessa noder.</span><span class="sxs-lookup"><span data-stu-id="146b2-198">A named user account exists on all nodes in hello pool and is available tooall tasks running on those nodes.</span></span> <span data-ttu-id="146b2-199">Du kan definiera valfritt antal namngivna användare för en pool.</span><span class="sxs-lookup"><span data-stu-id="146b2-199">You may define any number of named users for a pool.</span></span> <span data-ttu-id="146b2-200">Du kan ange hello uppgiften körs under en hello med namnet användarkonton som definierats i hello poolen när du lägger till en aktivitet eller en uppgift samling.</span><span class="sxs-lookup"><span data-stu-id="146b2-200">When you add a task or task collection, you can specify that hello task runs under one of hello named user accounts defined on hello pool.</span></span>

<span data-ttu-id="146b2-201">Ett namngivet användarkonto är användbart när du vill toorun alla aktiviteter i ett jobb under hello samma användarkonto, men isolera dem från aktiviteter som körs i andra jobb på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="146b2-201">A named user account is useful when you want toorun all tasks in a job under hello same user account, but isolate them from tasks running in other jobs at hello same time.</span></span> <span data-ttu-id="146b2-202">Du kan till exempel skapa en namngiven användare för varje jobb och köra varje jobb uppgifter under det namngivna användarkontot.</span><span class="sxs-lookup"><span data-stu-id="146b2-202">For example, you can create a named user for each job, and run each job's tasks under that named user account.</span></span> <span data-ttu-id="146b2-203">Varje jobb kan sedan delar en hemlighet med egna aktiviteter, men inte med aktiviteter som körs i andra jobb.</span><span class="sxs-lookup"><span data-stu-id="146b2-203">Each job can then share a secret with its own tasks, but not with tasks running in other jobs.</span></span>

<span data-ttu-id="146b2-204">Du kan också använda en namngiven användare konto toorun en aktivitet som anger behörigheter på externa resurser, till exempel filresurser.</span><span class="sxs-lookup"><span data-stu-id="146b2-204">You can also use a named user account toorun a task that sets permissions on external resources such as file shares.</span></span> <span data-ttu-id="146b2-205">Med en namngiven användarkonto styra hello användarens identitet och kan använda de identitet tooset behörigheterna.</span><span class="sxs-lookup"><span data-stu-id="146b2-205">With a named user account, you control hello user identity and can use that user identity tooset permissions.</span></span>  

<span data-ttu-id="146b2-206">Namngiven användare aktivera lösenord mindre SSH mellan Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="146b2-206">Named user accounts enable password-less SSH between Linux nodes.</span></span> <span data-ttu-id="146b2-207">Du kan använda en namngiven användarkonto med Linux-noder som behöver toorun uppgifter för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="146b2-207">You can use a named user account with Linux nodes that need toorun multi-instance tasks.</span></span> <span data-ttu-id="146b2-208">Varje nod i hello pool kan köra uppgifter under ett användarkonto som har definierats i hello hela poolen.</span><span class="sxs-lookup"><span data-stu-id="146b2-208">Each node in hello pool can run tasks under a user account defined on hello whole pool.</span></span> <span data-ttu-id="146b2-209">Mer information om flera instanser uppgifter finns [använder flera\-instans uppgifter toorun MPI program](batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="146b2-209">For more information about multi-instance tasks, see [Use multi\-instance tasks toorun MPI applications](batch-mpi.md).</span></span>

### <a name="create-named-user-accounts"></a><span data-ttu-id="146b2-210">Skapa namngiven användarkonton</span><span class="sxs-lookup"><span data-stu-id="146b2-210">Create named user accounts</span></span>

<span data-ttu-id="146b2-211">toocreate med namnet användarkonton i batchen, lägga till en samling användare konton toohello pool.</span><span class="sxs-lookup"><span data-stu-id="146b2-211">toocreate named user accounts in Batch, add a collection of user accounts toohello pool.</span></span> <span data-ttu-id="146b2-212">hello visar följande kodavsnitt hur toocreate med namnet användarkonton i .NET, Java och Python.</span><span class="sxs-lookup"><span data-stu-id="146b2-212">hello following code snippets show how toocreate named user accounts in .NET, Java, and Python.</span></span> <span data-ttu-id="146b2-213">Dessa code kodavsnitt visar hur toocreate både admin och icke-administratörer med namnet konton i en pool.</span><span class="sxs-lookup"><span data-stu-id="146b2-213">These code snippets show how toocreate both admin and non-admin named accounts on a pool.</span></span> <span data-ttu-id="146b2-214">hello exempel skapa pooler med konfiguration av hello cloud service, men du använder hello samma närmar sig när du skapar en Windows- eller Linux-pool med hello konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="146b2-214">hello examples create pools using hello cloud service configuration, but you use hello same approach when creating a Windows or Linux pool using hello virtual machine configuration.</span></span>

#### <a name="batch-net-example-windows"></a><span data-ttu-id="146b2-215">Batch .NET-exempel (Windows)</span><span class="sxs-lookup"><span data-stu-id="146b2-215">Batch .NET example (Windows)</span></span>

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

#### <a name="batch-net-example-linux"></a><span data-ttu-id="146b2-216">Batch .NET-exempel (Linux)</span><span class="sxs-lookup"><span data-stu-id="146b2-216">Batch .NET example (Linux)</span></span>

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


#### <a name="batch-java-example"></a><span data-ttu-id="146b2-217">Batch Java-exempel</span><span class="sxs-lookup"><span data-stu-id="146b2-217">Batch Java example</span></span>

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

#### <a name="batch-python-example"></a><span data-ttu-id="146b2-218">Batch Python-exempel</span><span class="sxs-lookup"><span data-stu-id="146b2-218">Batch Python example</span></span>

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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a><span data-ttu-id="146b2-219">Köra en aktivitet för en namngiven användarkonto med högre behörighet</span><span class="sxs-lookup"><span data-stu-id="146b2-219">Run a task under a named user account with elevated access</span></span>

<span data-ttu-id="146b2-220">toorun en aktivitet som en förhöjd användarens, ange hello uppgiften **UserIdentity** tooa för egenskapen med namnet konto som har skapats med dess **ElevationLevel** egenskapsuppsättning för`Admin`.</span><span class="sxs-lookup"><span data-stu-id="146b2-220">toorun a task as an elevated user, set hello task's **UserIdentity** property tooa named user account that was created with its **ElevationLevel** property set too`Admin`.</span></span>

<span data-ttu-id="146b2-221">Det här kodstycket anger hello aktiviteten ska köras under ett konto för namngiven användare.</span><span class="sxs-lookup"><span data-stu-id="146b2-221">This code snippet specifies that hello task should run under a named user account.</span></span> <span data-ttu-id="146b2-222">Namngivna användarkontot har definierats för hello pool när hello poolen har skapats.</span><span class="sxs-lookup"><span data-stu-id="146b2-222">This named user account was defined on hello pool when hello pool was created.</span></span> <span data-ttu-id="146b2-223">I det här fallet har hello med namnet konto skapats med administratörsbehörigheter:</span><span class="sxs-lookup"><span data-stu-id="146b2-223">In this case, hello named user account was created with admin permissions:</span></span>

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-toohello-latest-batch-client-library"></a><span data-ttu-id="146b2-224">Uppdatera din kod toohello senaste Batch-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="146b2-224">Update your code toohello latest Batch client library</span></span>

<span data-ttu-id="146b2-225">hello Batch-tjänstversion 2017-01-01.4.0 introducerar en ny ändring som ersätter hello **runElevated** egenskap som är tillgänglig i tidigare versioner med hello **userIdentity** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="146b2-225">hello Batch service version 2017-01-01.4.0 introduces a breaking change, replacing hello **runElevated** property available in earlier versions with hello **userIdentity** property.</span></span> <span data-ttu-id="146b2-226">följande tabeller hello ger en enkel mappning som du kan använda tooupdate koden från tidigare versioner av hello klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="146b2-226">hello following tables provide a simple mapping that you can use tooupdate your code from earlier versions of hello client libraries.</span></span>

### <a name="batch-net"></a><span data-ttu-id="146b2-227">.NET för Batch</span><span class="sxs-lookup"><span data-stu-id="146b2-227">Batch .NET</span></span>

| <span data-ttu-id="146b2-228">Om din kod använder...</span><span class="sxs-lookup"><span data-stu-id="146b2-228">If your code uses...</span></span>                  | <span data-ttu-id="146b2-229">Uppdatera det till...</span><span class="sxs-lookup"><span data-stu-id="146b2-229">Update it to....</span></span>                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| <span data-ttu-id="146b2-230">`CloudTask.RunElevated`inte angetts</span><span class="sxs-lookup"><span data-stu-id="146b2-230">`CloudTask.RunElevated` not specified</span></span> | <span data-ttu-id="146b2-231">Ingen uppdatering krävs</span><span class="sxs-lookup"><span data-stu-id="146b2-231">No update required</span></span>                                                                                               |

### <a name="batch-java"></a><span data-ttu-id="146b2-232">Batch Java</span><span class="sxs-lookup"><span data-stu-id="146b2-232">Batch Java</span></span>

| <span data-ttu-id="146b2-233">Om din kod använder...</span><span class="sxs-lookup"><span data-stu-id="146b2-233">If your code uses...</span></span>                      | <span data-ttu-id="146b2-234">Uppdatera det till...</span><span class="sxs-lookup"><span data-stu-id="146b2-234">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| <span data-ttu-id="146b2-235">`CloudTask.withRunElevated`inte angetts</span><span class="sxs-lookup"><span data-stu-id="146b2-235">`CloudTask.withRunElevated` not specified</span></span> | <span data-ttu-id="146b2-236">Ingen uppdatering krävs</span><span class="sxs-lookup"><span data-stu-id="146b2-236">No update required</span></span>                                                                                                                     |

### <a name="batch-python"></a><span data-ttu-id="146b2-237">Python för Batch</span><span class="sxs-lookup"><span data-stu-id="146b2-237">Batch Python</span></span>

| <span data-ttu-id="146b2-238">Om din kod använder...</span><span class="sxs-lookup"><span data-stu-id="146b2-238">If your code uses...</span></span>                      | <span data-ttu-id="146b2-239">Uppdatera det till...</span><span class="sxs-lookup"><span data-stu-id="146b2-239">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | <span data-ttu-id="146b2-240">`user_identity=user`, där</span><span class="sxs-lookup"><span data-stu-id="146b2-240">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | <span data-ttu-id="146b2-241">`user_identity=user`, där</span><span class="sxs-lookup"><span data-stu-id="146b2-241">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| <span data-ttu-id="146b2-242">`run_elevated`inte angetts</span><span class="sxs-lookup"><span data-stu-id="146b2-242">`run_elevated` not specified</span></span> | <span data-ttu-id="146b2-243">Ingen uppdatering krävs</span><span class="sxs-lookup"><span data-stu-id="146b2-243">No update required</span></span>                                                                                                                                  |


## <a name="next-steps"></a><span data-ttu-id="146b2-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="146b2-244">Next steps</span></span>

### <a name="batch-forum"></a><span data-ttu-id="146b2-245">Batch-Forum</span><span class="sxs-lookup"><span data-stu-id="146b2-245">Batch Forum</span></span>

<span data-ttu-id="146b2-246">Hej [Azure Batch-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN.</span><span class="sxs-lookup"><span data-stu-id="146b2-246">hello [Azure Batch Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="146b2-247">HEAD på över för användbara Fäst inlägg och publicera dina frågor när de uppstår när du skapar Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="146b2-247">Head on over for helpful pinned posts, and post your questions as they arise while you build your Batch solutions.</span></span>
