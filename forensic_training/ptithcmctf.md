Đây là solution 2 bài for mình may mắn first blood trong giải ptithcmctf của mình. Ngoài ra trong giải thì có 1 bài 6/7 chưa kịp xong thì hết giờ nên cũng sẽ giải nốt bài đó trong này luôn.  
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

Q13:After recovering the file, provide the SHA-256 hash of the recovered financial report.

ta biết rằng entry+0x70 sẽ ra được vị trí bắt đầu của tham chiếu của chunk đầu tiên -> 0x52D0 và ta cũng biết luôn rằng mỗi tham chiếu chunk chiếm 0x40 byte vậy thì bắt đầu tham chiếu của chunk 2 sẽ là 0x52D0+0x40 = 0x5310 có thể hiểu rằng tham chiếu tức là ta sẽ dùng chuỗi này để grep trong data từ đó mới khôi phục được file gốc. Trong này có nói At 0x78 there is the absolute position in the chunkstore nghĩa là 0x8 từ vị trí xuất phát chứa địa chỉ

<img width="487" height="510" alt="image" src="https://github.com/user-attachments/assets/1ddf0f2f-f68e-4517-b9b6-a8186129d426" />

<img width="1412" height="721" alt="image" src="https://github.com/user-attachments/assets/794e310a-e746-4396-b726-4efaf422b228" />

sau khi đảo byte ta được 0x000000010006CE20 và 00000001 là Data container số 1, tương ứng 00000001.00000001.ccc cái sau là offset 0x6CE20, chunk 2 làm tương tự và ta có offset 0x7BD20.

ở Q12 ta đã có size của chunk 1 rồi chunk 2 ta cũng làm tương tự vì tham chiếu các chunk cách nhau đúng 0x40 như nãy ở trên đã nói vậy cái chỗ chứa size cũng có thể tìm ra offset bằng cách tương tự 0x5308 + 0x40 = 0x5348. Ta có size chunk 2 là 29498. Dựa vào size ta có thể suy ra 2 byte cuối của 2 chunk bằng công thức byte cuối=byte đầu + size - 1. Việc còn lại chỉ là bôi đen các offset của 2 chunk rồi ghép lại thôi.

<img width="1411" height="708" alt="image" src="https://github.com/user-attachments/assets/41f738d5-3a74-4cf5-a441-76a849677f8b" />

SHA-256 hash của file là e51e773e5404f29cc2816cff3fbdcc8b2c28f9e8040a6e7fe926b21336b66513

<img width="1436" height="92" alt="image" src="https://github.com/user-attachments/assets/221280de-9934-415a-9cc6-948330c103f3" />

->Q13: e51e773e5404f29cc2816cff3fbdcc8b2c28f9e8040a6e7fe926b21336b66513

Q14:Who is the author of the recovered financial report?

câu này mở info của word là thấy thôi 

<img width="1917" height="1003" alt="image" src="https://github.com/user-attachments/assets/bad9234d-b4ff-4f24-bee7-fe1f4a984d72" />

->Q14: Elaine Chua

Q15: How much was the profit for the year overstated in the audited report compared to the actual one?

<img width="852" height="596" alt="image" src="https://github.com/user-attachments/assets/293c8705-1fc8-4896-b11d-537434659f29" />

ở đây lỗ 1.647.555 mà tổng phân bổ ở cuối ghi lời 2.672.196 tức là bị khai khống 

2.672.196-(-1.647.555)=4.319.751

->Q15:4319751

FOR01
author:KAiZ3n
An organization has detected signs of compromise on a workstation and a Domain Controller following a suspicious incident involving a document sent to a user.

The Incident Response (IR) team has collected forensic artifacts from both the endpoint and the Domain Controller.

Your task is to analyze the evidence, reconstruct the attack chain, and explain how the attacker pivoted from the compromised workstation into the Active Directory infrastructure.

Q1: Full URL contacted by the suspicious payload 

