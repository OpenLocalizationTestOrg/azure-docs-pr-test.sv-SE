---
title: "Köra uppgifter under användarkonton i Azure Batch | Microsoft Docs"
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
ms.openlocfilehash: d408c0565c0ed81fc97cc2b3976a4fc233e31302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a><span data-ttu-id="dfcff-103">Köra uppgifter under användarkonton i Batch</span><span class="sxs-lookup"><span data-stu-id="dfcff-103">Run tasks under user accounts in Batch</span></span>

<span data-ttu-id="dfcff-104">En aktivitet i Azure Batch körs alltid under ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-104">A task in Azure Batch always runs under a user account.</span></span> <span data-ttu-id="dfcff-105">Som standard körs uppgifter under standardanvändarkonton utan administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="dfcff-105">By default, tasks run under standard user accounts, without administrator permissions.</span></span> <span data-ttu-id="dfcff-106">Dessa standardinställningarna räcker oftast.</span><span class="sxs-lookup"><span data-stu-id="dfcff-106">These default user account settings are typically sufficient.</span></span> <span data-ttu-id="dfcff-107">För vissa scenarier, men är det användbart för att kunna konfigurera det användarkonto som du vill att aktiviteten ska köras.</span><span class="sxs-lookup"><span data-stu-id="dfcff-107">For certain scenarios, however, it's useful to be able to configure the user account under which you want a task to run.</span></span> <span data-ttu-id="dfcff-108">Den här artikeln beskrivs vilka typer av användarkonton och hur du kan konfigurera dem för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="dfcff-108">This article discusses the types of user accounts and how you can configure them for your scenario.</span></span>

## <a name="types-of-user-accounts"></a><span data-ttu-id="dfcff-109">Typer av användarkonton</span><span class="sxs-lookup"><span data-stu-id="dfcff-109">Types of user accounts</span></span>

<span data-ttu-id="dfcff-110">Azure Batch tillhandahåller två typer av användarkonton för pågående aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="dfcff-110">Azure Batch provides two types of user accounts for running tasks:</span></span>

- <span data-ttu-id="dfcff-111">**Automatisk användarkonton.**</span><span class="sxs-lookup"><span data-stu-id="dfcff-111">**Auto-user accounts.**</span></span> <span data-ttu-id="dfcff-112">Auto-användarkonton är inbyggda användarkonton som skapas automatiskt av Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dfcff-112">Auto-user accounts are built-in user accounts that are created automatically by the Batch service.</span></span> <span data-ttu-id="dfcff-113">Som standard körs aktiviteter under en automatisk användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-113">By default, tasks run under an auto-user account.</span></span> <span data-ttu-id="dfcff-114">Du kan konfigurera automatisk användaren specifikationen för en aktivitet att indikera en aktivitet ska köras under vilket auto-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-114">You can configure the auto-user specification for a task to indicate under which auto-user account a task should run.</span></span> <span data-ttu-id="dfcff-115">Specifikationen auto-användare kan du ange höjning nivå och omfattningen av automatisk-användarkonto som ska köra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="dfcff-115">The auto-user specification allows you to specify the elevation level and scope of the auto-user account that will run the task.</span></span> 

- <span data-ttu-id="dfcff-116">**En namngiven användarkonto.**</span><span class="sxs-lookup"><span data-stu-id="dfcff-116">**A named user account.**</span></span> <span data-ttu-id="dfcff-117">Du kan ange en eller flera namngivna användarkonton för en pool när du skapar poolen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-117">You can specify one or more named user accounts for a pool when you create the pool.</span></span> <span data-ttu-id="dfcff-118">Varje användarkonto skapas på varje nod i poolen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-118">Each user account is created on each node of the pool.</span></span> <span data-ttu-id="dfcff-119">Förutom kontonamnet ange användarens kontolösenord höjning nivå och, för Linux pooler privata SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="dfcff-119">In addition to the account name, you specify the user account password, elevation level, and, for Linux pools, the SSH private key.</span></span> <span data-ttu-id="dfcff-120">När du lägger till en aktivitet kan du ange namngivna användarkontot som aktiviteten ska köras under.</span><span class="sxs-lookup"><span data-stu-id="dfcff-120">When you add a task, you can specify the named user account under which that task should run.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="dfcff-121">Batch-tjänstversion 2017-01-01.4.0 introducerar en ny ändring som kräver att du uppdaterar din kod för att anropa den här versionen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-121">The Batch service version 2017-01-01.4.0 introduces a breaking change that requires that you update your code to call that version.</span></span> <span data-ttu-id="dfcff-122">Om du migrerar kod från en äldre version av Batch, Observera att den **runElevated** egenskapen stöds inte längre i klientbibliotek REST API eller Batch.</span><span class="sxs-lookup"><span data-stu-id="dfcff-122">If you are migrating code from an older version of Batch, note that the **runElevated** property is no longer supported in the REST API or Batch client libraries.</span></span> <span data-ttu-id="dfcff-123">Använd den nya **userIdentity** -egenskapen för en aktivitet för att ange nivån för höjning.</span><span class="sxs-lookup"><span data-stu-id="dfcff-123">Use the new **userIdentity** property of a task to specify elevation level.</span></span> <span data-ttu-id="dfcff-124">Se avsnittet [uppdatera din kod till det senaste batchen klientbiblioteket](#update-your-code-to-the-latest-batch-client-library) snabb riktlinjer för uppdatering av Batch-kod om du använder en av klientbiblioteken.</span><span class="sxs-lookup"><span data-stu-id="dfcff-124">See the section titled [Update your code to the latest Batch client library](#update-your-code-to-the-latest-batch-client-library) for quick guidelines for updating your Batch code if you are using one of the client libraries.</span></span>
>
>

