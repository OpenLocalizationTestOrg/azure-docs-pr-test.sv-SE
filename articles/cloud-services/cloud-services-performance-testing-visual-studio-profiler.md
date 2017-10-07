---
title: "en molnet tjänsten lokalt i hello Compute Emulator aaaProfiling | Microsoft Docs"
services: cloud-services
description: "Undersöka prestandaproblem i molntjänster med hello Visual Studio-profiler"
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
tags: 
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: fc37c85dad4db4cc0310f73afad56fc0fe5f3963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service-locally-in-hello-azure-compute-emulator-using-hello-visual-studio-profiler"></a><span data-ttu-id="7c772-103">Testa hello prestanda för en moln-tjänsten lokalt i hello Azure Compute Emulator med hello Visual Studio-Profiler</span><span class="sxs-lookup"><span data-stu-id="7c772-103">Testing hello Performance of a Cloud Service Locally in hello Azure Compute Emulator Using hello Visual Studio Profiler</span></span>
<span data-ttu-id="7c772-104">En mängd olika verktyg och tekniker är tillgängliga för att testa hello prestanda för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="7c772-104">A variety of tools and techniques are available for testing hello performance of cloud services.</span></span>
<span data-ttu-id="7c772-105">När du publicerar en cloud service tooAzure du har Visual Studio samla in profileringsdata och analysera den lokalt, enligt beskrivningen i [profilering ett Azure-program][1].</span><span class="sxs-lookup"><span data-stu-id="7c772-105">When you publish a cloud service tooAzure, you can have Visual Studio collect profiling data and then analyze it locally, as described in [Profiling an Azure Application][1].</span></span>
<span data-ttu-id="7c772-106">Du kan också använda diagnostik tootrack räknare för en mängd olika prestanda, enligt beskrivningen i [med hjälp av prestandaräknare i Azure][2].</span><span class="sxs-lookup"><span data-stu-id="7c772-106">You can also use diagnostics tootrack a variety of performance counters, as described in [Using performance counters in Azure][2].</span></span>
<span data-ttu-id="7c772-107">Du kan också tooprofile programmet lokalt i hello beräkningsemulatorn innan du distribuerar den toohello moln.</span><span class="sxs-lookup"><span data-stu-id="7c772-107">You might also want tooprofile your application locally in hello compute emulator before deploying it toohello cloud.</span></span>

<span data-ttu-id="7c772-108">Den här artikeln beskriver hello CPU urvalsmetoden för profilering, vilket kan göras lokalt i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="7c772-108">This article covers hello CPU Sampling method of profiling, which can be done locally in hello emulator.</span></span> <span data-ttu-id="7c772-109">CPU-provtagning är en metod för profilering som inte är mycket påträngande.</span><span class="sxs-lookup"><span data-stu-id="7c772-109">CPU sampling is a method of profiling that is not very intrusive.</span></span> <span data-ttu-id="7c772-110">Vid en angiven insamlingsintervall tar hello profiler en ögonblicksbild av hello anropsstacken.</span><span class="sxs-lookup"><span data-stu-id="7c772-110">At a designated sampling interval, hello profiler takes a snapshot of hello call stack.</span></span> <span data-ttu-id="7c772-111">hello data som samlas in under en viss tidsperiod, och visas i en rapport.</span><span class="sxs-lookup"><span data-stu-id="7c772-111">hello data is collected over a period of time, and shown in a report.</span></span> <span data-ttu-id="7c772-112">Den här metoden för profilering tenderar tooindicate där i ett beräkningsmässigt intensiva program de flesta av hello CPU arbetet görs.</span><span class="sxs-lookup"><span data-stu-id="7c772-112">This method of profiling tends tooindicate where in a computationally intensive application most of hello CPU work is being done.</span></span>  <span data-ttu-id="7c772-113">Detta ger hello möjlighet toofocus på hello ”varm path” där programmet tillbringar hello de flesta tid.</span><span class="sxs-lookup"><span data-stu-id="7c772-113">This gives you hello opportunity toofocus on hello "hot path" where your application is spending hello most time.</span></span>

