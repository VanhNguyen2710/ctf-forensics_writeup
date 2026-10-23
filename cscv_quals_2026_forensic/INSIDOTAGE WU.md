`````
During a routine infrastructure audit, the security operations center detected abnormal resource spikes and suspicious outbound network traffic originating from an overlooked legacy server.

Internal triage revealed that a rogue system administrator had abused their elevated privileges to repurpose the host for unauthorized computing, browser-based cryptocurrency operations, and illicit personal activities. Realizing an investigation was imminent, the insider attempted to cover their tracks by wiping key directories and purging activity logs before the server was isolated.

Despite their anti-forensic efforts, residual artifacts were preserved by the incident response team. Analyze the remaining evidence to reconstruct the rogue administrator's illicit deployment and uncover the personal digital footprint left behind.
`````
ở đây theo đề bài ta biết được rằng một máy chủ cũ đã có sự bất thường về tài nguyên sử dụng rất có thể đây là dấu hiệu của một lại mã độc cryptominer. Theo điều tra ta biết được rằng có một admin đã lợi dụng quyền hạn và sử dụng máy chủ cho mục đích cá nhân trái phép cũng như các hoạt động liên quan tới tiền điện tử. Và anh ta đã nhận ra mình sắp bị điều tra nên tiến hành xóa log trước khi máy chủ bị cô lập.
Ở đây khi ta biết được rằng admin đã cố tình xóa đi các log hoạt động thì hướng điều tra sẽ thay đổi. Ta không tiến hành check log bth nằm trên ổ đĩa mà sẽ chuyển hướng ưu tiên điều tra qua ram.
Khi tiến hành điều tra ram và sử dụng công cụ volitality 3 thì mình nhận ra rằng đây là linux mem dump cụ thể hơn khi check header ta biết được nó là định dạng LiME!

````
┌──(venv)(vuanh㉿Vanh)-[/mnt/d/Data/chall/cscv/for]
└─$ vol -f mem.raw banners.Banners
Volatility 3 Framework 2.28.0
Progress:  100.00               PDB scanning finished
Offset  Banner

0xad000220      Linux version 5.14.0-22.el9.x86_64 (mockbuild@x86-06.stream.rdu2.redhat.com) (gcc (GCC) 11.2.1 20211019 (Red Hat 11.2.1-6), GNU ld version 2.35.2-10.el9) #1 SMP Fri Nov 26 23:47:21 UTC 2021
0xae729258      Linux version 5.14.0-22.el9.x86_64 (mockbuild@x86-06.stream.rdu2.redhat.com) (gcc (GCC) 11.2.1 20211019 (Red Hat 11.2.1-6), GNU ld version 2.35.2-10.el9) #1 SMP Fri Nov 26 23:47:21 UTC 202121
0x110515498     Linux version 5.14.0-22.el9.x86_64 (mockbuild@x86-06.stream.rdu2.redhat.com) (gcc (GCC) 11.2.1 20211019 (Red Hat 11.2.1-6), GNU ld version 2.35.2-10.el9) #1 SMP Fri Nov 26 23:47:21 UTC 2021
0x128200220     Linux version 5.14.0-22.el9.x86_64 (mockbuild@x86-06.stream.rdu2.redhat.com) (gcc (GCC) 11.2.1 20211019 (Red Hat 11.2.1-6), GNU ld version 2.35.2-10.el9) #1 SMP Fri Nov 26 23:47:21 UTC 2021
0x129929258     Linux version 5.14.0-22.el9.x86_64 (mockbuild@x86-06.stream.rdu2.redhat.com) (gcc (GCC) 11.2.1 20211019 (Red Hat 11.2.1-6), GNU ld version 2.35.2-10.el9) #1 SMP Fri Nov 26 23:47:21 UTC 202121
````

<img width="791" height="86" alt="image" src="https://github.com/user-attachments/assets/a1000cea-dec3-4083-8471-1d336534ca86" />

ta biết được đây là Linux 5.14.0-22.el9.x86_64 và muốn vol đọc và dùng thì ta cần có ISF tương ứng 
