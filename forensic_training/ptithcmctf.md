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

Q6:When is the next Throughput Optimisation job scheduled to run?

ta check throughputoptimization ở ổ đĩa C nơi quản lý các task schedule của window cụ thể nó sẽ lưu vào C:\Windows\System32\Tasks

<img width="1917" height="1021" alt="image" src="https://github.com/user-attachments/assets/b8ad5e6e-7c2b-4f77-9355-211f501637e7" />

trong này ta thấy rằng <StartBoundary>2025-09-20T06:50:00</StartBoundary> nghĩa là ngày lịch có hiệu lực, và nó chạy vào mỗi thứ 5 hằng tuần tức lịch dedup tiếp theo sẽ rơi vào t5 tuần sau tức 2025-09-25 06:50:00 vì 2025-20-9 là t7.

->Q6:2025-09-25 06:50:00

Q7:What is the name of the financial report that was recently deleted in an attempt to cover up criminal activity?

ở đây đề muốn hỏi cái báo cáo tài chính nào bị xóa, ta sẽ dùng mft explorer để xem %MFT vì đây là bảng ghi thông tin về các file trên phân vùng ntfs của window, khi xóa report thì rất có thể thông tin của nó vẫn còn lưu lại mft từ đó có thể suy ra hướng tìm.

<img width="1627" height="965" alt="image" src="https://github.com/user-attachments/assets/14c89cef-cce7-4240-a7a0-8dec6244c400" />

->Q7:Internal Consolidated Financial Statements 2024.docx

Q8:What is the MFT entry number of that report file?

ở đây ta có thể thấy luôn entry number là 46

<img width="1622" height="967" alt="image" src="https://github.com/user-attachments/assets/0b65a5bf-87db-481a-ab00-108b6477fbfa" />

->Q8:46

Q9: List all the FILE_ATTRIBUTE flags that are set on the report file. Submit answer in ascending order based on the hex value of each flag.
trong phần details ta thấy được 3 phần của FILE_ATTRIBUTE flags
Flags: Archive|SparseFile|ReparsePoint

<img width="1626" height="966" alt="image" src="https://github.com/user-attachments/assets/635eee59-3f10-4526-973a-0ea2af9e66d7" />

->Q9:FILE_ATTRIBUTE_ARCHIVE, FILE_ATTRIBUTE_SPARSE_FILE, FILE_ATTRIBUTE_REPARSE_POINT

Q10:At what offset in the $MFT file does the $REPARSE_POINT attribute of the report begin? (Answer as an 8-digit hexadecimal offset, prefixed with 0x)

ta biết rằng First Attribute Offset thường nằm trong khoảng byte 0x14 đến 0x15

<img width="853" height="167" alt="image" src="https://github.com/user-attachments/assets/0579972b-3db1-4c63-9af2-e4f2bd0087b2" />

tức vị trí của First Attribute Offset nó sẽ nằm trong khoảng vị trí bắt đầu của mft+0x14 mà trước đó ta biết vị trí bắt đầu của mft là Offset: 0xB800, ta sẽ tìm First Attribute Offset ở 0xB814.

<img width="835" height="332" alt="image" src="https://github.com/user-attachments/assets/afcf7183-9b68-4749-8386-e742691d29ce" />

ta kết luận được thuộc tính đầu tiên ở offset cách điểm khởi đầu 0x38 -> 0xB800 + 0x38 = 0xB838, khi đã có điểm khởi đầu ta sẽ cộng các size cho tới reparse point 
0xB838+0x60(StandardInformation)+0xC8(FileName)+0x50(Data)=0x0000b9b0(ReparsePoint)

->Q10:0x0000b9b0

Q11:After the report was optimized by Data Deduplication, into how many chunks was its data divided?

ta sẽ tìm tới chunkstore và trích xuất 2 file .ccc ở data và stream sau đó dùng tool dedup explorer(https://github.com/7h3kn0w3r/DedupInspector/releases)

<img width="1098" height="672" alt="image" src="https://github.com/user-attachments/assets/59952738-99ed-426b-9fc5-5e982dddef11" />

->Q11:2

Q12: What is the data length in bytes of the first deduplicated chunk belonging to the report file under investigation?

ở đây Theo tài liệu Forensic analysis of deduplicated file systems của hai tác giả Dario Lanterna và Antonio Barili (https://dfrws.org/sites/default/files/session-files/2017_EU_paper_forensic_analysis_of_deduplicated_file_systems.pdf?). ta biết rằng tại offset tương đối 0x78 trong Reparse Point có một chuỗi byte dùng để nhận diện stream của file đó.

<img width="1917" height="907" alt="image" src="https://github.com/user-attachments/assets/9af57b3a-18bf-4bdf-a3da-62bc39a5fbb0" />

ban đầu ta có 0xB9B0 là vị trí bắt đầu của REPARSE_POINT vậy 0xB9B0 + 0x78 = 0xBA28, ta sẽ lấy len(30) tức 30 byte đây là chuỗi để nhận diện được stream của file Internal Consolidated Financial Statements 2024.docx.

<img width="1407" height="712" alt="image" src="https://github.com/user-attachments/assets/d0f9e4d6-f7ff-4571-8d25-f82f33bbbcb6" />

ta sẽ tìm cái chuỗi nhận diện đó bên file .ccc ở folder stream

<img width="1400" height="707" alt="image" src="https://github.com/user-attachments/assets/e9ac23aa-03b5-42f4-afea-206c16cce4a0" />

tại đây ta thấy được Smap cái này nó cho ta biết thứ tự các chunk và vị trí kết thúc của từng chunk trong file gốc.

<img width="1412" height="502" alt="image" src="https://github.com/user-attachments/assets/e788e1aa-a053-42d4-881d-05362276e224" />

theo cấu trúc Smap, ta biết rằng The value at offset 0xA8 is the length of the chunk payload tức là ở offset đầu tiên, chuỗi nhận diện ta biết là Ckhr + 0x38 vậy khi trừ 0x38 ta sẽ có entry của Ckhr, 0x5298 - 0x38 = 0x5260 sau đó lấy 0x5260+0xA8=0x5308

<img width="1913" height="885" alt="image" src="https://github.com/user-attachments/assets/9e3eed34-9976-472b-9056-56575e5d341e" />

sau khi tới được đó ta thấy được đáp án

<img width="1408" height="717" alt="image" src="https://github.com/user-attachments/assets/b523eaa3-a41e-41a3-9c4b-6ef011815bff" />

->Q12:61095

FOR01
author:KAiZ3n
An organization has detected signs of compromise on a workstation and a Domain Controller following a suspicious incident involving a document sent to a user.

The Incident Response (IR) team has collected forensic artifacts from both the endpoint and the Domain Controller.

Your task is to analyze the evidence, reconstruct the attack chain, and explain how the attacker pivoted from the compromised workstation into the Active Directory infrastructure.