## <a name="1-configure-visual-studio-for-profiling"></a><span data-ttu-id="7c772-114">1: Konfigurera Visual Studio för profilering</span><span class="sxs-lookup"><span data-stu-id="7c772-114">1: Configure Visual Studio for profiling</span></span>
<span data-ttu-id="7c772-115">Det finns först ett fåtal konfigurationsalternativ för Visual Studio som kan vara användbara när profilering.</span><span class="sxs-lookup"><span data-stu-id="7c772-115">First, there are a few Visual Studio configuration options that might be helpful when profiling.</span></span> <span data-ttu-id="7c772-116">toomake uppfattning om hello profilering rapporter måste du symboler (.pdb-filer) för programmet och även symboler för bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7c772-116">toomake sense of hello profiling reports, you'll need symbols (.pdb files) for your application and also symbols for system libraries.</span></span> <span data-ttu-id="7c772-117">Vill du att du refererar till hello tillgängliga symbolen servrar toomake.</span><span class="sxs-lookup"><span data-stu-id="7c772-117">You'll want toomake sure that you reference hello available symbol servers.</span></span> <span data-ttu-id="7c772-118">toodo detta på hello **verktyg** -menyn i Visual Studio väljer **alternativ**, Välj **Debugging**, sedan **symboler**.</span><span class="sxs-lookup"><span data-stu-id="7c772-118">toodo this, on hello **Tools** menu in Visual Studio, choose **Options**, then choose **Debugging**, then **Symbols**.</span></span> <span data-ttu-id="7c772-119">Kontrollera att Microsoft Symbol-servrar visas **Symbol filplatser (.pdb)**.</span><span class="sxs-lookup"><span data-stu-id="7c772-119">Make sure that Microsoft Symbol Servers is listed under **Symbol file (.pdb) locations**.</span></span>  <span data-ttu-id="7c772-120">Du kan också referera http://referencesource.microsoft.com/symbols som kan ha ytterligare symbol-filer.</span><span class="sxs-lookup"><span data-stu-id="7c772-120">You can also reference http://referencesource.microsoft.com/symbols, which might have additional symbol files.</span></span>

![Symbolalternativ][4]

<span data-ttu-id="7c772-122">Om du vill kan du förenkla hello rapporterar att hello profiler genererar genom att ange bara min kod.</span><span class="sxs-lookup"><span data-stu-id="7c772-122">If desired, you can simplify hello reports that hello profiler generates by setting Just My Code.</span></span> <span data-ttu-id="7c772-123">Med bara min kod aktiverat, förenklas funktionen anropsstackar så som anropar helt interna toolibraries och hello .NET Framework är dolda från hello rapporter.</span><span class="sxs-lookup"><span data-stu-id="7c772-123">With Just My Code enabled, function call stacks are simplified so that calls entirely internal toolibraries and hello .NET Framework are hidden from hello reports.</span></span> <span data-ttu-id="7c772-124">På hello **verktyg** -menyn, Välj **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="7c772-124">On hello **Tools** menu, choose **Options**.</span></span> <span data-ttu-id="7c772-125">Expandera hello **prestandaverktyg** nod, och välj **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="7c772-125">Then expand hello **Performance Tools** node, and choose **General**.</span></span> <span data-ttu-id="7c772-126">Markera kryssrutan för hello för **aktivera bara min kod för profiler rapporter**.</span><span class="sxs-lookup"><span data-stu-id="7c772-126">Select hello checkbox for **Enable Just My Code for profiler reports**.</span></span>

![Bara min kod alternativ][17]

<span data-ttu-id="7c772-128">Du kan använda dessa instruktioner med ett befintligt projekt eller med ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="7c772-128">You can use these instructions with an existing project or with a new project.</span></span>  <span data-ttu-id="7c772-129">Om du skapar ett nytt projekt tootry hello tekniker som beskrivs nedan, Välj en C# **Azure Cloud Service** projektet och välj en **Webbroll** och en **Arbetsrollen**.</span><span class="sxs-lookup"><span data-stu-id="7c772-129">If you create a new project tootry hello techniques described below, choose a C# **Azure Cloud Service** project, and select a **Web Role** and a **Worker Role**.</span></span>

![Azure Cloud Service Projektroller][5]

