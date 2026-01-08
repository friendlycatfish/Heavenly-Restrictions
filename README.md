# Heavenly-Restrictions


Dự án tập trung vào việc làm quen và thực hành các kỹ thuật lập trình hệ thống nhúng thông qua nền tảng chip **BW16 (RTL8720DN)** và **BW21 CBV**. Đây là không gian thực nghiệm để tìm hiểu cách một **vi điều khiển (MCU)** quản lý giao diện, xử lý nút bấm vật lý và tương tác trực tiếp với các giao thức kết nối phổ biến như **WiFi Dual Band (2.4GHz/5GHz)** và **Bluetooth**.

---

## 🛠 Linh kiện cần có

* **Vi điều khiển:** BW16 (RTL8720DN)
* **Hiển thị:** Màn hình OLED 0.96 inch (Giao tiếp I2C)
* **Điều khiển:** 03 nút nhấn (Tactile button)
* **(Tùy chọn):** Dây cắm (Jumper wires), Breadboard.
* **(Tùy chọn):** Điện trở pull-up bên ngoài nếu không sử dụng chế độ Pull-up nội bộ của chip.

---

## 🔌 Cách đấu nối

### 1. Đấu nối màn hình OLED (I2C)

Màn hình OLED hoạt động với mức điện áp 3.3V. 

**Lưu ý:** Không cấp nguồn 5V để tránh làm hỏng linh kiện.

| Chân OLED | Chân BW16 (RTL8720DN) |
| :--- | :--- |
| **VCC** | 3.3V |
| **GND** | GND |
| **SDA** | PA26|
| **SCL** | PA25 |

### 2. Đấu nối nút nhấn (Buttons)

Các nút nhấn được nối theo dạng **Active Low** (Một đầu nối chân GPIO, một đầu nối GND).

* **Nút UP:** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Chân **`PA27`**
* **Nút DOWN:** &nbsp;&nbsp;Chân **`PA12`**
* **Nút OK:** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Chân **`PA13`**

> **Khuyến nghị:** Trong mã nguồn, hãy cấu hình các chân này là `INPUT_PULLUP` để đảm bảo tín hiệu ổn định.

---

## 🚀 Chức năng chính

* **Hiển thị Menu:** Giao diện trực quan trên màn hình OLED.
* **Điều hướng linh hoạt:**
    * **UP:** Di chuyển thanh chọn lên trên.
    * **DOWN:** Di chuyển thanh chọn xuống dưới.
    * **OK:** Xác nhận lựa chọn.
* **Khả năng mở rộng:** Cấu trúc mã nguồn dễ dàng thêm bớt các mục menu hoặc gán chức năng mới.

---

## 📝 Ghi chú kỹ thuật

## 📥 Hướng dẫn nạp Code (Flash Firmware)

Để nạp code cho **BW16**, bạn cần thực hiện đưa chip vào chế độ nạp (Burn Mode) thủ công bằng cách phối hợp hai nút bấm trên board mạch. Dự án hỗ trợ nạp qua **Arduino IDE** hoặc **VS Code**.

### 1. Cách kích hoạt chế độ Burn Mode (Thủ công)
Thực hiện theo trình tự "giữ - nhấn - thả" sau đây:

* **Nhấn và giữ** nút `Burn` (nút nằm phía bên trái).

* Trong khi vẫn đang giữ nút `Burn`, hãy **nhấn và thả** nút `RST` (Reset).

* Cuối cùng, **thả tay** khỏi nút `Burn`. 

&emsp; *Lúc này chip đã sẵn sàng để nhận code từ máy tính.*



### 2. Sử dụng công cụ nạp
Bạn có thể linh hoạt sử dụng một trong hai phần mềm sau:

* **Arduino IDE:**
    * Chọn đúng Board: `Realtek RTL8720DN (BW16)`.
    * Chọn đúng cổng COM tương ứng.
    * Nhấn biểu tượng **Upload** (Mũi tên phải).
* **VS Code (PlatformIO / Arduino Extension):**
    * Đảm bảo cấu hình đúng tệp `platformio.ini` cho chip Realtek.
    * Nhấn biểu tượng **Upload** trên thanh trạng thái phía dưới.

---

### 💡 Mẹo nạp code tự động (Auto Flash)
Nếu bạn không muốn nhấn tổ hợp nút bấm thủ công mỗi lần nạp code, hãy thực hiện cài đặt sau trong **Arduino IDE**:

1.  Vào menu **Tools**.
2.  Tìm mục **Auto Flash Mode**.
3.  Chuyển từ `Disable` sang **`Enable`**.

&emsp; *Sau khi bật tính năng này, phần mềm sẽ tự động đưa chip vào chế độ nạp (Auto Boot) mà không cần bạn phải can thiệp vào phần cứng.*

---

### 📝 Lưu ý quan trọng
* **Sau khi nạp xong:** Nếu không dùng chế độ Auto Flash, bạn cần nhấn nút `RST` một lần nữa để khởi động chương trình vừa nạp.
* **Kiểm tra kết nối:** Đảm bảo cáp Micro-USB của bạn có hỗ trợ truyền dữ liệu (Data cable) thay vì chỉ sạc thuần túy.
