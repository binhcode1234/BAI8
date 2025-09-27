# Dự án STM32F103C8T6: Đọc ADC và Gửi Dữ Liệu Qua UART

## 1. Giới thiệu

Dự án này được viết bằng ngôn ngữ C sử dụng **Standard Peripheral Library (SPL)** cho vi điều khiển **STM32F103C8T6**.
Chương trình thực hiện:

* Cấu hình **UART1** để truyền dữ liệu (TX: PA9, RX: PA10).
* Cấu hình **ADC1** để đọc giá trị từ kênh ADC0 (chân PA0).
* Chuyển đổi giá trị ADC sang điện áp (mV).
* Gửi dữ liệu ADC và điện áp đọc được qua UART về máy tính (qua cổng Serial).

## 2. Yêu cầu phần cứng

* **STM32F103C8T6** (Blue Pill hoặc custom board).
* **Nguồn 3.3V** ổn định.
* **Cổng USB-UART (FTDI/CH340/CP2102)** để kết nối UART1 với máy tính.
* **Điện trở chia áp / cảm biến analog** nối vào chân **PA0 (ADC Channel 0)**.
* **Thạch anh ngoài 8 MHz** (nếu dùng board Blue Pill).

### Sơ đồ chân:

* **PA9 (TX)** → RX của USB-UART.
* **PA10 (RX)** → TX của USB-UART.
* **PA0 (ADC0)** → Tín hiệu analog (0–3.3V).

## 3. Cấu hình phần mềm

* **IDE**: Keil uVision, STM32CubeIDE, hoặc PlatformIO.
* **Thư viện**: Standard Peripheral Library (SPL) cho STM32F10x.
* **Baudrate UART**: 9600 bps.
* **ADC**: Độ phân giải 12 bit, Vref = 3.3V, tần số lấy mẫu = 55.5 chu kỳ.

## 4. Chức năng chính

### UART1

* Khởi tạo UART1 với tốc độ baud tùy chọn (mặc định 9600).
* Gửi ký tự, chuỗi sang PC qua Serial.

### ADC1

* Khởi tạo ADC1 ở chế độ **Independent**, đọc kênh **ADC0 (PA0)**.
* Hiệu chuẩn ADC trước khi đọc giá trị.
* Hàm `Read_ADC()` trả về giá trị 12-bit (0–4095).

### Main Loop

* Đọc giá trị ADC.

* Tính điện áp theo công thức:

  ```
  voltage (mV) = (adc_value * 3300.0) / 4095.0
  ```

* Gửi kết quả qua UART theo định dạng:

  ```
  ADC: 1234 | Voltage: 995.43 mV
  ```

* Lặp lại sau một khoảng delay ngắn.

## 5. Cách sử dụng

1. Nạp chương trình vào STM32F103C8T6.
2. Kết nối board với USB-UART (TX ↔ RX, RX ↔ TX, GND ↔ GND).
3. Mở **Serial Monitor** trên PC (baudrate: 9600).
4. Quan sát dữ l
