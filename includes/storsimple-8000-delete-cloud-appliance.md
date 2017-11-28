#### <a name="to-delete-a-cloud-appliance"></a><span data-ttu-id="b0e98-101">Så här tar du bort en molninstallation</span><span class="sxs-lookup"><span data-stu-id="b0e98-101">To delete a cloud appliance</span></span>

1. <span data-ttu-id="b0e98-102">Logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b0e98-102">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="b0e98-103">Du kan bara ta bort en inaktiverad enhet som inte innehåller data.</span><span class="sxs-lookup"><span data-stu-id="b0e98-103">You can only delete a deactivated device that does not contain data.</span></span> <span data-ttu-id="b0e98-104">Ta bort alla data på enheten först. Du kan även [redundansväxla data](../articles/storsimple/storsimple-8000-device-failover-cloud-appliance.md) i volymbehållare till en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="b0e98-104">Delete the data on the device first or you can [fail over the data](../articles/storsimple/storsimple-8000-device-failover-cloud-appliance.md) in volume containers to another device.</span></span> <span data-ttu-id="b0e98-105">När alla data tagits bort är du färdig och kan inaktivera enheten.</span><span class="sxs-lookup"><span data-stu-id="b0e98-105">Once the data is deleted, you are ready to deactivate the device.</span></span>
3. <span data-ttu-id="b0e98-106">På tjänstsidan för StorSimple Devide Manager klickar du på **Enheter** och sedan markerar du enheten.</span><span class="sxs-lookup"><span data-stu-id="b0e98-106">In your StorSimple Devide Manager service page, click **Devices** and then select the device.</span></span> <span data-ttu-id="b0e98-107">Högerklicka och välj **Inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="b0e98-107">Right-click and select **Deactivate**.</span></span>
4. <span data-ttu-id="b0e98-108">När enheten är inaktiverad högerklickar du på enheten och väljer **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b0e98-108">Once the device is deactivated, right-click the device and select **Delete**.</span></span>

    ![Välj den inaktiverade enheten och klicka på ta bort](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance1.png)

5. <span data-ttu-id="b0e98-110">Ange enhetsnamnet för att bekräfta borttagningen.</span><span class="sxs-lookup"><span data-stu-id="b0e98-110">Type the device name to confirm the deletion.</span></span> <span data-ttu-id="b0e98-111">När enheten har tagits bort uppdateras listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="b0e98-111">After the device is deleted, the device list updates.</span></span>

    ![Bekräfta borttagning](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance2.png)

6. <span data-ttu-id="b0e98-113">Du får ett meddelande när enheten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b0e98-113">You are notified after the device is deleted.</span></span>

    ![Meddelande för lyckad borttagning av enhet](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance4.png)

7. <span data-ttu-id="b0e98-115">Listan över enheter uppdateras för att visa att enheten tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b0e98-115">The list of devices updates to indicate the deleted device.</span></span>

    ![Uppdaterad enhetslista](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance5.png)
