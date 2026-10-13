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
dữ liệu cần khôi phục có lẽ là flag đã bị encrypt bởi ransom
<img width="1176" height="487" alt="image" src="https://github.com/user-attachments/assets/e0fb20bb-5f92-4103-8e7b-115d8b81e0e3" />

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
ở đây ta thấy nó lấy 8 byte mã nhận diện, 4 byte mã kiểm tra và cộng với 12 byte nonce sau đó lưu vào ram theo cấu trúc [ Mã nhận diện file ][ Nonce ][ Mã kiểm tra ] rồi lưu vào ram vậy nên đây sẽ là hướng tiếp theo mình đi để tìm ra được nonce nhằm mục đích giải mã

<img width="1536" height="686" alt="image" src="https://github.com/user-attachments/assets/2f15bb85-f903-4c4d-ae61-ea53c092357c" />

theo cấu trúc ta biết được ban đầu nó tạo ra 8 byte nhận diện và lưu vào trước tức nó sẽ từ offset 0-7 sau đó lưu 8 byte nonce tức là sẽ từ 8-15 2 cái này sẽ lưu vào v8 theo đúng thứ tự nhận diện là low còn nonce là high, tiếp đó nó lưu thêm 4 byte nonce offset 16–19 vào low của v9 và 4 byte check vào high của v9 tức offset 20–23. Sau đó cái check sẽ được xor với một hằng số cố định đây sẽ là ý tưởng để ta giải quyết được, nó lưu vào ram vậy thì để tìm kiếm những cái có khả năng là nonce ta sẽ tạo một script quét ram vì ta biết được khi ransom mã hóa một bản như flag.txt thành .enc hay gì đó nó sẽ lưu vào ram đoạn 24 byte vừa phân tích, khi tìm được đoạn đấy ta sẽ check xem coi id tự tính để ra check khi xor với hằng số có bằng với giá trị trong ram ko từ đó xác định được mã 24 byte này có phải dùng để mã hóa file đó hay ko.
script mình dùng 
````
import argparse
import mmap
import struct
import sys

MAGIC = 0xA17C4E29
RECORD_SIZE = 24
UINT32 = struct.Struct('<I')


def scan_ram(path):
    found = 0
    with open(path, 'rb') as f:
        f.seek(0, 2)
        size = f.tell()
        f.seek(0)
        if size < RECORD_SIZE:
            print('RAM dump is too small for a 24-byte record.', file=sys.stderr)
            return

        with mmap.mmap(f.fileno(), 0, access=mmap.ACCESS_READ) as ram:
            for offset in range(size - RECORD_SIZE + 1):
                file_id_low = UINT32.unpack_from(ram, offset)[0]
                check = UINT32.unpack_from(ram, offset + 20)[0]
                if (file_id_low ^ MAGIC) != check:
                    continue

                file_id_bytes = ram[offset:offset + 8]
                nonce = ram[offset + 8:offset + 20]
                print(f'offset=0x{offset:x} file_id={file_id_bytes.hex()} '
                      f'nonce={nonce.hex()} check=0x{check:08x}')
                found += 1

    print(f'Found {found} candidate record(s).', file=sys.stderr)


if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Find 24-byte nonce records in a RAM dump')
    parser.add_argument('ram', help='Path to mem.raw')
    args = parser.parse_args()
    scan_ram(args.ram)
