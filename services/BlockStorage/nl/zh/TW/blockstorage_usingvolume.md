{:new_window: target="_blank"} 

# 使用 {{site.data.keyword.blockstorageshort}} 磁區  
{: #using-block-storage-volume} 
前次更新：2016 年 9 月 7 日
{: .last-updated}

若要使用磁區，請遵循下列步驟：

## 刪除磁區 {: #deleting-volume}

1.  在 Bluemix 使用者介面中，選取**主控台 > 儲存空間**。
2.  選取您先前佈建的 Block Storage 實例。
3.	選取您要刪除的磁區。
4.	從「動作」下拉功能表，按一下**刪除**。
5.	確認刪除此磁區。

您無法刪除已連接至虛擬伺服器的磁區。您必須先分離該磁區。刪除磁區會使得未來無法存取該磁區，且其中的資料會遺失。此外，您也無法刪除具有相關聯 Snapshot 的磁區。

## 擴充磁區 {: #extending-volume}
您可以透過**擴充**動作來增加磁區大小，最大可以達到原始大小的十倍。您無法縮減磁區大小。

1.  在 Bluemix 使用者介面中，選取**主控台 > 儲存空間**。
2.  選取您先前佈建的 Block Storage 實例。
3.	選取要擴充的磁區。
4.	從「動作」下拉功能表，按一下**擴充**。
5.	選取磁區的新的大小。提供磁區的新的大小總計。
6.	按一下**擴充**，以提交資訊並關閉此對話框。 

若要擴充，磁區必須處於**可用**狀態。擴充磁區之後，您必須通知檔案系統（例如 ext4），磁碟已經透過將檔案系統重新調整大小成新的大小而擴充。 

## 連接及分離磁區與虛擬伺服器 {: #attaching-detaching-volume}
利用特定的裝置名稱，將磁區作為裝置來與虛擬伺服器連接及分離。若要讓虛擬伺服器能夠將資料持續保存在磁區上，您必須將磁區連接至虛擬伺服器。

若要連接磁區，請遵循下列步驟： 

1.  在 Bluemix 使用者介面中，選取**主控台 > 儲存空間**。
2.  選取您先前佈建的 Block Storage 實例。
3.	從可用磁區清單中，選取一個磁區。
4.	從「動作」下拉功能表，按一下**連接**。
5.	在「連接」對話框中，從下拉清單中選取一個虛擬伺服器實例。 
6.	選擇性地指定用來連接此磁區的裝置。如果您未指定裝置，系統會自動選取虛擬伺服器上的第一個可用裝置。
7.	按一下**連接**，以提交資訊並關閉此對話框。

此磁區即會列在連接磁區表格中，其中包含虛擬伺服器實例的相關資訊。虛擬伺服器現在即可使用此裝置來持續保存資料。 

當您準備好分離磁區時，必須準備虛擬伺服器以進行分離。例如，停止正在使用檔案系統的任何應用程式。此外，也請將裝置從虛擬伺服器實例內卸載。

若要分離磁區，請遵循下列步驟： 

1.  在 Bluemix 使用者介面中，選取**主控台 > 儲存空間**。
2.  選取您先前佈建的 Block Storage 實例。
3.	從連接磁區清單中，選取一個磁區。 
4.	從「動作」下拉功能表，按一下**分離**。
5.	在對話框中確認分離。 

分離之後，磁區即無法再用於虛擬伺服器實例中的 I/O 作業。在 {{site.data.keyword.blockstorageshort}} 服務使用者介面中，此磁區現在可用來連接至其他虛擬伺服器。
