FOR04
author:nhoktiger12

đề bài cấp cho ta một file .ad1 khi ta connect vào netcat để bắt đầu trả lời câu hỏi, câu 1 của đề là

Q1: When was the Windows Data Deduplication feature first installed on the system?

khi một feature được cài vào hệ thống thì ta sẽ nghĩ ngay tới log của system.evtx, cụ thể hơn ta sẽ kiểm tra event id 7045 vì đây là event id được ghi lại khi cài một service hoặc driver mới. 

<img width="1917" height="1008" alt="image" src="https://github.com/user-attachments/assets/79cf4e11-70df-44c9-b9d7-f3c398206007" />

ta tìm được đáp án, đổi 16:56:40 của giờ việt nam ra utc ta sẽ có 09:56:40 và theo định dạng câu hỏi cần thêm ngày tháng năm theo format yyyy-mm-dd thì đáp án cần điền là 2025-09-18 09:56:40
->q1:2025-09-18 09:56:40

FOR01
author:KAiZ3n
An organization has detected signs of compromise on a workstation and a Domain Controller following a suspicious incident involving a document sent to a user.

The Incident Response (IR) team has collected forensic artifacts from both the endpoint and the Domain Controller.

Your task is to analyze the evidence, reconstruct the attack chain, and explain how the attacker pivoted from the compromised workstation into the Active Directory infrastructure.
