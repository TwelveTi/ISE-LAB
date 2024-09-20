<h1>Phan Dinh Trung - 22110083</h1>

# ctf.c LAB.

## - Stack frame:

![Screenshot 2024-09-20 181452](https://github.com/user-attachments/assets/ab484234-7098-4fc9-8cb0-3ac4019da18b)

- ## Tạo một file flag1.txt để khi chương trình chạy hàm myfunc sẽ không báo lỗi không đọc được file flag1.txt
- ## Tạo flag1.txt:

![Screenshot 2024-09-20 181833](https://github.com/user-attachments/assets/c8228149-3796-412b-8f99-87c5b78482a2)

- ## Vào gdb để lấy địa chỉ hàm myfunc, địa chỉ là: `0x0804851b`.

![Screenshot 2024-09-20 182208](https://github.com/user-attachments/assets/12cbbaac-f22d-47a2-a068-ec5254ea3395)

- ## Hàm myfunc có hai tham số đầu vào là p và q nhưng cả chương trình lại không có truyền giá trị cho 2 biến này. Vì vậy ta sẽ đặt break tại hàm myfunc để có thể đặt giá trị cho p và q lần lượt là: `0x04081211` và `0x44644262`:

![Screenshot 2024-09-20 182335](https://github.com/user-attachments/assets/6c499be9-ef5b-422f-9962-ea04795e33c2)

![Screenshot 2024-09-20 183532](https://github.com/user-attachments/assets/b883671b-f10e-4f5a-893a-2861da9b8bfb)

- ## Chạy chương trình với dòng lệnh `r $(python -c "print('a'*104+'\x1b\x85\x04\x08')")` để đặt điều khiển return address của hàm vuln điều khiển đi đến hàm `myfunc`. Sau đó đặt giá trị cho p và q thông qua `[ebp+8]=0x04081211` và `[ebp+12]=0x44644262`. Hoàn thành nhiệm vụ đề bài:

![Screenshot 2024-09-20 183927](https://github.com/user-attachments/assets/a38d9ac2-3ea8-43d8-ab3d-f4abf4aaac96)

![Screenshot 2024-09-20 190001](https://github.com/user-attachments/assets/19d3e731-3d54-4642-9af5-9a8efa162074)

![Screenshot 2024-09-20 190139](https://github.com/user-attachments/assets/45aac15f-4fec-4d1e-8c1e-79cab1f8c3d3)