> [!NOTE] 
> <span data-ttu-id="dfcff-125">Användarkonton som beskrivs i den här artikeln stöder inte Remote Desktop Protocol (RDP) eller SSH (Secure Shell), av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="dfcff-125">The user accounts discussed in this article do not support Remote Desktop Protocol (RDP) or Secure Shell (SSH), for security reasons.</span></span> 
>
> <span data-ttu-id="dfcff-126">För att ansluta till en nod som kör Linux virtuella datorkonfigurationen via SSH finns [Använd Fjärrskrivbord till en Linux-VM i Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="dfcff-126">To connect to a node running the Linux virtual machine configuration via SSH, see [Use Remote Desktop to a Linux VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span></span> <span data-ttu-id="dfcff-127">Om du vill ansluta till noder som kör Windows via RDP finns [Anslut till en Windows Server-VM](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="dfcff-127">To connect to nodes running Windows via RDP, see [Connect to a Windows Server VM](../virtual-machines/windows/connect-logon.md).</span></span><br /><br />
> <span data-ttu-id="dfcff-128">För att ansluta till en nod som kör tjänsten molnkonfigurationen via RDP finns [aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dfcff-128">To connect to a node running the cloud service configuration via RDP, see [Enable Remote Desktop Connection for a Role in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span></span>
>
>

## <a name="user-account-access-to-files-and-directories"></a><span data-ttu-id="dfcff-129">Användarkontoåtkomst till filer och kataloger</span><span class="sxs-lookup"><span data-stu-id="dfcff-129">User account access to files and directories</span></span>

<span data-ttu-id="dfcff-130">Både ett automatiskt användarkonto och ett namngivet användarkonto har läsning och skrivning åtkomst till aktivitetens arbetskatalogen delad katalog och flera instanser uppgifter directory.</span><span class="sxs-lookup"><span data-stu-id="dfcff-130">Both an auto-user account and a named user account have read/write access to the task’s working directory, shared directory, and multi-instance tasks directory.</span></span> <span data-ttu-id="dfcff-131">Båda typer av konton har läsbehörighet till Start- och förberedelse av kataloger.</span><span class="sxs-lookup"><span data-stu-id="dfcff-131">Both types of accounts have read access to the startup and job preparation directories.</span></span>

<span data-ttu-id="dfcff-132">Om en aktivitet ska köras under samma konto som användes för att köra Startuppgiften, har skrivskyddad åtkomst till katalogen för aktiviteten starta uppgiften.</span><span class="sxs-lookup"><span data-stu-id="dfcff-132">If a task runs under the same account that was used for running a start task, the task has read-write access to the start task directory.</span></span> <span data-ttu-id="dfcff-133">På samma sätt, om en aktivitet ska köras under samma konto som användes för att köra en projektaktivitet förberedelse, aktiviteten har skrivskyddad åtkomst till katalogen jobbet förberedelse av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="dfcff-133">Similarly, if a task runs under the same account that was used for running a job preparation task, the task has read-write access to the job preparation task directory.</span></span> <span data-ttu-id="dfcff-134">Om en aktivitet körs under ett annat konto än startuppgift eller förberedelse projektaktivitet har aktiviteten bara läsbehörighet till katalogen för respektive.</span><span class="sxs-lookup"><span data-stu-id="dfcff-134">If a task runs under a different account than the start task or job preparation task, then the task has only read access to the respective directory.</span></span>

<span data-ttu-id="dfcff-135">Mer information om hur du använder filer och kataloger från en aktivitet finns [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md#files-and-directories).</span><span class="sxs-lookup"><span data-stu-id="dfcff-135">For more information on accessing files and directories from a task, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#files-and-directories).</span></span>

## <a name="elevated-access-for-tasks"></a><span data-ttu-id="dfcff-136">Utökad åtkomst för aktiviteter</span><span class="sxs-lookup"><span data-stu-id="dfcff-136">Elevated access for tasks</span></span> 

<span data-ttu-id="dfcff-137">Användarkontots höjning nivå anger om en aktivitet körs med förhöjd åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dfcff-137">The user account's elevation level indicates whether a task runs with elevated access.</span></span> <span data-ttu-id="dfcff-138">Både ett automatiskt användarkonto och ett konto för namngiven användare kan köra med högre behörighet.</span><span class="sxs-lookup"><span data-stu-id="dfcff-138">Both an auto-user account and a named user account can run with elevated access.</span></span> <span data-ttu-id="dfcff-139">De två alternativen för höjning nivå är:</span><span class="sxs-lookup"><span data-stu-id="dfcff-139">The two options for elevation level are:</span></span>

- <span data-ttu-id="dfcff-140">**NonAdmin:** aktiviteten körs som en vanlig användare utan utökad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dfcff-140">**NonAdmin:** The task runs as a standard user without elevated access.</span></span> <span data-ttu-id="dfcff-141">Standardnivån för höjning för ett användarkonto för Batch är alltid **NonAdmin**.</span><span class="sxs-lookup"><span data-stu-id="dfcff-141">The default elevation level for a Batch user account is always **NonAdmin**.</span></span>
- <span data-ttu-id="dfcff-142">**Admin:** uppgiften körs som en användare med högre behörighet och fungerar med fullständig administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="dfcff-142">**Admin:** The task runs as a user with elevated access and operates with full Administrator permissions.</span></span> 

## <a name="auto-user-accounts"></a><span data-ttu-id="dfcff-143">Auto-användarkonton</span><span class="sxs-lookup"><span data-stu-id="dfcff-143">Auto-user accounts</span></span>

<span data-ttu-id="dfcff-144">Som standard köra uppgifter i Batch under en auto-användarkonto som en vanlig användare utan utökad åtkomst och med aktiviteten omfattning.</span><span class="sxs-lookup"><span data-stu-id="dfcff-144">By default, tasks run in Batch under an auto-user account, as a standard user without elevated access, and with task scope.</span></span> <span data-ttu-id="dfcff-145">När specifikationen auto-användare konfigureras för omfånget för aktiviteten skapar Batch-tjänsten en automatisk användarkontot för aktiviteten endast.</span><span class="sxs-lookup"><span data-stu-id="dfcff-145">When the auto-user specification is configured for task scope, the Batch service creates an auto-user account for that task only.</span></span>

<span data-ttu-id="dfcff-146">Alternativ till aktiviteten scope är poolen omfång.</span><span class="sxs-lookup"><span data-stu-id="dfcff-146">The alternative to task scope is pool scope.</span></span> <span data-ttu-id="dfcff-147">När automatisk användaren specifikationen för en aktivitet är konfigurerad för poolen omfång, körs aktiviteten under en auto-användarkonto som är tillgänglig för alla aktiviteter i poolen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-147">When the auto-user specification for a task is configured for pool scope, the task runs under an auto-user account that is available to any task in the pool.</span></span> <span data-ttu-id="dfcff-148">Mer information om poolen scope finns i avsnittet [köra en aktivitet som automatiskt-användare med poolen scope](#run-a-task-as-the-autouser-with-pool-scope).</span><span class="sxs-lookup"><span data-stu-id="dfcff-148">For more information about pool scope, see the section titled [Run a task as the auto-user with pool scope](#run-a-task-as-the-autouser-with-pool-scope).</span></span>   

<span data-ttu-id="dfcff-149">Standardvärde skiljer sig på Windows- och Linux-noder:</span><span class="sxs-lookup"><span data-stu-id="dfcff-149">The default scope is different on Windows and Linux nodes:</span></span>

- <span data-ttu-id="dfcff-150">På Windows-noder körs aktiviteter under scope på uppgift som standard.</span><span class="sxs-lookup"><span data-stu-id="dfcff-150">On Windows nodes, tasks run under task scope by default.</span></span>
- <span data-ttu-id="dfcff-151">Linux-noder kan alltid köras under poolen omfång.</span><span class="sxs-lookup"><span data-stu-id="dfcff-151">Linux nodes always run under pool scope.</span></span>

<span data-ttu-id="dfcff-152">Det finns fyra konfigurationer för automatisk användar-specifikationen som motsvarar ett unikt auto-konto:</span><span class="sxs-lookup"><span data-stu-id="dfcff-152">There are four possible configurations for the auto-user specification, each of which corresponds to a unique auto-user account:</span></span>

- <span data-ttu-id="dfcff-153">Icke-administratörsåtkomst med uppgiften omfattning (standardtypen användare automatiskt)</span><span class="sxs-lookup"><span data-stu-id="dfcff-153">Non-admin access with task scope (the default auto-user specification)</span></span>
- <span data-ttu-id="dfcff-154">Administratörsåtkomst (förhöjd) med uppgiften scope</span><span class="sxs-lookup"><span data-stu-id="dfcff-154">Admin (elevated) access with task scope</span></span>
- <span data-ttu-id="dfcff-155">Icke-administratörsåtkomst med poolen scope</span><span class="sxs-lookup"><span data-stu-id="dfcff-155">Non-admin access with pool scope</span></span>
- <span data-ttu-id="dfcff-156">Administratörsåtkomst med poolen scope</span><span class="sxs-lookup"><span data-stu-id="dfcff-156">Admin access with pool scope</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="dfcff-157">Aktiviteter som körs under aktiviteten scope har inte faktisk åtkomst till andra aktiviteter på en nod.</span><span class="sxs-lookup"><span data-stu-id="dfcff-157">Tasks running under task scope do not have de facto access to other tasks on a node.</span></span> <span data-ttu-id="dfcff-158">Men kan en obehörig användare med åtkomst till kontot undvika den här begränsningen genom att skicka en aktivitet som körs med administratörsbehörighet och har åtkomst till andra kataloger som aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="dfcff-158">However, a malicious user with access to the account could work around this restriction by submitting a task that runs with administrator privileges and accesses other task directories.</span></span> <span data-ttu-id="dfcff-159">En obehörig användare kan också använda RDP eller SSH för att ansluta till en nod.</span><span class="sxs-lookup"><span data-stu-id="dfcff-159">A malicious user could also use RDP or SSH to connect to a node.</span></span> <span data-ttu-id="dfcff-160">Det är viktigt att skydda åtkomst till dina nycklar med Batch-konto att förhindra att ett sådant scenario.</span><span class="sxs-lookup"><span data-stu-id="dfcff-160">It's important to protect access to your Batch account keys to prevent such a scenario.</span></span> <span data-ttu-id="dfcff-161">Om du misstänker att ditt konto komprometteras, måste du återskapa dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="dfcff-161">If you suspect your account may have been compromised, be sure to regenerate your keys.</span></span>
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a><span data-ttu-id="dfcff-162">Köra en aktivitet som automatiskt-användare med högre behörighet</span><span class="sxs-lookup"><span data-stu-id="dfcff-162">Run a task as an auto-user with elevated access</span></span>

<span data-ttu-id="dfcff-163">Du kan konfigurera automatisk användaren specifikationen för administratörsbehörighet när du behöver köra en aktivitet med högre behörighet.</span><span class="sxs-lookup"><span data-stu-id="dfcff-163">You can configure the auto-user specification for administrator privileges when you need to run a task with elevated access.</span></span> <span data-ttu-id="dfcff-164">Till exempel behöva Startuppgiften utökad åtkomst till installera programvara på noden.</span><span class="sxs-lookup"><span data-stu-id="dfcff-164">For example, a start task may need elevated access to install software on the node.</span></span>

> [!NOTE] 
> <span data-ttu-id="dfcff-165">I allmänhet är det bäst att använda utökad åtkomst bara när det behövs.</span><span class="sxs-lookup"><span data-stu-id="dfcff-165">In general, it's best to use elevated access only when necessary.</span></span> <span data-ttu-id="dfcff-166">Bästa praxis rekommenderar bevilja det minsta privilegiet som krävs för att uppnå önskat utfall.</span><span class="sxs-lookup"><span data-stu-id="dfcff-166">Best practices recommend granting the minimum privilege necessary to achieve the desired outcome.</span></span> <span data-ttu-id="dfcff-167">Till exempel om Startuppgiften installerar programvara för den aktuella användaren, i stället för för alla användare, du kan undvika utökade åtkomst beviljas till aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="dfcff-167">For example, if a start task installs software for the current user, instead of for all users, you may be able to avoid granting elevated access to tasks.</span></span> <span data-ttu-id="dfcff-168">Du kan konfigurera automatisk användaren specifikationen för poolen omfång och icke-administratörer åtkomst för alla aktiviteter som måste köras under samma konto, inklusive Startuppgiften.</span><span class="sxs-lookup"><span data-stu-id="dfcff-168">You can configure the auto-user specification for pool scope and non-admin access for all tasks that need to run under the same account, including the start task.</span></span> 
>
>

<span data-ttu-id="dfcff-169">Följande kodavsnitt visar hur du konfigurerar automatisk användar-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-169">The following code snippets show how to configure the auto-user specification.</span></span> <span data-ttu-id="dfcff-170">Exemplen anger höjningen till `Admin` och omfång till `Task`.</span><span class="sxs-lookup"><span data-stu-id="dfcff-170">The examples set the elevation level to `Admin` and the scope to `Task`.</span></span> <span data-ttu-id="dfcff-171">Uppgiften scope är standardinställningen, men finns här för enkelhetens exempel.</span><span class="sxs-lookup"><span data-stu-id="dfcff-171">Task scope is the default setting, but is included here for the sake of example.</span></span>

#### <a name="batch-net"></a><span data-ttu-id="dfcff-172">.NET för Batch</span><span class="sxs-lookup"><span data-stu-id="dfcff-172">Batch .NET</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a><span data-ttu-id="dfcff-173">Batch Java</span><span class="sxs-lookup"><span data-stu-id="dfcff-173">Batch Java</span></span>

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a><span data-ttu-id="dfcff-174">Python för Batch</span><span class="sxs-lookup"><span data-stu-id="dfcff-174">Batch Python</span></span>

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

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a><span data-ttu-id="dfcff-175">Köra en aktivitet som automatiskt-användare med poolen scope</span><span class="sxs-lookup"><span data-stu-id="dfcff-175">Run a task as an auto-user with pool scope</span></span>

<span data-ttu-id="dfcff-176">När en nod har etablerats skapas två poolen hela auto-användarkonton på varje nod i poolen, en med högre behörighet och en utan utökad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dfcff-176">When a node is provisioned, two pool-wide auto-user accounts are created on each node in the pool, one with elevated access, and one without elevated access.</span></span> <span data-ttu-id="dfcff-177">Ställa in automatisk-användarens omfång till poolen omfattningen för en viss aktivitet kör aktiviteten under någon av dessa två poolen hela auto-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="dfcff-177">Setting the auto-user's scope to pool scope for a given task runs the task under one of these two pool-wide auto-user accounts.</span></span> 

<span data-ttu-id="dfcff-178">När du anger poolen omfång för automatisk-användaren alla aktiviteter som körs med administratörsåtkomst köras under samma pool hela auto-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-178">When you specify pool scope for the auto-user, all tasks that run with administrator access run under the same pool-wide auto-user account.</span></span> <span data-ttu-id="dfcff-179">Aktiviteter som körs utan administratörsbehörighet körs på samma sätt även under samma pool hela auto-konto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-179">Similarly, tasks that run without administrator permissions also run under a single pool-wide auto-user account.</span></span> 

> [!NOTE] 
> <span data-ttu-id="dfcff-180">Två poolen hela auto-användarkontona är separata konton.</span><span class="sxs-lookup"><span data-stu-id="dfcff-180">The two pool-wide auto-user accounts are separate accounts.</span></span> <span data-ttu-id="dfcff-181">Aktiviteter som körs under kontot poolen hela administrativa kan inte dela data med aktiviteter som körs under kontot standard och vice versa.</span><span class="sxs-lookup"><span data-stu-id="dfcff-181">Tasks running under the pool-wide administrative account cannot share data with tasks running under the standard account, and vice versa.</span></span> 
>
>

<span data-ttu-id="dfcff-182">Fördelen med att köras under samma auto-användarkontot är att aktiviteter kan dela data med andra aktiviteter som körs på samma nod.</span><span class="sxs-lookup"><span data-stu-id="dfcff-182">The advantage to running under the same auto-user account is that tasks are able to share data with other tasks running on the same node.</span></span>

<span data-ttu-id="dfcff-183">Dela hemligheter mellan aktiviteter är ett scenario där pågående aktiviteter under en av två poolen hela auto-användarkontona är användbart.</span><span class="sxs-lookup"><span data-stu-id="dfcff-183">Sharing secrets between tasks is one scenario where running tasks under one of the two pool-wide auto-user accounts is useful.</span></span> <span data-ttu-id="dfcff-184">Anta exempelvis att Startuppgiften måste tillhandahålla en hemlighet på den nod som andra uppgifter kan använda.</span><span class="sxs-lookup"><span data-stu-id="dfcff-184">For example, suppose a start task needs to provision a secret onto the node that other tasks can use.</span></span> <span data-ttu-id="dfcff-185">Du kan använda Windows Data Protection API (DPAPI), men det krävs administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="dfcff-185">You could use the Windows Data Protection API (DPAPI), but it requires administrator privileges.</span></span> <span data-ttu-id="dfcff-186">I stället kan du skydda hemligheten på användarnivå.</span><span class="sxs-lookup"><span data-stu-id="dfcff-186">Instead, you can protect the secret at the user level.</span></span> <span data-ttu-id="dfcff-187">Aktiviteter som körs under samma användarkonto kan komma åt hemligheten utan utökad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dfcff-187">Tasks running under the same user account can access the secret without elevated access.</span></span>

<span data-ttu-id="dfcff-188">Ett annat scenario där du kanske vill köra uppgifter under en automatisk användarkonto med poolen scope är en filresurs för Message Passing Interface (MPI).</span><span class="sxs-lookup"><span data-stu-id="dfcff-188">Another scenario where you may want to run tasks under an auto-user account with pool scope is a Message Passing Interface (MPI) file share.</span></span> <span data-ttu-id="dfcff-189">En filresurs för MPI är användbart när noderna i aktiviteten MPI behöver arbeta på samma fildata.</span><span class="sxs-lookup"><span data-stu-id="dfcff-189">An MPI file share is useful when the nodes in the MPI task need to work on the same file data.</span></span> <span data-ttu-id="dfcff-190">Huvudnoden skapar en filresurs som underordnade noder har åtkomst till om de körs under samma auto-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-190">The head node creates a file share that the child nodes can access if they are running under the same auto-user account.</span></span> 

<span data-ttu-id="dfcff-191">Följande kodavsnitt anger auto-användarens omfång till poolen omfattningen för en aktivitet i Batch .NET.</span><span class="sxs-lookup"><span data-stu-id="dfcff-191">The following code snippet sets the auto-user's scope to pool scope for a task in Batch .NET.</span></span> <span data-ttu-id="dfcff-192">Nivån höjning utelämnas så aktiviteten körs under kontot poolen hela auto-standardanvändare.</span><span class="sxs-lookup"><span data-stu-id="dfcff-192">The elevation level is omitted, so the task runs under the standard pool-wide auto-user account.</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a><span data-ttu-id="dfcff-193">Namngiven användare</span><span class="sxs-lookup"><span data-stu-id="dfcff-193">Named user accounts</span></span>

<span data-ttu-id="dfcff-194">Du kan definiera namngivna användarkonton när du skapar en pool.</span><span class="sxs-lookup"><span data-stu-id="dfcff-194">You can define named user accounts when you create a pool.</span></span> <span data-ttu-id="dfcff-195">En namngiven användarkonto har ett namn och lösenord som du anger.</span><span class="sxs-lookup"><span data-stu-id="dfcff-195">A named user account has a name and password that you provide.</span></span> <span data-ttu-id="dfcff-196">Du kan ange höjning-nivå för en namngiven användarkontot.</span><span class="sxs-lookup"><span data-stu-id="dfcff-196">You can specify the elevation level for a named user account.</span></span> <span data-ttu-id="dfcff-197">Du kan också tillhandahålla en privata SSH-nyckel för Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="dfcff-197">For Linux nodes, you can also provide an SSH private key.</span></span>

<span data-ttu-id="dfcff-198">En namngiven användarkontot finns på alla noder i poolen och är tillgänglig för alla aktiviteter som körs på noderna.</span><span class="sxs-lookup"><span data-stu-id="dfcff-198">A named user account exists on all nodes in the pool and is available to all tasks running on those nodes.</span></span> <span data-ttu-id="dfcff-199">Du kan definiera valfritt antal namngivna användare för en pool.</span><span class="sxs-lookup"><span data-stu-id="dfcff-199">You may define any number of named users for a pool.</span></span> <span data-ttu-id="dfcff-200">När du lägger till en aktivitet eller en uppgift samling kan du ange att aktiviteten körs under ett konto för namngiven användare som definierats i poolen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-200">When you add a task or task collection, you can specify that the task runs under one of the named user accounts defined on the pool.</span></span>

<span data-ttu-id="dfcff-201">Ett namngivet användarkonto är användbart när du vill köra alla uppgifter i ett jobb under samma användarkonto, men kan isolera dem från aktiviteter som körs i andra jobb på samma gång.</span><span class="sxs-lookup"><span data-stu-id="dfcff-201">A named user account is useful when you want to run all tasks in a job under the same user account, but isolate them from tasks running in other jobs at the same time.</span></span> <span data-ttu-id="dfcff-202">Du kan till exempel skapa en namngiven användare för varje jobb och köra varje jobb uppgifter under det namngivna användarkontot.</span><span class="sxs-lookup"><span data-stu-id="dfcff-202">For example, you can create a named user for each job, and run each job's tasks under that named user account.</span></span> <span data-ttu-id="dfcff-203">Varje jobb kan sedan delar en hemlighet med egna aktiviteter, men inte med aktiviteter som körs i andra jobb.</span><span class="sxs-lookup"><span data-stu-id="dfcff-203">Each job can then share a secret with its own tasks, but not with tasks running in other jobs.</span></span>

<span data-ttu-id="dfcff-204">Du kan också använda ett med namnet konto för att köra en aktivitet som anger behörigheter på externa resurser, till exempel filresurser.</span><span class="sxs-lookup"><span data-stu-id="dfcff-204">You can also use a named user account to run a task that sets permissions on external resources such as file shares.</span></span> <span data-ttu-id="dfcff-205">Med en namngiven användarkonto styra användarnas identitet och användarens identitet kan använda för att ange behörigheter.</span><span class="sxs-lookup"><span data-stu-id="dfcff-205">With a named user account, you control the user identity and can use that user identity to set permissions.</span></span>  

<span data-ttu-id="dfcff-206">Namngiven användare aktivera lösenord mindre SSH mellan Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="dfcff-206">Named user accounts enable password-less SSH between Linux nodes.</span></span> <span data-ttu-id="dfcff-207">Du kan använda en namngiven användarkonto med Linux-noder som behöver för att köra flera instanser uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dfcff-207">You can use a named user account with Linux nodes that need to run multi-instance tasks.</span></span> <span data-ttu-id="dfcff-208">Varje nod i poolen kan köra uppgifter under ett användarkonto som har definierats för hela poolen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-208">Each node in the pool can run tasks under a user account defined on the whole pool.</span></span> <span data-ttu-id="dfcff-209">Mer information om flera instanser uppgifter finns [använder flera\-instansen aktiviteter att köra MPI program](batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="dfcff-209">For more information about multi-instance tasks, see [Use multi\-instance tasks to run MPI applications](batch-mpi.md).</span></span>

### <a name="create-named-user-accounts"></a><span data-ttu-id="dfcff-210">Skapa namngiven användarkonton</span><span class="sxs-lookup"><span data-stu-id="dfcff-210">Create named user accounts</span></span>

<span data-ttu-id="dfcff-211">Om du vill skapa namngiven användarkonton i Batch, lägger du till en samling av användarkonton i poolen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-211">To create named user accounts in Batch, add a collection of user accounts to the pool.</span></span> <span data-ttu-id="dfcff-212">Följande kodavsnitt visar hur du skapar namngivna användarkonton i .NET, Java och Python.</span><span class="sxs-lookup"><span data-stu-id="dfcff-212">The following code snippets show how to create named user accounts in .NET, Java, and Python.</span></span> <span data-ttu-id="dfcff-213">Dessa kodstycken visar hur du skapar både admin och icke-administratörer med namnet konton i en pool.</span><span class="sxs-lookup"><span data-stu-id="dfcff-213">These code snippets show how to create both admin and non-admin named accounts on a pool.</span></span> <span data-ttu-id="dfcff-214">Exemplen skapa pooler med tjänstkonfigurationen moln, men du kan använda samma metod när du skapar en Windows- eller Linux-pool med konfigurationen av virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="dfcff-214">The examples create pools using the cloud service configuration, but you use the same approach when creating a Windows or Linux pool using the virtual machine configuration.</span></span>

#### <a name="batch-net-example-windows"></a><span data-ttu-id="dfcff-215">Batch .NET-exempel (Windows)</span><span class="sxs-lookup"><span data-stu-id="dfcff-215">Batch .NET example (Windows)</span></span>

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
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

// Commit the pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a><span data-ttu-id="dfcff-216">Batch .NET-exempel (Linux)</span><span class="sxs-lookup"><span data-stu-id="dfcff-216">Batch .NET example (Linux)</span></span>

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the virtual machine configuration to use to create the pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create the unbound pool.
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

// Commit the pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a><span data-ttu-id="dfcff-217">Batch Java-exempel</span><span class="sxs-lookup"><span data-stu-id="dfcff-217">Batch Java example</span></span>

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

#### <a name="batch-python-example"></a><span data-ttu-id="dfcff-218">Batch Python-exempel</span><span class="sxs-lookup"><span data-stu-id="dfcff-218">Batch Python example</span></span>

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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a><span data-ttu-id="dfcff-219">Köra en aktivitet för en namngiven användarkonto med högre behörighet</span><span class="sxs-lookup"><span data-stu-id="dfcff-219">Run a task under a named user account with elevated access</span></span>

<span data-ttu-id="dfcff-220">Ange om du vill köra en aktivitet som en förhöjd aktivitetens **UserIdentity** egenskapen till ett namngivet användarkonto som har skapats med dess **ElevationLevel** egenskapen `Admin`.</span><span class="sxs-lookup"><span data-stu-id="dfcff-220">To run a task as an elevated user, set the task's **UserIdentity** property to a named user account that was created with its **ElevationLevel** property set to `Admin`.</span></span>

<span data-ttu-id="dfcff-221">Det här kodstycket anger att aktiviteten ska köras under en namngiven användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dfcff-221">This code snippet specifies that the task should run under a named user account.</span></span> <span data-ttu-id="dfcff-222">Namngivna användarkontot har definierats för poolen när poolen har skapats.</span><span class="sxs-lookup"><span data-stu-id="dfcff-222">This named user account was defined on the pool when the pool was created.</span></span> <span data-ttu-id="dfcff-223">I det här fallet, har namngiven användarkontot skapats med administratörsbehörigheter:</span><span class="sxs-lookup"><span data-stu-id="dfcff-223">In this case, the named user account was created with admin permissions:</span></span>

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a><span data-ttu-id="dfcff-224">Uppdatera din kod till det senaste Batch-klientbiblioteket</span><span class="sxs-lookup"><span data-stu-id="dfcff-224">Update your code to the latest Batch client library</span></span>

<span data-ttu-id="dfcff-225">Batch-tjänstversion 2017-01-01.4.0 introducerar en ny ändring ersätter den **runElevated** egenskap som är tillgänglig i tidigare versioner med den **userIdentity** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="dfcff-225">The Batch service version 2017-01-01.4.0 introduces a breaking change, replacing the **runElevated** property available in earlier versions with the **userIdentity** property.</span></span> <span data-ttu-id="dfcff-226">Följande tabeller innehåller en enkel mappning som du kan använda för att uppdatera din kod från tidigare versioner av klientbiblioteken.</span><span class="sxs-lookup"><span data-stu-id="dfcff-226">The following tables provide a simple mapping that you can use to update your code from earlier versions of the client libraries.</span></span>

### <a name="batch-net"></a><span data-ttu-id="dfcff-227">.NET för Batch</span><span class="sxs-lookup"><span data-stu-id="dfcff-227">Batch .NET</span></span>

| <span data-ttu-id="dfcff-228">Om din kod använder...</span><span class="sxs-lookup"><span data-stu-id="dfcff-228">If your code uses...</span></span>                  | <span data-ttu-id="dfcff-229">Uppdatera det till...</span><span class="sxs-lookup"><span data-stu-id="dfcff-229">Update it to....</span></span>                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| <span data-ttu-id="dfcff-230">`CloudTask.RunElevated`inte angetts</span><span class="sxs-lookup"><span data-stu-id="dfcff-230">`CloudTask.RunElevated` not specified</span></span> | <span data-ttu-id="dfcff-231">Ingen uppdatering krävs</span><span class="sxs-lookup"><span data-stu-id="dfcff-231">No update required</span></span>                                                                                               |

### <a name="batch-java"></a><span data-ttu-id="dfcff-232">Batch Java</span><span class="sxs-lookup"><span data-stu-id="dfcff-232">Batch Java</span></span>

| <span data-ttu-id="dfcff-233">Om din kod använder...</span><span class="sxs-lookup"><span data-stu-id="dfcff-233">If your code uses...</span></span>                      | <span data-ttu-id="dfcff-234">Uppdatera det till...</span><span class="sxs-lookup"><span data-stu-id="dfcff-234">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| <span data-ttu-id="dfcff-235">`CloudTask.withRunElevated`inte angetts</span><span class="sxs-lookup"><span data-stu-id="dfcff-235">`CloudTask.withRunElevated` not specified</span></span> | <span data-ttu-id="dfcff-236">Ingen uppdatering krävs</span><span class="sxs-lookup"><span data-stu-id="dfcff-236">No update required</span></span>                                                                                                                     |

### <a name="batch-python"></a><span data-ttu-id="dfcff-237">Python för Batch</span><span class="sxs-lookup"><span data-stu-id="dfcff-237">Batch Python</span></span>

| <span data-ttu-id="dfcff-238">Om din kod använder...</span><span class="sxs-lookup"><span data-stu-id="dfcff-238">If your code uses...</span></span>                      | <span data-ttu-id="dfcff-239">Uppdatera det till...</span><span class="sxs-lookup"><span data-stu-id="dfcff-239">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | <span data-ttu-id="dfcff-240">`user_identity=user`, där</span><span class="sxs-lookup"><span data-stu-id="dfcff-240">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | <span data-ttu-id="dfcff-241">`user_identity=user`, där</span><span class="sxs-lookup"><span data-stu-id="dfcff-241">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| <span data-ttu-id="dfcff-242">`run_elevated`inte angetts</span><span class="sxs-lookup"><span data-stu-id="dfcff-242">`run_elevated` not specified</span></span> | <span data-ttu-id="dfcff-243">Ingen uppdatering krävs</span><span class="sxs-lookup"><span data-stu-id="dfcff-243">No update required</span></span>                                                                                                                                  |


## <a name="next-steps"></a><span data-ttu-id="dfcff-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfcff-244">Next steps</span></span>

### <a name="batch-forum"></a><span data-ttu-id="dfcff-245">Batch-Forum</span><span class="sxs-lookup"><span data-stu-id="dfcff-245">Batch Forum</span></span>

<span data-ttu-id="dfcff-246">Den [Azure Batch-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) på MSDN är det bra att diskutera Batch och ställa frågor om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dfcff-246">The [Azure Batch Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) on MSDN is a great place to discuss Batch and ask questions about the service.</span></span> <span data-ttu-id="dfcff-247">HEAD på över för användbara Fäst inlägg och publicera dina frågor när de uppstår när du skapar Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="dfcff-247">Head on over for helpful pinned posts, and post your questions as they arise while you build your Batch solutions.</span></span>