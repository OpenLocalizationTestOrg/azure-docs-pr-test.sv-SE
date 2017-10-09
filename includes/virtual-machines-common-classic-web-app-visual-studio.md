

När du skapar ett webbprojekt program för Azure kan etablera du en virtuell dator i Azure. Du kan sedan konfigurera hello virtuell dator med ytterligare programvara eller använda hello virtuell dator för diagnostik och felsökning.

toocreate en virtuell dator när du skapar ett webbprogram, så här:

1. I Visual Studio klickar du på **filen** > **ny** > **projekt** > **Web**, och välj sedan **ASP.NET-webbprogram** (under hello **Visual C#** eller **Visual Basic** noder).
2. I hello **nytt ASP.NET-projekt** dialogrutan, Välj hello typ av webbprogram som du vill och i hello Azure avsnittet hello dialogrutan (i hello längst ned till höger), se till att hello **värd i molnet hello**är markerad (den här kryssrutan **skapa fjärresurser** i vissa installationer).
   
    ![][0]
3. Det här exemplet i hello listrutan under Microsoft Azure, Välj **virtuell dator (v1)**, och klicka sedan på hello **OK** knappen.
4. Logga in tooAzure om du uppmanas. Hej **Skapa virtuell dator** dialogrutan visas.
   
    ![][2]
5. I hello **DNS-namnet** ange ett namn för hello virtuella datorn. hello DNS-namn måste vara unikt i Azure. Om hello namnet du angav är inte tillgänglig, visas ett rött utropstecken.
6. I hello **bild** Välj hello bilden som du vill toobase hello virtuella datorn på. Du kan välja någon av hello Azure-dator bilder eller att du har överfört tooAzure avbildningen.
7. Lämna hello **aktivera IIS och Web Deploy** Markera kryssrutan om du planerar tooinstall en annan webbserver. Du kommer inte att kunna toopublish från Visual Studio om du inaktiverar webbdistribution. Du kan lägga till IIS och Web Deploy tooany av hello paketeras Windows Server-avbildningar, inklusive dina egna anpassade avbildningar.
8. I hello **storlek** Välj hello storleken på hello virtuella datorn.
9. Ange hello inloggningsuppgifter för den virtuella datorn. Anteckna dem, eftersom du behöver dem tooaccess hello datorn via fjärrskrivbord.
10. I hello **plats** Välj hello region toohost hello virtuell dator.
11. Klicka på hello **OK** knappen toostart skapar hello virtuell dator. Du kan följa hello åtgärdens i hello hello förlopp **utdata** fönster.
    
    ![][3]
12. När hello virtuella datorn har etablerats publicerade skript skapas i en **PublishScripts** nod i din lösning. hello publicerade körs skriptet och etablerar en virtuell dator i Azure. Hej **utdata** fönster visar hello status. hello skriptet utför hello följande åtgärder tooset hello virtuella datorn:
    
    * Skapar hello virtuella datorn om den inte redan finns.
    * Skapar ett lagringskonto med ett namn som börjar med `devtest`, men endast om det inte redan finns ett lagringskonto i regionen hello.
    * Skapar en molnbaserad tjänst som en behållare för hello virtuella datorn och skapar en webbroll för hello webbprogrammet.
    * Konfigurerar webbdistribution på hello virtuell dator.
    * Konfigurerar IIS och ASP.NET i hello virtuell dator.
    
    ![][4]
13. (Valfritt) Du kan ansluta toohello ny virtuell dator. I **Server Explorer**, expandera hello **virtuella datorer** noden, Välj hello noden för hello virtuell dator som du skapade och dess snabbmenyn, Välj **Anslut med fjärrskrivbord**. Du kan också i **Cloud Explorer** du **öppna i portalen** hello snabbmenyn och ansluta det virtuella för toohello.
    
    ![][5]

## <a name="next-steps"></a>Nästa steg
Om du vill toocustomize hello publicerade skript som du skapat kan läsa mer detaljerad information på [med hjälp av Windows PowerShell-skript tooPublish tooDev och testmiljöer](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
