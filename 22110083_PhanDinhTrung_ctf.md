<h1>Phan Dinh Trung - 22110083</h1>

# ctf.c LAB.

## - Stack frame:

![Screenshot 2024-09-20 181452](https://github.com/user-attachments/assets/5a59b865-324b-47da-a314-3e57536cac19)

- ## Tạo một file flag1.txt để khi chương trình chạy hàm myfunc sẽ không báo lỗi không đọc được file flag1.txt
- ## Tạo flag1.txt:

![Screenshot 2024-09-20 181833](https://github.com/user-attachments/assets/534f448d-65eb-4989-881d-a22035e7e273)

- ## Vào gdb để lấy địa chỉ hàm myfunc, địa chỉ là: `0x0804851b`.

![Screenshot 2024-09-20 182208](https://github.com/user-attachments/assets/7632868a-9957-43a8-8f14-26cc72fd3e0c)

- ## Hàm myfunc có hai tham số đầu vào là p và q nhưng cả chương trình lại không có truyền giá trị cho 2 biến này. Vì vậy ta sẽ đặt break tại hàm myfunc để có thể đặt giá trị cho p và q lần lượt là: `0x04081211` và `0x44644262`:

![Screenshot 2024-09-20 182335](https://github.com/user-attachments/assets/a3310751-3711-4cf9-96ea-743e70a9a92c)

![Screenshot 2024-09-20 183532](https://github.com/user-attachments/assets/e9b1fdb1-8c8b-478e-af2b-09b832c0ab5c)

- ## Chạy chương trình với dòng lệnh `r $(python -c "print('a'*104+'\x1b\x85\x04\x08')")` để đặt điều khiển return address của hàm vuln điều khiển đi đến hàm `myfunc`. Sau đó đặt giá trị cho p và q thông qua `[ebp+8]=0x04081211` và `[ebp+12]=0x44644262`. Hoàn thành nhiệm vụ đề bài:

![Screenshot 2024-09-20 183927](https://github.com/user-attachments/assets/d9a3a687-f6c7-4a84-9bfa-2be88be75df3)

![Screenshot 2024-09-20 190001](https://github.com/user-attachments/assets/9b65e21c-7287-40a5-9a15-c319b4e86c4f)

![Screenshot 2024-09-20 190139](https://github.com/user-attachments/assets/5125a8f7-6f7c-4e72-8305-27d321a617dd)