<span data-ttu-id="7c772-131">Till exempel kan lägga till vissa Kodprojekt tooyour som tar mycket tid och visar vissa uppenbara prestandaproblem.</span><span class="sxs-lookup"><span data-stu-id="7c772-131">For example purposes, add some code tooyour project that takes a lot of time and demonstrates some obvious performance problem.</span></span> <span data-ttu-id="7c772-132">Till exempel lägga till följande kod tooa arbetsrollsprojektet hello:</span><span class="sxs-lookup"><span data-stu-id="7c772-132">For example, add hello following code tooa worker role project:</span></span>

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

<span data-ttu-id="7c772-133">Anropa den här koden från hello RunAsync metod i hello worker rollen RoleEntryPoint-härledd klass.</span><span class="sxs-lookup"><span data-stu-id="7c772-133">Call this code from hello RunAsync method in hello worker role's RoleEntryPoint-derived class.</span></span> <span data-ttu-id="7c772-134">(Ignorera hello varning om hello-metoden körs synkront.)</span><span class="sxs-lookup"><span data-stu-id="7c772-134">(Ignore hello warning about hello method running synchronously.)</span></span>

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace hello following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

<span data-ttu-id="7c772-135">Skapa och köra Molntjänsten lokalt utan felsökning (Ctrl + F5) med hello lösning konfigurationsuppsättning för**versionen**.</span><span class="sxs-lookup"><span data-stu-id="7c772-135">Build and run your cloud service locally without debugging (Ctrl+F5), with hello solution configuration set too**Release**.</span></span> <span data-ttu-id="7c772-136">Detta säkerställer att alla filer och mappar skapas för att köra programmet hello lokalt och säkerställer att alla hello emulatorerna har startats.</span><span class="sxs-lookup"><span data-stu-id="7c772-136">This ensures that all files and folders are created for running hello application locally, and ensures that all hello emulators are started.</span></span> <span data-ttu-id="7c772-137">Starta hello Compute Emulator UI från hello Aktivitetsfältet tooverify som kör arbetsrollen.</span><span class="sxs-lookup"><span data-stu-id="7c772-137">Start hello Compute Emulator UI from hello taskbar tooverify that your worker role is running.</span></span>

## <a name="2-attach-tooa-process"></a><span data-ttu-id="7c772-138">2: Anslut tooa process</span><span class="sxs-lookup"><span data-stu-id="7c772-138">2: Attach tooa process</span></span>
<span data-ttu-id="7c772-139">I stället för profilering hello programmet genom att starta från hello Visual Studio 2010 IDE, måste du koppla hello profileraren tooa körs.</span><span class="sxs-lookup"><span data-stu-id="7c772-139">Instead of profiling hello application by starting it from hello Visual Studio 2010 IDE, you must attach hello profiler tooa running process.</span></span> 

<span data-ttu-id="7c772-140">tooattach hello profileraren tooa processen på hello **analysera** -menyn, Välj **Profiler** och **Lägg till/ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7c772-140">tooattach hello profiler tooa process, on hello **Analyze** menu, choose **Profiler** and **Attach/Detach**.</span></span>

![Koppla profil][6]

<span data-ttu-id="7c772-142">Hitta hello WaWorkerHost.exe process för en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="7c772-142">For a worker role, find hello WaWorkerHost.exe process.</span></span>

![WaWorkerHost process][7]

<span data-ttu-id="7c772-144">Om din projektmapp finns på en nätverksenhet, ber hello profileraren dig tooprovide en annan plats toosave hello profilering rapporter.</span><span class="sxs-lookup"><span data-stu-id="7c772-144">If your project folder is on a network drive, hello profiler will ask you tooprovide another location toosave hello profiling reports.</span></span>

 <span data-ttu-id="7c772-145">Du kan också koppla tooa webbroll genom att koppla tooWaIISHost.exe.</span><span class="sxs-lookup"><span data-stu-id="7c772-145">You can also attach tooa web role by attaching tooWaIISHost.exe.</span></span>
