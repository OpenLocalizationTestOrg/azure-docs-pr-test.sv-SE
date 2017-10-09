När du inte längre behöver en datadisk som är bifogade tooa virtuell dator kan du enkelt kan koppla från den. Kopplar bort en disk tar bort hello disk från hello virtuell dator, men ta bort inte hello disken från hello Azure storage-konto.

Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.  

> [!NOTE]
> toodetach en operativsystemdisk, måste du först toodelete hello virtuell dator.
>

## <a name="find-hello-disk"></a>Hitta hello disk
Om du inte vet hello namnet på hello disk eller vill tooverify den innan du koppla från den, Följ dessa steg.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Klicka på **virtuella datorer**, och sedan väljer hello lämpliga VM.

3. Klicka på **diskar** längs hello vänster kant hello virtuella instrumentpanelen under **inställningar**.

 hello virtuella instrumentpanelen visar hello namn och typ för alla anslutna diskar. Den här skärmen visar till exempel en virtuell dator med en operativsystemdisk och en datadisk:

    ![Hitta datadisk](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-hello-disk"></a>Koppla bort hello disk
1. Hello Azure-portalen, klicka på **virtuella datorer**, och klicka sedan på hello namnet på hello virtuell dator som har hello datadisk du vill toodetach.

2. Klicka på **diskar** längs hello vänster kant hello virtuella instrumentpanelen under **inställningar**.

3. Klicka på hello disk du vill toodetach.

  ![Identifiera hello disk toodetach](./media/howto-detach-disk-windows-linux/disklist.png)

4. Hello kommandofältet klickar du på **Detach**.

  ![Leta upp hello frånkoppling kommando](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. I fönstret för bekräftelse av hello klickar du på **Ja** toodetach hello disk.

  ![Bekräfta kopplar bort hello-disk](./media/howto-detach-disk-windows-linux/confirmdetach.png)

hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.
