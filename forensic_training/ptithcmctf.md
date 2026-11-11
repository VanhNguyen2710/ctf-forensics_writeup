FOR04
author:nhoktiger12

đề bài cấp cho ta một file .ad1 khi ta connect vào netcat để bắt đầu trả lời câu hỏi, câu 1 của đề là

Q1: When was the Windows Data Deduplication feature first installed on the system?

khi một feature được cài vào hệ thống thì ta sẽ nghĩ ngay tới log của system.evtx, cụ thể hơn ta sẽ kiểm tra event id 7045 vì đây là event id được ghi lại khi cài một service hoặc driver mới. 

<img width="1917" height="1008" alt="image" src="https://github.com/user-attachments/assets/79cf4e11-70df-44c9-b9d7-f3c398206007" />

ta tìm được đáp án, đổi 16:56:40 của giờ việt nam ra utc ta sẽ có 09:56:40 và theo định dạng câu hỏi cần thêm ngày tháng năm theo format yyyy-mm-dd thì đáp án cần điền là 2025-09-18 09:56:40

->Q1:2025-09-18 09:56:40

Q2:How many bytes of disk space were saved after the first optimization job was completed by Data Deduplication?

ở đây ta sẽ tìm theo event id 6153 vì đó là event khi một tác vụ deduplication hoàn tất nó nằm trong log Microsoft-Windows-Deduplication_4Operational.evtx, ta có khung thời gian từ trước vậy nên log sẽ nằm quanh khung thời gian sau khi cài đặt. 
Khi đó ta có được 
<img width="1917" height="1015" alt="image" src="https://github.com/user-attachments/assets/fb86c619-a038-400c-8e25-77c9acabe926" />
ở đây bytes tiết kiệm được là 6805707

->Q2:6805707

Q3:When was Data Deduplication most recently re-enabled on volume D:?

ở đây câu hỏi là ta đã bật dedup vào khoảng thời gian gần nhất là khi nào, khi kiểm tra dedupConfig.01.xml trong setting của dedup ta có được một giá trị thời gian 134028128759740533 và khi đổi qua giờ utc ta sẽ có là 2025-09-20 03:34:35.9740533. Đây là file xml chứa thông tin cấu hình thay đổi tức là khi dedup vừa được bật lên là file sẽ được cập nhập, ta có thể thấy rằng sau đó khoảng 20 giây ta liền thấy quá trình optimize thành công

<img width="1917" height="1020" alt="image" src="https://github.com/user-attachments/assets/49e89e0c-0ebc-4f2f-8835-f5d1240de45d" />

<img width="1917" height="1010" alt="image" src="https://github.com/user-attachments/assets/dfa51492-13fd-48ab-ac7f-dd46f07ba51f" />

->Q3:2025-09-20 03:34:35

Q4:List all file extensions that are not optimized by Data Deduplication in alphabetical order.

trong file xml có đáp án luôn gồm các trường excludeFileExtensions và excludeFileExtensionsDefault tổng cộng có 6 cái bị loại trừ theo cấu hình và mặc định

<img width="1417" height="803" alt="image" src="https://github.com/user-attachments/assets/ea29c504-3d6a-4e25-8498-7ec81df9f10b" />

->Q4:avi, bak, edb, iso, jrs, mp4, tif

Q5:How many files have been optimized by Data Deduplication?
ở đây khi check lần optimize cuối cùng ta thấy nó tối ưu được 168 file

<img width="1917" height="1017" alt="image" src="https://github.com/user-attachments/assets/2ae6f46a-df12-40aa-a1c7-173a3b660214" />

->Q5:168

FOR01
author:KAiZ3n
An organization has detected signs of compromise on a workstation and a Domain Controller following a suspicious incident involving a document sent to a user.

The Incident Response (IR) team has collected forensic artifacts from both the endpoint and the Domain Controller.

Your task is to analyze the evidence, reconstruct the attack chain, and explain how the attacker pivoted from the compromised workstation into the Active Directory infrastructure.
