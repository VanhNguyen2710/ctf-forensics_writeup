````
Ring... tuttt... tuttt...

SOC: Alo, Vũ ấy hả em.

VuVT: Anh ơi, em chỉ chạy update thôi mà sao máy em tự nhiên bị mã hóa rồi...

SOC: Đừng động vào máy nữa. Bọn anh sẽ kiểm tra.

Một bộ dữ liệu được thu thập từ máy tính gặp sự cố.

Hãy điều tra xem chuyện gì đã thực sự xảy ra và tìm ra thông tin cần thiết để khôi phục kết quả cuối cùng.

Ring... tuttt... tuttt....

SOC: Moshi moshi, Is that you Vu?
````
ta thấy từ đề bài mình biết được rằng nhân viên VuVT đã chạy một file update hoặc gì đó và sau đó các dữ liệu trong máy đã bị mã hóa rất có thể là một cuộc tấn công ransomware. Đề muốn ta tìm ra thông tin để khôi phục kết quả rất có thể muốn ta tìm được key hoặc phương thức mã hóa từ đó đảo ngược quy trình và lấy lại dữ liệu rất có thể dữ liệu đó chứa flag.

Ở trong download của user bkav ta thấy được một folder có tên update và bên trong là 2 file update.exe và updater.exe rất có thể đây là con malware gây nên sự cố này này.

<img width="847" height="252" alt="Screenshot 2026-10-08 171751" src="https://github.com/user-attachments/assets/78e655e3-741c-4b4a-8408-b35392e1989d" />

đầu tiên ta thấy được rằng con malware này nó lấy computer name và pid của session.

<img width="1318" height="417" alt="image" src="https://github.com/user-attachments/assets/38ec8003-ced2-4d8d-a816-d651b8bae894" />

sau đó ở dưới ta thấy rằng sub_408070 đang khởi tạo một url và sau đó hàm sub_406500 tiến hành tương tác với url đó bằng WinHttpOpen.

<img width="1112" height="61" alt="image" src="https://github.com/user-attachments/assets/cdb2840a-c993-400c-a8c7-cda2c720eddc" />

sau khi malware thực hiện tương tác với url thì malware này nó sẽ gọi sub_406940 rồi trích xuất campaign còn nếu server ko trả ra chuỗi thì nó sẽ đi tới label 121.

<img width="908" height="41" alt="image" src="https://github.com/user-attachments/assets/70ae1ca3-b359-4282-aa9b-94ccc41a7799" />

<img width="692" height="35" alt="image" src="https://github.com/user-attachments/assets/1602a137-b18b-4f08-8734-6dd3191e6360" />

sau đó tiến hành đổi từ hoa sang thường sau khi lấy computername.

<img width="1365" height="422" alt="image" src="https://github.com/user-attachments/assets/6b0f9ee9-4dcf-4722-a8a3-20008d03e6a2" />

rồi đổi pid number từ dạng số sang dãng chuỗi tức từ int sang string.

<img width="1493" height="187" alt="image" src="https://github.com/user-attachments/assets/b26cb37b-ff7c-44ea-8c8f-21fc42812c0e" />

sau đó ghép lại thành định dạng hostname|PID|campaign

<img width="1536" height="60" alt="image" src="https://github.com/user-attachments/assets/2882f19e-8a6e-4166-8c4e-655b5682c6fc" />

sau đó băm và dùng thuật toán sha256 để tạo ra 32 bytes khóa

<img width="1503" height="563" alt="image" src="https://github.com/user-attachments/assets/df31eb24-a77a-4eae-b7f6-3cfc5e7f8a3e" />

sau khi finish hash ta thấy có một hàm sub_405F20() nên ta truy cập vào để xem sau khi tạo khóa thì nó sẽ làm gì, 

<img width="1536" height="567" alt="image" src="https://github.com/user-attachments/assets/2b01d585-52f0-4876-a4de-45df35e0fdd8" />

ta thấy nó gọi một hàm là sub_405C90 với lpWideCharStr = lpFileName[0]; trỏ đến phần tử đầu tiên trong đường dẫn và lpFileName[1] là phần tử cuối của đường dẫn-> sub_405C90 rất có thể đang mã hóa file thuộc đường dẫn này.

ở trong sub_405C90 thì nó có một hàm để tạo nonce tức iv mà ta thường thấy trong mã hóa aes với 12 byte được tạo ngẫu nhiên bằng hàm BCryptGenRandom

<img width="896" height="135" alt="image" src="https://github.com/user-attachments/assets/6145ed45-d4f1-43b6-826d-f711a68e72e1" />

ở đây ta có thể thấy nó đang thực hiện mã hóa và xóa file với sub_404FA0 rất có thể đât là hàm dùng để mã hóa và sau đó tiến hành xóa file qua hàm __std_fs_remove

<img width="1476" height="462" alt="image" src="https://github.com/user-attachments/assets/770a53ab-fe6a-4f12-9b9b-eb2f74256af4" />

trong sub_404FA0 ta thấy được nó đang tiến hành mã hóa bằng AES-GCM

<img width="1163" height="185" alt="image" src="https://github.com/user-attachments/assets/e996d20f-8397-4641-a1bf-084e0120657e" />

<img width="1355" height="501" alt="image" src="https://github.com/user-attachments/assets/2518c8b9-bc2f-4382-9dc6-63e9bce53cca" />

bởi vì iv được tạo ra ngẫu nhiên nên chúng ta vẫn còn thiếu iv để tiến hành giải mã được các file bị mã hóa, ta quay lại sub_405C90 để xem sau khi nó gọi hàm mã hóa nó sẽ làm gì tiếp theo thì thấy sub_404F00 đang lưu lại nonce, rất có thể vì đây là mã độc ransosm tống tiền nên attacker vẫn cần để có thể giải mã sau khi user trả tiền hoặc với một ý đồ khác.
<img width="1532" height="541" alt="image" src="https://github.com/user-attachments/assets/feb0236a-9414-4844-88cb-8047bfe27b0b" />