````
khi chạy xong script kết quả ta thu được
````
offset=0x45322d20 file_id=09bf333824d1c1bb nonce=f13ba19b255ae4f590c25cfd check=0x994ff120
offset=0x45322d38 file_id=b412dea421fb3e14 nonce=df8f031f5e50e9b0eaea27ff check=0x05a25c9d
offset=0x45322d50 file_id=d9597923064b29ce nonce=97595686149cf7a4afe5cd6f check=0x820517f0
offset=0x45322d68 file_id=dccc0e51a39631e4 nonce=24c696f9341fd8e7882b44f0 check=0xf07282f5
offset=0x45322d80 file_id=ce7ce4992f5fb13c nonce=5a3ea1093656db3093d8df16 check=0x389832e7
offset=0x45322d98 file_id=8119e0d3f82dc644 nonce=257c8a8ff3bf9cb6cdddce85 check=0x729c57a8
offset=0x45322db0 file_id=520fb18cf905e12d nonce=adea20882fde1b4d6e74f57c check=0x2dcd417b
offset=0x6d95bd20 file_id=09bf333824d1c1bb nonce=f13ba19b255ae4f590c25cfd check=0x994ff120
offset=0x6d95bd38 file_id=b412dea421fb3e14 nonce=df8f031f5e50e9b0eaea27ff check=0x05a25c9d
offset=0x6d95bd50 file_id=d9597923064b29ce nonce=97595686149cf7a4afe5cd6f check=0x820517f0
offset=0x6d95bd68 file_id=dccc0e51a39631e4 nonce=24c696f9341fd8e7882b44f0 check=0xf07282f5
offset=0x6d95bd80 file_id=ce7ce4992f5fb13c nonce=5a3ea1093656db3093d8df16 check=0x389832e7
offset=0x6d95bd98 file_id=8119e0d3f82dc644 nonce=257c8a8ff3bf9cb6cdddce85 check=0x729c57a8
offset=0x6d95bdb0 file_id=520fb18cf905e12d nonce=adea20882fde1b4d6e74f57c check=0x2dcd417b
Found 14 candidate record(s).
````
ở đây 
flag.jpeg.enc có nonce là	97595686149cf7a4afe5cd6f theo file id ta tính dựa theo logic của hàm sub_404510
vậy ta đã thu được nonce giờ cần tìm thêm key để có thể giải mã, trước đó ta đã biết key gồm hostname|pid|campagin dưới dạng hex và với hostname và pid ta có thể tìm từ sysmon log của máy vì khi con malware hoạt động log của tiến trình này sẽ được chạy từ đó biết pid và host name, campaign là từ một request trên web vậy nên có thể suy luận ta có thể thu được qua lưu luognjw mạng được lưu vào pcap

<img width="1888" height="1015" alt="image" src="https://github.com/user-attachments/assets/0379488f-9b82-449a-a089-8010f75b3402" />

khi ptich pcap ta có thể thấy ip 192.168.56.10 đang tải malware về như ban đầu ta điều tra vậy nên nếu malware lấy campaign thì chỉ có thể là từ ip này gửi tới đâu đó

<img width="1862" height="21" alt="image" src="https://github.com/user-attachments/assets/e88c56ff-49e8-4d48-86de-69ad9fb1517a" />

ở đây khi vào conservation và kiểm tra giao tiếp thì ta thấy ip này có kết nối tới 1 port rất lạ là 8080 (thực ra ko lạ lắm vì ta biết trước trong ida rồi) còn port 1337 ta biết là chỗ ip này down malware về

<img width="1632" height="142" alt="image" src="https://github.com/user-attachments/assets/642f4138-79dc-4ac8-994e-d033e8e8723d" />

sau đó ta filter theo tcp.port == 8080 thì thấy được campaign cần tìm

<img width="1917" height="1020" alt="image" src="https://github.com/user-attachments/assets/9156f1c4-a034-486b-bfc1-a6d9f7b560ba" />

-> có key đầy đủ và ta sẽ chuyển nó về dạng hex để giải mã, dạng hex của nó là 3c0249e672188f75d3f6a845d534ecfc1bdd2c7c681562393f636d24d4409830 
có key và iv ta có thể tiến hành giải mã qua cyberchef nhưng iv của ta chỉ là 12 byte tức nó ko phải dạng cbc mà là gcm do cbc là 16 byte lận, ngoài ra trong file còn có 1 dạng kiểu 8 byte magic tức malware thêm vào để nhận diện là file bị mã hóa +16 byte gcm tag( được tạo ra để check iv và key có đúng không) và cuối cùng phần còn lại là cyphertext
ta sẽ tiến hành lấy gcm tag tức start từ 8 rồi lấy 16 byte rồi chuyển đoạn đó sang dạng hex sau đó loại bỏ 24 byte trong file gốc và ta sẽ giải mã thành công

<img width="1167" height="551" alt="image" src="https://github.com/user-attachments/assets/b33c4fe9-96d3-44ff-936d-97efd3e820fb" />

<img width="1170" height="548" alt="image" src="https://github.com/user-attachments/assets/4b26a267-5681-4020-92f9-279e13b5cb7c" />

cơ mà file flag này thì lại ko có flag mất rồi nên ta cứ làm tương tự với mấy file kia rồi cũng sẽ ra flag thôi, flag mình tìm được nằm ở hi.txt.enc với nonce khác là 24c696f9341fd8e7882b44f0

<img width="1911" height="915" alt="image" src="https://github.com/user-attachments/assets/d6c10fe4-d240-4cf3-afe7-dd151bcefa35" />

->flag: CSCV2026{c0rr3l4t3_b3f0r3_d3crypt}
