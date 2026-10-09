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

sau đó ghép lại thành định dạng abc|abc|abc

<img width="1536" height="60" alt="image" src="https://github.com/user-attachments/assets/2882f19e-8a6e-4166-8c4e-655b5682c6fc" />







