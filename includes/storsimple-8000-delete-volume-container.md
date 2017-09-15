<!--author=alkohli last changed: 01/13/17-->

<span data-ttu-id="91b8b-101">Om volymbehållaren har associerade volymer, ta volymerna offline först.</span><span class="sxs-lookup"><span data-stu-id="91b8b-101">If the volume container has associated volumes, take those volumes offline first.</span></span> <span data-ttu-id="91b8b-102">Följ stegen i [kopplar från en volym](../articles/storsimple/storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="91b8b-102">Follow the steps in [Take a volume offline](../articles/storsimple/storsimple-manage-volumes.md#take-a-volume-offline).</span></span> <span data-ttu-id="91b8b-103">När volymerna som är offline kan du ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="91b8b-103">After the volumes are offline, you can delete them.</span></span> <span data-ttu-id="91b8b-104">När volymbehållaren har inga associerade volymer, ta bort volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="91b8b-104">When the volume container has no associated volumes, delete the volume container.</span></span> <span data-ttu-id="91b8b-105">Utför följande procedur för att ta bort en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="91b8b-105">Perform the following procedure to delete a volume container.</span></span>

#### <a name="to-delete-a-volume-container"></a><span data-ttu-id="91b8b-106">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="91b8b-106">To delete a volume container</span></span>
1. <span data-ttu-id="91b8b-107">Gå till StorSimple Device Manager-tjänsten och klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="91b8b-107">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="91b8b-108">Välj och klicka på enheten och gå sedan till **Inställningar > Hantera > volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="91b8b-108">Select and click the device and then go to **Settings > Manage > Volume containers**.</span></span>

    ![Volymen behållare bladet](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. <span data-ttu-id="91b8b-110">Tabell listan över volymbehållare, Välj volymbehållare som du vill ta bort, högerklicka på **...**  och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="91b8b-110">From the tabular list of volume containers, select the volume container you want to delete, right click **...** and then select **Delete**.</span></span>

    ![Ta bort volymbehållare](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. <span data-ttu-id="91b8b-112">Om en volymbehållare har ingen associerad volymer kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="91b8b-112">If a volume container has no associated volumes, then it can be deleted.</span></span> <span data-ttu-id="91b8b-113">När du uppmanas att bekräfta, granska och markera kryssrutan om effekten av att ta bort volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="91b8b-113">When prompted for confirmation, review and select the checkbox stating the impact of deleting the volume container.</span></span> <span data-ttu-id="91b8b-114">Klicka på **ta bort** att ta bort volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="91b8b-114">Click **Delete** to then delete the volume container.</span></span>

    ![Bekräfta borttagning](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

<span data-ttu-id="91b8b-116">Listan över volymbehållare uppdateras för att återspegla den borttagna volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="91b8b-116">The list of volume containers is updated to reflect the deleted volume container.</span></span>

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