theo đề ta biết được rằng sự việc xảy ra liên quan đến một tệp tài liệu bất ngờ. Khi kiểm tra ta thấy được một file document.pdf tuy nhiên nó là một file shortcut chứ không phải pdf thông thường. 

<img width="815" height="222" alt="image" src="https://github.com/user-attachments/assets/e1757734-bb78-40da-811c-d5f12dde2ed1" />

sau khi dùng linkinfo để kiểm tra thì ở đây ta thấy shortcut thực ra là mở powershell sau đó chạy lệnh đã được mã hóa bằng -enc và trong window thì cái này nó sẽ chuyển plaintext thành UTF-16LE rồi base64encode ta chỉ việc làm ngược lại

<img width="1442" height="160" alt="image" src="https://github.com/user-attachments/assets/a67a347f-0d3d-401d-b2b8-ad76c6150945" />

decode ra ta thấy được link và đáp án câu 1 

<img width="1916" height="910" alt="image" src="https://github.com/user-attachments/assets/d07ddfcc-5f61-47fd-a5ac-7fe99ae39da0" />

Q1:http://192.168.56.1:9091/s.ps1

Q2:Persistence technique ID

ta kiểm tra những technique persistence phổ biến của window như startup, sheduled task, services và registry. Khi kiểm tra NTUSER.DAT ta sẽ chú tâm tới Run Key vì nó cho phép chương trình tự động chạy khi đăng nhập vào tuy nhiên khi kiểm tra ta lại không thấy gì đáng ngờ

<img width="1917" height="1020" alt="image" src="https://github.com/user-attachments/assets/2d3477c1-9eb3-4e8c-8893-8ebac45dc17f" />

Ngoài ra trong lúc làm mình nảy ra ý tưởng là tìm lệnh trong memory để xem file ps1 nó làm gì hoặc biết đâu nó cho ta manh mối thì vô tình thấy được một file dllhost.exe nằm ở vị trí bất thường. Thông thương thì dllhost.exe nó phải nằm ở những vị trí như C:\Windows\System32\dllhost.exe hoặc C:\Windows\SysWOW64\dllhost.exe nhưng ở đây nó lại nằm ở một đường dẫn khá kì lạ nên rất có thẻ đó là file độc hại. 

<img width="1445" height="46" alt="image" src="https://github.com/user-attachments/assets/abe11866-4691-4c59-b7ef-4806717419cd" />

ta biết được rằng trên window có một cơ chế tên là COM nó được dùng khi chương trình cần sử dụng một chức năng từ một ứng dụng khác và để tránh gây lỗi bởi cơ chế COM nó sẽ được đưa vào dllhost.exe và chạy trong đó để nếu COM có lỗi gì cũng không ảnh hưởng tới chương trình gốc.

<img width="872" height="143" alt="image" src="https://github.com/user-attachments/assets/a0b53f4a-aa4d-4f7c-ab56-850f81dfed6b" />

vì vậy rất có thể COM đã bị lợi dụng để chạy chương trình, thêm nữa cái dllhost này nó nằm trong user thong từ đó ta nghĩ ngay tới kiểm tra UsrClass.dat của user thong vì đây là file lưu thông tin đăng ký COM của riêng user thong.

từ câu 1 ta đã biết ip của attacker rồi vậy nên khi grep trong UsrClass.dat ta biết được rằng attacker đã đăng ký một TypeLib trỏ đến scriptlet trên máy chủ attacker script:http://192.168.56.1:9091/launch.sct 

<img width="1917" height="1020" alt="image" src="https://github.com/user-attachments/assets/557e3750-44ea-4066-97b3-75e0ae891f1d" />

vậy thì với việc có một dllhost.exe nằm ở vị trí bất thường sau đó sửa COM để chuyển hướng sang một script độc hại ta có thể suy ra com hijacking giwof chỉ việc tra id thôi

<img width="870" height="137" alt="image" src="https://github.com/user-attachments/assets/fdb06cfa-2578-4f41-8bb1-ce1ce5534b87" />

->Q2:T1546.015

