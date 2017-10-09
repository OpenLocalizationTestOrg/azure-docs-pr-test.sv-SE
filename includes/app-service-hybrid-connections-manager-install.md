
1. <span data-ttu-id="6c8aa-101">I hello **hybridanslutningar** bladet hello hybridanslutning som du precis skapade Klicka **lyssnare installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-101">In hello **Hybrid connections** blade, click hello hybrid connection you just created, then click **Listener Setup**.</span></span>
   
    ![Klicka på lyssnaren installationsprogrammet](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. <span data-ttu-id="6c8aa-103">Hej **Hybrid anslutningsegenskaper** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-103">hello **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="6c8aa-104">Under **på lokal Hybridanslutningshanterare**, Välj **hämta och konfigurera manuellt**, sparar hello hämtas HybridConnectionManager.msi paketet och kopiera anslutningssträngen för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-104">Under **On-premises Hybrid Connection Manager**, choose **download and configure manually**, save hello downloaded HybridConnectionManager.msi package, and copy hello gateway connection string.</span></span>
   
    ![Klicka på tooinstall](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. <span data-ttu-id="6c8aa-106">Från en administratörskommandotolk skriver du följande kommando toostart hello installer hello:</span><span class="sxs-lookup"><span data-stu-id="6c8aa-106">From an administrator command prompt, type hello following command toostart hello installer:</span></span>
   
        start HybridConnectionManager.msi
4. <span data-ttu-id="6c8aa-107">Efter hello installationsprogrammet körs, klickar du på **inte**, bläddra sedan toohello %ProgramFiles%\Microsoft\HybridConnectionManager mappen kör HCMConfigWizard.exe och klickar på **Ja** i hello **användare Kontroll** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-107">After hello installer runs, click **Not now**, then browse toohello %ProgramFiles%\Microsoft\HybridConnectionManager folder, run HCMConfigWizard.exe and click **Yes** in hello **User Account Control** dialog.</span></span>
5. <span data-ttu-id="6c8aa-108">Klistra in hello hybrid-anslutningssträng som du kopierade tidigare och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-108">Paste hello hybrid connection string that you copied earlier and click **OK**.</span></span> 
   
    ![Installera](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. <span data-ttu-id="6c8aa-110">När hello-installationen har slutförts klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-110">When hello install completes, click **Close**.</span></span>
   
    ![Klicka på Stäng](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    <span data-ttu-id="6c8aa-112">På hello **hybridanslutningar** bladet, hello **Status** kolumnen visar nu **ansluten**.</span><span class="sxs-lookup"><span data-stu-id="6c8aa-112">On hello **Hybrid connections** blade, hello **Status** column now shows **Connected**.</span></span> 
   
    ![Anslutna Status](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

