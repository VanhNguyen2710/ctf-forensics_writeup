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




