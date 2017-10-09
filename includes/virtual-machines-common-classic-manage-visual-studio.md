Du kan skapa virtuella datorer i Azure med hjälp av Server Explorer i Visual Studio.

## <a name="create-an-azure-virtual-machine-in-server-explorer"></a>Skapa en virtuell Azure-dator i Server Explorer
Du kan skapa en virtuell dator i hello [Azure-hanteringsportalen](http://go.microsoft.com/fwlink/?LinkID=253103), du kan också skapa en virtuell dator i Azure med hjälp av kommandon i Server Explorer. Virtuella datorer kan användas, till exempel tooprovide ett främre sista bakom en gemensam offentlig belastningsutjämnade-slutpunkt.

### <a name="toocreate-a-new-virtual-machine"></a>toocreate en ny virtuell dator
1. I Server Explorer öppnar du hello **Azure** och klickar på **virtuella datorer**.
2. Klicka på hello snabbmenyn **Skapa virtuell dator**.
   
    Hej **skapa en ny virtuell dator** guiden visas.
   
    ![hello kommandot Skapa virtuell dator](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718342.png)
3. På hello **Välj en prenumeration** , välja en prenumeration toouse när du skapar hello virtuella datorn och klickar sedan på **nästa**.
   
    Om du inte är inloggad tooAzure klickar du på **logga In** toosign i. Markera din Azure-prenumeration i listrutan hello om den inte redan är markerad.
4. På hello **markerar du en avbildning av virtuell dator** väljer du en bildtyp i hello **bildtypen** nedrullningsbar listruta och välj sedan en avbildningar av virtuella datorer i hello **avbildningsnamn** listrutan. När du är klar klickar du på **nästa**.
   
    ![Välj en virtuell dator sida](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744137.png)
   
    Du kan välja följande bildtyper hello.
   
   * **Offentliga bilder** visar virtuella avbildningar av operativsystem och serverprogram, till exempel Windows Server och SQL Server.
   * **MSDN-bilder** visar virtuella avbildningar av program tillgängliga tooMSDN prenumeranter, till exempel Visual Studio och Microsoft Dynamics.
   * **Privat bilder** listor specialiserade och generaliserad avbildningar av virtuella datorer som du har skapat.
     
     toolearn om specialiserade och generaliserade virtuella datorer, se [VM-avbildning](https://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/). Se [hur tooCapture en virtuell Windows-dator tooUse som en mall](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) information om hur tooturn som en virtuell dator till en mall som du kan använda tooquickly skapa nya förkonfigurerade virtuella datorer.
     
     Du kan klicka på en virtuell dator avbildningen namn toosee information om hello avbildningen hello höger hello-sidan.
     
     > [!NOTE]
     > Du kan inte lägga till virtuella avbildningar toohello **offentliga bilder** eller **MSDN-bilder** visas eftersom de är skrivskyddad. Alla virtuella datorer som du skapar läggs toohello **privata bilder** lista.
     > 
     > 
     
     Om du är en MSDN-prenumerant med en prenumeration på Visual Studio-nivån, kan du skapa en förskapad Azure virtuell dator som innehåller Visual Studio, samt andra bilder. Mer information finns i [skapar en virtuell dator i Visual Studio av avbildningen med hjälp av avbildningar Visual Studio 2013-galleriet för MSDN-prenumeranter](http://visualstudio2013msdngalleryimage.azurewebsites.net) och [MSDN prenumerationer](https://www.visualstudio.com/products/msdn-subscriptions-vs). |
5. På hello **grundläggande inställningar för virtuell dator** anger du ett namn för datorn och Lägg sedan till hello specifikationer för hello virtuella datorn, inklusive hello storlek, och ett användarnamn och lösenord. När du är klar klickar du på **nästa**.
   
    Du ska använda hello nytt namn och lösenord toolog till hello-dator med hjälp av fjärrskrivbord, så det är en bra idé toowrite dem ned om du glömmer bort. När du har skapat en virtuell Azure-dator i Visual Studio kan du ändra dess storlek och andra inställningar i hello [Azure-hanteringsportalen](http://go.microsoft.com/fwlink/?LinkID=253103).
   
   > [!NOTE]
   > Om du väljer större storlekar för virtuella hello kan extra avgifter tillkomma. Se [prisinformation för virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/) för mer information.
   > 
   > 
6. Virtuella datorer som skapats i Visual Studio kräver en tjänst i molnet. På hello **moln tjänstinställningar** väljer du en tjänst i molnet för hello virtuella datorn, eller klicka på **< Skapa nytt >** i hello nedrullningsbara listan om du inte redan har ett moln tjänst eller vill toouse en ny en. Ett lagringskonto krävs också, så väljer något lagringskonto (eller skapa ett nytt lagringskonto) i hello **lagringskonto** listrutan.. Se [introduktion tooMicrosoft Azure Storage](../articles/storage/common/storage-introduction.md) för mer information.
7. Om du vill toospecify ett virtuellt nätverk (som är valfri), markerar du den i hello virtuellt nätverk och undernät dropdown listrutor.
   
    Virtuella datorer som är medlemmar i en tillgänglighetsuppsättning är distribuerade toodifferent feldomäner. Se [Azure Virtual Network](https://azure.microsoft.com/services/virtual-network/) för mer information.
8. Om du vill att din virtuella toobelong tooan tillgänglighetsuppsättning (även valfritt) Välj hello **ange en tillgänglighetsuppsättning** kryssrutan och välj sedan en tillgänglighetsuppsättning i listrutan hello.. När du är klar väljer du hello **nästa** knappen.
   
    Lägga till din virtuella tooan hjälper tillgänglighetsuppsättning ditt program vara tillgängliga under nätverksfel, lokal disk maskinvarufel och planerade driftavbrott. Du behöver toouse hello [Azure-hanteringsportalen](http://go.microsoft.com/fwlink/?LinkID=253103) anger toocreate virtuella nätverk, undernät och tillgänglighet. Se [hantera hello tillgängligheten för Virtual Machines](https://azure.microsoft.com/documentation/articles/manage-availability-virtual-machines/) för mer information.
9. På hello **slutpunkter** anger hello offentliga slutpunkter som du vill tillgängliga toousers i den virtuella datorn. Exempelvis kan du tooenable HTTP (Port 80) dessutom toohello fjärrskrivbord och PowerShell slutpunkter, som är aktiverade som standard. tooadd en slutpunkt och välja det i hello **portnamn** nedrullningsbar listruta och välj sedan hello **Lägg till** knappen. tooremove en slutpunkt, Välj hello red **X** nästa toohello namn i listan för hello-slutpunkter.
   
    ![hello slutpunkter sida i guiden för hello virtuella datorer.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718351.png)
   
    hello-slutpunkter som är tillgängliga beror på hello molntjänst som du valt för den virtuella datorn. Se [Azure Tjänsteslutpunkter](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) för mer information.
   
   > [!NOTE]
   > Aktivera offentliga slutpunkter gör tjänster på den virtuella datorn tillgänglig toohello internet. Vara säker på att tooinstall och konfigurera hello slutpunkter och tjänster på den virtuella datorn som inställningen åtkomstkontrollistor (ACL) för hello-slutpunkter. Se [hur tooSet in slutpunkter tooa virtuella](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) för mer information.
   > 
   > 
10. När du är klar att konfigurera inställningar för virtuell dator för hello väljer hello **skapa** knappen toocreate hello virtuell dator.
    
     Eftersom Azure skapar hello virtuell dator, hello **Azure-aktivitetsloggen** visar hello hello virtuell dator skapas åtgärdens förlopp.
    
     ![Virtuella aktivitetsloggen - pågår.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744138.png)
    
     tooview virtuell dator med enbart information, Välj hello **virtuella datorer** fliken i hello **Azure-aktivitetsloggen**.
    
     ![Virtuella aktivitetsloggen - slutfördes.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744139.png)
    
     Om hello åtgärden slutförs hello ny virtuell dator visas under hello **virtuella datorer** nod i Server Explorer. Du kan logga in på den genom att klicka på hello **ansluta med hjälp av fjärrskrivbord** genväg.
    
     ![Den virtuella datorn som visas i Server Explorer.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744140.png)

## <a name="manage-your-virtual-machines"></a>Hantera dina virtuella datorer
På konfigurationssidan för hello virtuella dessutom tooshutting ned, ansluter, uppdatera och lägga till kontrollpunkter toohello valda virtuella datorn, du kan också visa eller ändra inställningar för hello virtuell dator. Du kan:

* Ändra storlek på hello virtuell dator.
* Välj hello tillgänglighetsuppsättning toouse med hello virtuell dator.
* Lägga till, ta bort eller ändra inställningar för offentliga slutpunkter.
* Lägga till, ta bort eller konfigurera tillägg för virtuell dator.
* Visa information om hello diskar som är associerade med hello virtuell dator.

### <a name="view-or-change-virtual-machine-settings"></a>Visa eller ändra inställningar för virtuell dator
1. I Server Explorer, väljer du den virtuella datorn i hello **Azure Virtual Machines** nod.
2. På snabbmenyn hello väljer **konfigurera** konfigurationssidan för tooview hello virtuell dator.
   
    ![konfigurationssidan för hello Azure-dator](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744141.png)
3. Visa information om hello virtuella datorn eller ändra den.

### <a name="save-or-restore-hello-status-of-your-virtual-machine"></a>Spara eller återställa hello status för den virtuella datorn
När du konfigurera den virtuella datorn och installera programvara på den är en bra idé tooregularly spara ditt arbete genom att skapa kontrollpunkter för virtuell dator. En kontrollpunkt är en ögonblicksbild eller en bild av hello aktuell status för den virtuella datorn. Om något går fel med hello virtuell dator eller om du vill tooreconfigure hello virtuella datorn, kan du spara tid genom att återställa den tooa föregående kontrollpunkt tillstånd i stället för att börja om från början.

### <a name="toocreate-a-virtual-machine-checkpoint"></a>toocreate en kontrollpunkt för virtuell dator
1. I Server Explorer, väljer du den virtuella datorn i hello **Azure Virtual Machines** nod.
2. På snabbmenyn hello väljer **konfigurera** konfigurationssidan för tooview hello virtuell dator.
3. På sidan för konfiguration av hello väljer hello **Skapa avbildning** knappen.
   
    ![Konfigurationen av Azure sidan klipp ut](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744142.png)
   
    Hej **avbilda virtuella** visas.
   
    ![Dialogrutan för Azure avbilda virtuell dator](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744143.png)
4. Ange en avbildningsetiketten och en beskrivning. En standardetikett och en beskrivning finns, men du kan skriva över dem med din egen om du vill.
5. Om du redan har kört Sysprep på den virtuella datorn, väljer hello **jag har kört Sysprep på den virtuella datorn hello** rutan.
   
    Sysprep är ett verktyg som bland annat tar bort system-specifika data från hello virtuella datorns version av Windows, vilket gör den mall som andra kan använda. Se [hur tooCapture en virtuell Windows-dator tooUse som en mall](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) för mer information. Säkerhetskopiera hello VM innan du kör Sysprep.
6. När du är klar att konfigurera hello hämtningsinställningar väljer hello **avbilda** knappen toocreate hello kontrollpunkt.
   
    Eftersom Azure skapar hello kontrollpunkt, hello **Azure-aktivitetsloggen** visar hello hello åtgärdens förlopp.
   
    ![Skapa en kontrollpunkt för virtuell dator](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744144.png)
   
    När hello kontrollpunktsåtgärden är klar visas den i hello **Azure-aktivitetsloggen**.
   
    ![CheckPoint-åtgärden slutfördes](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744145.png)

## <a name="toomanage-virtual-machine-checkpoints"></a>toomanage kontrollpunkter för virtuell dator
### <a name="toorestore-a-virtual-machine-tooa-previously-saved-state"></a>toorestore en virtuell dator tooa tidigare sparade tillstånd
* Följ stegen i hello [stegvisa: utföra molnet återställer av Microsoft Azure virtuella datorer med hjälp av PowerShell - del 2](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx).

### <a name="toodelete-a-checkpoint"></a>toodelete en kontrollpunkt
1. Gå toohello [Azure-hanteringsportalen](http://go.microsoft.com/fwlink/?LinkID=253103).
2. På konfigurationssidan för hello virtuella väljer hello **bilder** fliken hello överst på hello sidan.
3. Välj hello kontrollpunkt toodelete, och välj sedan hello **ta bort** knappen på hello hello sidans nederkant.

## <a name="shut-down-your-virtual-machine"></a>Stäng av den virtuella datorn
1. I Server Explorer Välj hello virtuell dator som du vill tooshut ned i hello **Azure Virtual Machines** nod.
2. Hello snabbmenyn väljer du antingen hello **avstängning** kommandot, eller välj **konfigurera** tooview hello sidan för konfiguration av virtuell dator och välj sedan hello **avstängning**knappen.

## <a name="next-steps"></a>Nästa steg
toolearn mer om hur du skapar virtuella datorer, se [skapa en virtuell dator kör Linux](../articles/virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [skapa en virtuell dator som kör Windows i hello Azure preview portal](../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

