
1. I hello **hybridanslutningar** bladet hello hybridanslutning som du precis skapade Klicka **lyssnare installationsprogrammet**.
   
    ![Klicka på lyssnaren installationsprogrammet](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. Hej **Hybrid anslutningsegenskaper** blad öppnas. Under **på lokal Hybridanslutningshanterare**, Välj **hämta och konfigurera manuellt**, sparar hello hämtas HybridConnectionManager.msi paketet och kopiera anslutningssträngen för hello gateway.
   
    ![Klicka på tooinstall](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Från en administratörskommandotolk skriver du följande kommando toostart hello installer hello:
   
        start HybridConnectionManager.msi
4. Efter hello installationsprogrammet körs, klickar du på **inte**, bläddra sedan toohello %ProgramFiles%\Microsoft\HybridConnectionManager mappen kör HCMConfigWizard.exe och klickar på **Ja** i hello **användare Kontroll** dialogrutan.
5. Klistra in hello hybrid-anslutningssträng som du kopierade tidigare och klicka på **OK**. 
   
    ![Installera](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. När hello-installationen har slutförts klickar du på **Stäng**.
   
    ![Klicka på Stäng](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    På hello **hybridanslutningar** bladet, hello **Status** kolumnen visar nu **ansluten**. 
   
    ![Anslutna Status](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

