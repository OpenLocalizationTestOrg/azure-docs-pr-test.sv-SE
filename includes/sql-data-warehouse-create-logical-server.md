### <a name="create-a-new-logical-sql-server-in-hello-azure-portal"></a>Skapa en ny logisk SQLServer i hello Azure-portalen

1. Klicka på **Nytt**, sök efter **logisk server** och tryck på **Retur**.

    ![sök efter logisk server](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Välj **SQL-server (logisk server)** 

    ![välj logisk server](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Klicka på **skapa** tooopen hello-bladet ny SQL Server (logisk server).

   <kbd>![öppna logiska serverblad](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![logiska serverblad](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd>
  
3. Ange ett giltigt namn för hello ny logisk server i hello SQL Server (logisk server) bladet server name textruta. En grön kryssmarkering visar att du har angett ett giltigt namn.
    
    ![Namn på ny server](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > hello fullständigt kvalificerade namn för den nya servern kommer att < namn_på_servern >. database.windows.net.
    >
    
4. Ange ett användarnamn för inloggning för hello SQL-autentisering för den här servern hello Server-administratören logga in i textrutan. Den här inloggningen kallas hello huvudsaklig inloggning på servern. En grön kryssmarkering visar att du har angett ett giltigt namn.
    
    ![SQL-administratörsinloggning](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. I hello **lösenord** och **Bekräfta lösenord** textrutor, ange ett lösenord för hello server huvudsaklig inloggning på kontot. En grön kryssmarkering visar att du har angett ett giltigt lösenord.
    
    ![SQL-administratörslösenord](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Välj en prenumeration där du har behörighet toocreate objekt.

    ![prenumeration](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Hello resurs gruppen i textrutan Välj **Skapa nytt** och ange ett giltigt namn för hello ny resursgrupp (du kan också använda en befintlig resursgrupp om du redan har skapat en själv) hello resurs gruppen i textrutan. En grön kryssmarkering visar att du har angett ett giltigt namn.

    ![Ny resursgrupp](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. I hello **plats** textruta, Välj en datatyp center lämpliga tooyour plats - till exempel ”östra”.
    
    ![Serverplats](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > Hej kryssrutan för **Tillåt azure-tjänster tooaccess server** kan inte ändras på det här bladet. Du kan ändra den här inställningen på hello serverblad för brandväggen. Mer information finns i [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md) (Komma igång med säkerhet).
    >
    
9. Klicka på **Skapa**.

    ![Knappen Skapa](./media/sql-data-warehouse-create-logical-server/create.png)

