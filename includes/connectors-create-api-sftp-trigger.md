Lägg till en utlösare.

1. Ange *sftp* i hello sökrutan på hello logic apps designer väljer du sedan hello **SFTP - när en fil har lagts till eller ändrats** utlösare   
   ![SFTP utlösarbild 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. Hej **när en fil har lagts till eller ändrats** kontrollen öppnas  
   ![Bild 2 till SFTP utlösare](./media/connectors-create-api-sftp/trigger-2.png)  
3. Välj hello **...**  finns på hello höger sida av hello kontroll. Då öppnas hello mappen väljarkontrollen  
   ![Bild 3 till SFTP utlösare](./media/connectors-create-api-sftp/action-1.png)  
4. Välj hello **SFTP** tooselect hello-rotmappen som hello mappen toomonitor för nya eller ändrade filer. Rotmapp för meddelande hello visas nu i hello **mappen** kontroll.  
   ![Bild 4 till SFTP utlösare](./media/connectors-create-api-sftp/action-2.png)   

Din logikapp har nu konfigurerats med en utlösare som börjar en körning av hello andra utlösare och åtgärder i hello arbetsflödet när en fil ändras eller skapas i hello viss SFTP-mapp. 

> [!NOTE]
> Det måste innehålla minst en utlösare och en åtgärd för en logik app toobe funktionella. Hello åtgärderna i hello nästa avsnitt tooadd en åtgärd.  
> 
> 

