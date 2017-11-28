<span data-ttu-id="b28e1-101">Anvisningarna för att avregistrera en processerver är olika beroende på dess anslutningsstatus till konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="b28e1-101">The steps to unregister a process server differs depending on its connection status with the Configuration Server.</span></span>

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a><span data-ttu-id="b28e1-102">Avregistrera en processerver som är i kopplat läge</span><span class="sxs-lookup"><span data-stu-id="b28e1-102">Unregister a process server that is in a connected state</span></span>

1. <span data-ttu-id="b28e1-103">Fjärranslut till processervern som administratör.</span><span class="sxs-lookup"><span data-stu-id="b28e1-103">Remote into the process server as an Administrator.</span></span>
2. <span data-ttu-id="b28e1-104">Starta **Kontrollpanelen** och öppna **Program > Avinstallera ett program**</span><span class="sxs-lookup"><span data-stu-id="b28e1-104">Launch the **Control Panel** and open **Programs > Uninstall a program**</span></span>
3. <span data-ttu-id="b28e1-105">Avinstallera ett program med namnet **Microsoft Azure Site Recovery Configuration/Process Server** (Microsoft Azure Site Recovery konfigurations-/processerver)</span><span class="sxs-lookup"><span data-stu-id="b28e1-105">Uninstall a program by the name **Microsoft Azure Site Recovery Configuration/Process Server**</span></span>
4. <span data-ttu-id="b28e1-106">När steg 3 är slutfört kan du avinstallera **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** (Beroenden för Microsoft Azure Site Recovery konfigurations-/processerver)</span><span class="sxs-lookup"><span data-stu-id="b28e1-106">Once step 3 is completed, you can uninstall **Microsoft Azure Site Recovery Configuration/Process Server Dependencies**</span></span>

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a><span data-ttu-id="b28e1-107">Avregistrera en processerver som är i frånkopplat läge</span><span class="sxs-lookup"><span data-stu-id="b28e1-107">Unregister a process server that is in a disconnected state</span></span>

> [!WARNING]
> <span data-ttu-id="b28e1-108">Stegen nedan ska användas om det inte finns något sätt att återskapa den virtuella datorn som processervern installerades på.</span><span class="sxs-lookup"><span data-stu-id="b28e1-108">Use the below steps should be used if there is no way to revive the virtual machine on which the Process Server was installed.</span></span>

1. <span data-ttu-id="b28e1-109">Logga in på din konfigurationsserver som administratör.</span><span class="sxs-lookup"><span data-stu-id="b28e1-109">Log on to your configuration server as an Administrator.</span></span>
2. <span data-ttu-id="b28e1-110">Öppna en administrativ kommandotolk och gå till katalogen `%ProgramData%\ASR\home\svsystems\bin`.</span><span class="sxs-lookup"><span data-stu-id="b28e1-110">Open an Administrative command prompt and browse to the directory `%ProgramData%\ASR\home\svsystems\bin`.</span></span>
3. <span data-ttu-id="b28e1-111">Kör kommandot nu.</span><span class="sxs-lookup"><span data-stu-id="b28e1-111">Now run the command.</span></span>

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. <span data-ttu-id="b28e1-112">Information om processervern rensas från systemet.</span><span class="sxs-lookup"><span data-stu-id="b28e1-112">This will purge the details of the process server from the system.</span></span>