<span data-ttu-id="7c772-146">Om det finns flera arbetsprocesser roll i ditt program, måste du toouse hello processID toodistinguish dem.</span><span class="sxs-lookup"><span data-stu-id="7c772-146">If there are multiple worker role processes in your application, you need toouse hello processID toodistinguish them.</span></span> <span data-ttu-id="7c772-147">Du kan fråga hello processID via programmering genom att öppna processobjektet för hello.</span><span class="sxs-lookup"><span data-stu-id="7c772-147">You can query hello processID programmatically by accessing hello Process object.</span></span> <span data-ttu-id="7c772-148">Till exempel om du lägger till den här koden toohello kör metoden hello RoleEntryPoint-härledd klass i en roll titta du på loggen i hello Compute Emulator UI tooknow vilka processen tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="7c772-148">For example, if you add this code toohello Run method of hello RoleEntryPoint-derived class in a role, you can look at the log in hello Compute Emulator UI tooknow what process tooconnect to.</span></span>

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

<span data-ttu-id="7c772-149">tooview hello logg, start hello Compute Emulator UI.</span><span class="sxs-lookup"><span data-stu-id="7c772-149">tooview hello log, start hello Compute Emulator UI.</span></span>

![Starta hello Compute Emulator UI][8]

<span data-ttu-id="7c772-151">Öppna hello worker-rollen loggen konsolfönstret i hello Compute Emulator UI genom att klicka på namnlisten i fönstret hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="7c772-151">Open hello worker role log console window in hello Compute Emulator UI by clicking on hello console window's title bar.</span></span> <span data-ttu-id="7c772-152">Du kan se hello process-ID i hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="7c772-152">You can see hello process ID in hello log.</span></span>

![Visa process-ID][9]

<span data-ttu-id="7c772-154">Ett du har kopplat utför hello steg i programmets användargränssnitt (vid behov) tooreproduce hello scenario.</span><span class="sxs-lookup"><span data-stu-id="7c772-154">One you've attached, perform hello steps in your application's UI (if needed) tooreproduce hello scenario.</span></span>

<span data-ttu-id="7c772-155">När du vill toostop profilering, Välj hello **stoppa profilering** länk.</span><span class="sxs-lookup"><span data-stu-id="7c772-155">When you want toostop profiling, choose hello **Stop Profiling** link.</span></span>

![Stoppa profilering alternativet][10]

## <a name="3-view-performance-reports"></a><span data-ttu-id="7c772-157">3: Visa prestandarapporter</span><span class="sxs-lookup"><span data-stu-id="7c772-157">3: View performance reports</span></span>
<span data-ttu-id="7c772-158">hello systemprestanda-rapport för ditt program visas.</span><span class="sxs-lookup"><span data-stu-id="7c772-158">hello performance report for your application is displayed.</span></span>

<span data-ttu-id="7c772-159">Nu hello profileraren stoppas utförandet, sparar data i en .vsp-fil och visar en rapport som visar en analys av data.</span><span class="sxs-lookup"><span data-stu-id="7c772-159">At this point, hello profiler stops executing, saves data in a .vsp file, and displays a report that shows an analysis of this data.</span></span>

![Profileraren rapport][11]

<span data-ttu-id="7c772-161">Om du ser String.wstrcpy i hello visa varm sökväg, klicka på bara min kod toochange hello tooshow användarkod endast.</span><span class="sxs-lookup"><span data-stu-id="7c772-161">If you see String.wstrcpy in hello Hot Path, click on Just My Code toochange hello view tooshow user code only.</span></span>  <span data-ttu-id="7c772-162">Om du ser String.Concat kan du prova att trycka på knappen för hello visar All kod.</span><span class="sxs-lookup"><span data-stu-id="7c772-162">If you see String.Concat, try pressing hello Show All Code button.</span></span>

<span data-ttu-id="7c772-163">Du bör se hello sammanfoga metoden och String.Concat tar upp en stor del av hello körningstid.</span><span class="sxs-lookup"><span data-stu-id="7c772-163">You should see hello Concatenate method and String.Concat taking up a large portion of hello execution time.</span></span>

![Analys av rapporten][12]

