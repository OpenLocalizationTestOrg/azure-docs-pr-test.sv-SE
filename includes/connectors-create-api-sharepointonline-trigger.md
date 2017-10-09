I det här exemplet ska jag visa hur toouse hello **SharePoint Online - när ett nytt objekt skapas** utlösa tooinitiate en logik app arbetsflödet när ett nytt objekt skapas i en SharePoint Online-lista.

> [!NOTE]
> Du får ange toosign till SharePoint-konto om du inte redan har skapat en *anslutning* tooSharePoint Online.  
> 
> 

1. Ange *sharepoint* i hello sökrutan på hello logic apps designer väljer du sedan hello **SharePoint Online - när ett nytt objekt skapas** utlösare.  
   ![SharePoint online utlösande bilden](./media/connectors-create-api-sharepointonline/trigger-1.png)  
2. Hej **när ett nytt objekt skapas** kontrollen visas.  
   ![Bild 2 till SharePoint online-utlösare](./media/connectors-create-api-sharepointonline/trigger-2.png)   
3. Välj en **webbplatsens URL**. Detta är hello SharePoint online-webbplatsen du vill använda toomonitor för nya objekt tootrigger arbetsflödet.  
   ![Bild 3 till SharePoint online-utlösare](./media/connectors-create-api-sharepointonline/trigger-3.png)   
4. Välj en **namn**. Detta är hello på hello SharePoint Online-webbplats du vill använda toomonitor för nya objekt som utlöser arbetsflödet.  
   ![Bild 4 till SharePoint online-utlösare](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Din logikapp har nu konfigurerats med en utlösare som börjar en körning av hello andra utlösare och åtgärder i hello arbetsflöde. Detta sker varje gång en ny artikel skapas i SharePoint Online listan som du har valt.  

