1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com).  
2. Klicka på hello kommandofältet längst hello hello fönstret **ny**.
3. Under **Compute**, klickar du på **virtuella**, och klicka sedan på **från galleriet**.
   
    ![Skapa en ny virtuell dator][Image1]
4. Under hello **SUSE** gruppen, Välj en avbildning av virtuell dator OpenSUSE och klicka sedan på hello pilen toocontinue.
5. På hello första **konfiguration av virtuell dator** sidan:
   
   * Ange en **virtuellt datornamn**, till exempel ”testlinuxvm”. hello namn måste innehålla mellan 3 och 15 tecken, kan innehålla endast bokstäver, siffror och bindestreck, och måste börja med en bokstav och sluta med en bokstav eller siffra.
   * Kontrollera hello **nivå** och välj en **storlek**. hello nivå anger hello-storlekar som du kan välja bland. hello storlek påverkar hello kostnaden för att använda den, samt konfigurationsalternativen, t.ex hur många diskar du kan bifoga. Mer information finns i [storlekar för virtuella datorer](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Ange en **nytt användarnamn**, eller Godkänn hello standard **azureuser**. Det här namnet läggs toohello Sudoers fil.
   * Bestämmer vilka typer av **autentisering** toouse. För allmänna lösenordsriktlinjer finns [starka lösenord](http://msdn.microsoft.com/library/ms161962.aspx).
6. På hello nästa **konfiguration av virtuell dator** sidan:
   
   * Använd hello standard **skapa en ny molntjänst**.
   * I hello **DNS-namnet** Skriv ett unikt DNS-namnet toouse som en del av hello-adress, till exempel ”testlinuxvm”.
   * I hello **Region/tillhörighet grupp/virtuellt nätverk** väljer du en region där den här virtuella bilden kommer att finnas.
   * Under **slutpunkter**, hålla hello SSH-slutpunkten. Du kan lägga till andra nu, eller lägga till, ändra eller ta bort dem när hello virtuell dator skapas.
     
     > [!NOTE]
     > Om du vill att en virtuell dator toouse ett virtuellt nätverk du **måste** ange hello virtuellt nätverk när du skapar hello virtuell dator. Du kan inte lägga till ett virtuellt nätverk för virtuell dator-tooa när du skapar hello virtuell dator. Mer information finns i [översikt över virtuella nätverk](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. På hello senaste **konfiguration av virtuell dator** , hålla hello standardinställningarna och klickar sedan på hello markerat toofinish.

hello portal visar hello ny virtuell dator under **virtuella datorer**. Medan hello status rapporteras som **(allokering)**, hello virtuella ställs in. När hello status rapporteras som **kör**, kan du flytta på toohello nästa steg.

## <a name="connect-toohello-virtual-machine"></a>Ansluta toohello virtuell dator
Du använder SSH eller PuTTY tooconnect toohello virtuell dator, beroende på hello operativsystem på hello dator att ansluta från:

* Använd SSH från en dator som kör Linux. I hello kommandotolk, skriver du:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Ange hello användarens lösenord.
* Använd PuTTY från en dator som kör Windows. Om du inte har installerats kan du hämta det från hello [PuTTY-hämtningssida][PuTTYDownload].
  
    Spara **putty.exe** tooa katalog på din dator. Öppna en kommandotolk, navigera toothat mappen och kör **putty.exe**.
  
    Skriver hello värdnamnet, till exempel ”testlinuxvm.cloudapp.net” och ”22” för hello **Port**.
  
    ![PuTTY skärmen][Image6]  

## <a name="update-hello-virtual-machine-optional"></a>Uppdatera hello virtuella datorn (valfritt)
1. När du är ansluten toohello virtuell dator kan du installera uppdateringar och korrigeringsfiler. toorun hello uppdatering, typ:
   
    `$ sudo zypper update`
2. Välj **programvara**, sedan **Online-uppdatering** toolist tillgängliga uppdateringar. Välj **acceptera** toostart hello installationen och tillämpa alla nya tillgängliga korrigeringar (utom hello valfria som).
3. När installationen är klar väljer du **Slutför**.  Systemet är nu upp toodate.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
