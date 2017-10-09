<span data-ttu-id="95ee2-101">hello steg toounregister en processerver skiljer sig beroende på dess anslutningsstatus med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="95ee2-101">hello steps toounregister a process server differs depending on its connection status with hello Configuration Server.</span></span>

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a><span data-ttu-id="95ee2-102">Avregistrera en processerver som är i kopplat läge</span><span class="sxs-lookup"><span data-stu-id="95ee2-102">Unregister a process server that is in a connected state</span></span>

1. <span data-ttu-id="95ee2-103">Fjärråtkomst till hello processervern som administratör.</span><span class="sxs-lookup"><span data-stu-id="95ee2-103">Remote into hello process server as an Administrator.</span></span>
2. <span data-ttu-id="95ee2-104">Starta hello **Kontrollpanelen** och öppna **program > Avinstallera ett program**</span><span class="sxs-lookup"><span data-stu-id="95ee2-104">Launch hello **Control Panel** and open **Programs > Uninstall a program**</span></span>
3. <span data-ttu-id="95ee2-105">Avinstallera ett program med namnet hello **Microsoft Azure Site Recovery konfigurationsprocessen/Server**</span><span class="sxs-lookup"><span data-stu-id="95ee2-105">Uninstall a program by hello name **Microsoft Azure Site Recovery Configuration/Process Server**</span></span>
4. <span data-ttu-id="95ee2-106">När steg 3 är slutfört kan du avinstallera **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** (Beroenden för Microsoft Azure Site Recovery konfigurations-/processerver)</span><span class="sxs-lookup"><span data-stu-id="95ee2-106">Once step 3 is completed, you can uninstall **Microsoft Azure Site Recovery Configuration/Process Server Dependencies**</span></span>

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a><span data-ttu-id="95ee2-107">Avregistrera en processerver som är i frånkopplat läge</span><span class="sxs-lookup"><span data-stu-id="95ee2-107">Unregister a process server that is in a disconnected state</span></span>

> [!WARNING]
> <span data-ttu-id="95ee2-108">Använd hello nedanstående steg ska användas om det finns inget sätt toorevive hello virtuell dator på vilken hello Processervern har installerats.</span><span class="sxs-lookup"><span data-stu-id="95ee2-108">Use hello below steps should be used if there is no way toorevive hello virtual machine on which hello Process Server was installed.</span></span>

1. <span data-ttu-id="95ee2-109">Logga in tooyour konfigurationsservern som administratör.</span><span class="sxs-lookup"><span data-stu-id="95ee2-109">Log on tooyour configuration server as an Administrator.</span></span>
2. <span data-ttu-id="95ee2-110">Öppna en administrativ kommandotolk och gå toohello directory `%ProgramData%\ASR\home\svsystems\bin`.</span><span class="sxs-lookup"><span data-stu-id="95ee2-110">Open an Administrative command prompt and browse toohello directory `%ProgramData%\ASR\home\svsystems\bin`.</span></span>
3. <span data-ttu-id="95ee2-111">Kör nu hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="95ee2-111">Now run hello command.</span></span>

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. <span data-ttu-id="95ee2-112">Hello-information för hello processerver rensas från hello system.</span><span class="sxs-lookup"><span data-stu-id="95ee2-112">This will purge hello details of hello process server from hello system.</span></span>
