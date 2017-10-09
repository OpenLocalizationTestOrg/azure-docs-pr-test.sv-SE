1. I hanteraren för redundanskluster, expandera **roller**, och sedan markera tillgänglighetsgruppen.  

2. På hello **resurser** fliken, högerklicka på hello grupplyssnarens namn och klicka sedan på **egenskaper**.

3. Klicka på hello **beroenden** fliken. Om flera resurser visas, kontrollerar du att hello IP-adresser har eller inte och beroenden.  

4. Klicka på **OK**.

5. Högerklicka på hello grupplyssnarens namn och klicka sedan på **Anslut**.

6. Efter hello lyssnare är online på hello **resurser** fliken, högerklicka på hello tillgänglighetsgruppen och klicka sedan på **egenskaper**.
   
    ![Konfigurera hello tillgänglighetsgruppsresursen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Skapa ett beroende på hello lyssnare resurs (inte hello IP-adress resurser namn) och klicka sedan på **OK**.
   
    ![Lägg till beroende på hello grupplyssnarens namn](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Starta SQL Server Management Studio och Anslut toohello primära repliken.

9. Gå för**AlwaysOn hög tillgänglighet** > **Tillgänglighetsgrupper** > **\<AvailabilityGroupName\>**   >  **Tillgänglighetsgruppens lyssnare**.  
    Hej grupplyssnarens namn som du skapade i hanteraren för redundanskluster ska visas.

10. Högerklicka på hello grupplyssnarens namn och klicka sedan på **egenskaper**.

11. I hello **Port** ange hello portnummer för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort som du använde tidigare (i den här självstudiekursen var 1433 hello standard), och klicka sedan på **OK**.