<span data-ttu-id="7c772-165">Om du har lagt till hello sträng sammanfogning koden i den här artikeln bör du se en varning i hello uppgiftslista för den här.</span><span class="sxs-lookup"><span data-stu-id="7c772-165">If you added hello string concatenation code in this article, you should see a warning in hello Task List for this.</span></span> <span data-ttu-id="7c772-166">Du kan också se en varning om att det finns en orimlig mängd skräpinsamling förfaller toohello antal strängar som skapas och tagits bort.</span><span class="sxs-lookup"><span data-stu-id="7c772-166">You may also see a warning that there is an excessive amount of garbage collection, which is due toohello number of strings that are created and disposed.</span></span>

![Prestanda varningar][14]

## <a name="4-make-changes-and-compare-performance"></a><span data-ttu-id="7c772-168">4: gör ändringar och jämföra prestanda</span><span class="sxs-lookup"><span data-stu-id="7c772-168">4: Make changes and compare performance</span></span>
<span data-ttu-id="7c772-169">Du kan också jämföra hello prestanda före och efter en kodändring.</span><span class="sxs-lookup"><span data-stu-id="7c772-169">You can also compare hello performance before and after a code change.</span></span>  <span data-ttu-id="7c772-170">Stoppa hello körs och redigera hello kod tooreplace hello sträng sammanfogning igen med hello används för StringBuilder:</span><span class="sxs-lookup"><span data-stu-id="7c772-170">Stop hello running process, and edit hello code tooreplace hello string concatenation operation with hello use of StringBuilder:</span></span>

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

<span data-ttu-id="7c772-171">Göra en annan prestanda kör och jämför hello prestanda.</span><span class="sxs-lookup"><span data-stu-id="7c772-171">Do another performance run, and then compare hello performance.</span></span> <span data-ttu-id="7c772-172">I hello prestanda Explorer, om hello körs finns i hello samma session, du kan bara markera båda rapporterna öppna hello snabbmenyn och välj **jämför Prestandarapporterna**.</span><span class="sxs-lookup"><span data-stu-id="7c772-172">In hello Performance Explorer, if hello runs are in hello same session, you can just select both reports, open hello shortcut menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="7c772-173">Om du vill toocompare med en körning i en annan prestanda session, öppna hello **analysera** -menyn och välj **jämför Prestandarapporterna**.</span><span class="sxs-lookup"><span data-stu-id="7c772-173">If you want toocompare with a run in another performance session, open hello **Analyze** menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="7c772-174">Ange båda filerna i hello dialogrutan som visas.</span><span class="sxs-lookup"><span data-stu-id="7c772-174">Specify both files in hello dialog box that appears.</span></span>

![Jämför prestanda rapporter alternativet][15]

<span data-ttu-id="7c772-176">hello rapporter Markera skillnaderna mellan två hello-körs.</span><span class="sxs-lookup"><span data-stu-id="7c772-176">hello reports highlight differences between hello two runs.</span></span>

![Jämförelserapport][16]

