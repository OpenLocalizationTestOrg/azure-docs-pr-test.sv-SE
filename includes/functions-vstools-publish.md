1. I **Solution Explorer**, högerklicka på hello-projektet och välj **publicera**. Välj **Skapa ny** och klicka sedan på **Publicera**. 

    ![Skapa och publicera ny funktionsapp](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

2. Om du inte har redan anslutit Visual Studio tooyour Azure-konto, klickar du på **Lägg till ett konto...** .  

3. I hello **skapa Apptjänst** dialogrutan, Använd hello **värd** inställningar som anges i hello följande tabell: 

    ![Lokal Azure-körningsmiljö](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Inställning      | Föreslaget värde  | Beskrivning                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Appnamn** | Globalt unikt namn | Namn som unikt identifierar din nya funktionsapp. |
    | **Prenumeration** | Välj din prenumeration | hello Azure-prenumeration toouse. |
    | **[Resursgrupp](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Namnet på resursen hello gruppera i vilka toocreate funktionen appen. |
    | **[App Service-plan](../articles/azure-functions/functions-scale.md)** | Förbrukningsplan | Se till att toochoose hello **förbrukning** under **storlek** när du skapar en ny plan.  |
    | **[Lagringskonto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** | Globalt unikt namn | Använd ett befintligt lagringskonto eller skapa ett nytt.   |

4. Klicka på **skapa** toocreate en funktionsapp i Azure med de här inställningarna. När hello etableringen har slutförts, anteckna hello **Webbadress** -värde som är hello-adressen för din funktionsapp i Azure. 

    ![Lokal Azure-körningsmiljö](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
