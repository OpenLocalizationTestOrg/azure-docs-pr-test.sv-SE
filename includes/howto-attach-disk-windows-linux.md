


## <a name="attach-an-empty-disk"></a>Ansluta en tom disk
Bifoga en tom disk är ett enkelt sätt tooadd en data-disken, eftersom Azure skapas hello VHD-fil och lagrar den i hello storage-konto.

1. Klicka på **virtuella datorer (klassisk)**, och sedan väljer hello lämpliga VM.

2. Klicka på menyn Inställningar hello **diskar**.

   ![Koppla en ny tom disk](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Klicka på hello kommandofältet **bifoga nya**.  
    Hej **bifoga den nya disken** dialogrutan visas.

    ![Koppla en ny disk](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Fyll i hello följande information:
    - I **filnamn**accepterar hello standardnamnet eller ange ett annat namn för hello VHD-filen. Hej datadisk använder ett automatiskt genererat namn, även om du anger ett annat namn för hello VHD-filen.
    - Välj hello **typen** av hello datadisk. Alla virtuella datorer stöder standarddiskar. Många virtuella datorer har också stöd för premiumdiskar.
    - Välj hello **storlek (GB)** av hello datadisk.
    - För **Värdcachelagring**, väljer du ingen eller skrivskyddad.
    - Klicka på OK toofinish.

4. När hello datadisk skapas och bifogas, visas den under hello diskar i hello VM.

   ![Nya och tom disk har kopplats](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> När du lägger till en datadisk, du behöver toolog på toohello VM och initierar hello disk så att den kan användas.

## <a name="how-to-attach-an-existing-disk"></a>Så här: bifoga en befintlig disk
För att kunna ansluta en befintlig disk måste du ha en VHD-fil tillgänglig i ett lagringskonto. Använd hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet tooupload hello VHD-filen toohello storage-konto. När du har skapat och överföra hello VHD-filen, kan du koppla den tooa VM.

1. Klicka på **virtuella datorer (klassisk)**, och sedan väljer hello lämplig virtuell dator.

2. Klicka på menyn Inställningar hello **diskar**.

3. Klicka på hello kommandofältet **bifoga befintliga**.

    ![Anslut en datadisk](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Klicka på **plats**. Visa hello tillgängliga storage-konton. Välj sedan ett lämpligt storage-konto från dem i listan.

    ![Ange disk storage-konto](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. En **lagringskonto** innehåller en eller flera behållare som innehåller hårddiskar (VHD). Välj lämplig hello-behållare från dem i listan.

    ![Ange behållare för virtuella datorer windows](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. Hej **virtuella hårddiskar** panelen visas hello diskenheter som lagras i hello behållare. Klicka på en av diskarna hello och klicka på Välj.

    ![Ange diskavbildning för virtuella datorer windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. Hej **bifoga den befintliga disken** panelen visas igen med hello-plats som innehåller hello storage-konto, behållare och valda hårddisk (vhd) tooadd toohello virtuell dator.

  Ange **Värdcachelagring** toonone eller Läs bara, klicka på OK.

    ![Datadisk har kopplats](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