<span data-ttu-id="7c772-178">Grattis!</span><span class="sxs-lookup"><span data-stu-id="7c772-178">Congratulations!</span></span> <span data-ttu-id="7c772-179">Du har kommit igång med hello profiler.</span><span class="sxs-lookup"><span data-stu-id="7c772-179">You've gotten started with hello profiler.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7c772-180">Felsökning</span><span class="sxs-lookup"><span data-stu-id="7c772-180">Troubleshooting</span></span>
* <span data-ttu-id="7c772-181">Kontrollera att du profilering en slutversion och starta utan felsökning.</span><span class="sxs-lookup"><span data-stu-id="7c772-181">Make sure you are profiling a Release build and start without debugging.</span></span>
* <span data-ttu-id="7c772-182">Kör hello prestanda guiden om hello Lägg till/ta bort alternativet inte är aktiverad på hello Profiler-menyn.</span><span class="sxs-lookup"><span data-stu-id="7c772-182">If hello Attach/Detach option is not enabled on hello Profiler menu, run hello Performance Wizard.</span></span>
* <span data-ttu-id="7c772-183">Använd hello Compute Emulator UI tooview hello status för ditt program.</span><span class="sxs-lookup"><span data-stu-id="7c772-183">Use hello Compute Emulator UI tooview hello status of your application.</span></span> 
* <span data-ttu-id="7c772-184">Om du har problem med start av program i hello-emulatorn eller bifoga hello profiler, Stäng hello beräkningsemulatorn och starta om den.</span><span class="sxs-lookup"><span data-stu-id="7c772-184">If you have problems starting applications in hello emulator, or attaching hello profiler, shut down hello compute emulator and restart it.</span></span> <span data-ttu-id="7c772-185">Om det inte löser problemet hello, startar du om datorn.</span><span class="sxs-lookup"><span data-stu-id="7c772-185">If that doesn't solve hello problem, try rebooting.</span></span> <span data-ttu-id="7c772-186">Det här problemet kan inträffa om du använder hello Compute Emulator toosuspend och ta bort körs distributioner.</span><span class="sxs-lookup"><span data-stu-id="7c772-186">This problem can occur if you use hello Compute Emulator toosuspend and remove running deployments.</span></span>
* <span data-ttu-id="7c772-187">Om du har använt någon av hello profilering kommandon från kommandoraden, särskilt hello globala inställningar, se till att VSPerfClrEnv /globaloff har anropats och att VsPerfMon.exe har avslutats.</span><span class="sxs-lookup"><span data-stu-id="7c772-187">If you have used any of hello profiling commands from the command line, especially hello global settings, make sure that VSPerfClrEnv /globaloff has been called and that VsPerfMon.exe has been shut down.</span></span>
* <span data-ttu-id="7c772-188">Om när provtagning, hello meddelande ”PRF0025: inga data samlades in”, kontrollera att hello processen bifogade toohas CPU-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7c772-188">If when sampling, you see hello message "PRF0025: No data was collected," check that hello process you attached toohas CPU activity.</span></span> <span data-ttu-id="7c772-189">Program som inte gör några beräkningsarbete kan inte skapa några provtagning data.</span><span class="sxs-lookup"><span data-stu-id="7c772-189">Applications that are not doing any computational work might not produce any sampling data.</span></span>  <span data-ttu-id="7c772-190">Det är också möjligt att hello processen avslutades innan alla provtagning utfördes.</span><span class="sxs-lookup"><span data-stu-id="7c772-190">It's also possible that hello process exited before any sampling was done.</span></span> <span data-ttu-id="7c772-191">Kontrollera toosee som hello Run-metoden för en roll som du profilering inte avslutas.</span><span class="sxs-lookup"><span data-stu-id="7c772-191">Check toosee that hello Run method for a role that you are profiling does not terminate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c772-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c772-192">Next Steps</span></span>
<span data-ttu-id="7c772-193">Instrumentering Azure binärfiler i hello-emulatorn stöds inte i hello Visual Studio profiler, men om du vill tootest minnesallokering, kan du välja alternativet när profilering.</span><span class="sxs-lookup"><span data-stu-id="7c772-193">Instrumenting Azure binaries in hello emulator is not supported in hello Visual Studio profiler, but if you want tootest memory allocation, you can choose that option when profiling.</span></span> <span data-ttu-id="7c772-194">Du kan också välja samtidighet profilering, som hjälper dig att avgöra om trådar ha lagt tid konkurrerande för Lås eller tjänstnivån interaktion profilering, som hjälper dig att spåra prestandaproblem vid interaktion mellan nivåer för ett program, mest ofta mellan hello datanivå och en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="7c772-194">You can also choose concurrency profiling, which helps you determine whether threads are wasting time competing for locks, or tier interaction profiling, which helps you track down performance problems when interacting between tiers of an application, most frequently between hello data tier and a worker role.</span></span>  <span data-ttu-id="7c772-195">Du kan visa hello databasfrågor som genererar din app och använda hello profilering data tooimprove din användning av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="7c772-195">You can view hello database queries that your app generates and use hello profiling data tooimprove your use of hello database.</span></span> <span data-ttu-id="7c772-196">Information om nivån interaktion profilering läser du blogginlägget hello [genomgång: med hello nivå interaktion Profiler i Visual Studio Team System 2010][3].</span><span class="sxs-lookup"><span data-stu-id="7c772-196">For information about tier interaction profiling, see hello blog post [Walkthrough: Using hello Tier Interaction Profiler in Visual Studio Team System 2010][3].</span></span>

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
