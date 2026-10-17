

# Bài 5 : Xây dựng bộ tài liệu kỹ thuật cho dự án Smart Farm

> Hệ thống tự động hóa nông nghiệp thông minh ứng dụng công nghệ IoT cho trang trại Lan.

---

## 1. Phân tích hệ thống (I/O & Phần cứng)
- **Input:** Dữ liệu thô từ cảm biến nhiệt độ, độ ẩm đất (`analog_pin`).
- **Output:** Giao diện giám sát trực quan và lệnh kích hoạt máy bơm nước tự động.
- **Sơ đồ phối hợp phần cứng:** 
  ![Sơ đồ hệ thống Smart Farm](https://rikkeiedu-storage.s3.ap-southeast-2.amazonaws.com/uploads/editor-images/1787648283613_687ec2e7-229c-4942-a347-080fae59560a.webp)

---

## 2. Hướng dẫn cài đặt (`Code Block`)
Đoạn mã cấu hình hệ thống vi điều khiển (SoC):
```python
# Cấu hình cảm biến và ngưỡng kích hoạt
SENSOR_PIN = 34
TEMP_THRESHOLD = 32.5

def check_environment(temp):
    if temp > TEMP_THRESHOLD:
        print("[ALARM] Nhiệt độ cao, bật hệ thống phun sương!")
    else:
        print("[STATUS] Môi trường ổn định.")
```
## 3. Quy trình nghiệp vụ (Fetch–Decode–Execute)

> Khi CPU nhận lệnh từ cảm biến nhiệt độ để điều khiển máy bơm, chu kỳ hoạt động diễn ra như sau:
> 
> 1.  **Fetch (Lấy lệnh):** CPU tải lệnh đọc dữ liệu từ bộ nhớ RAM.
>     
> 2.  **Decode (Giải mã):** Bộ điều khiển dịch lệnh để xác định tác vụ cần thực hiện (kiểm tra nhiệt độ).
>     
> 3.  **Execute (Thực thi):** Kích hoạt phần cứng đo đạc và đưa ra quyết định đóng/mở rơ-le máy bơm.
>     

## 4. Mô hình tháp tri thức (DIKW)

-   **Data (Dữ liệu):** `35°C` (con số nhiệt độ thô từ cảm biến).
    
-   **Information (Thông tin):** `Nhiệt độ hiện tại vượt mức tối ưu cho lan (32.5°C).`
    
-   **Knowledge (Tri thức):** `Trời quá nóng sẽ làm khô giá thể, cần tưới nước giảm nhiệt ngay lập tức.`
    
-   **Wisdom (Trí tuệ):** `Tự động lập lịch tưới định kỳ vào các khung giờ cao điểm để tối ưu hóa sự phát triển của cây.`

## 5. Bảng thông số kỹ thuật

|**Linh kiện**|**Tên thiết bị**|**Thông số chính**|
| --- | ---| ---|
|**SoC**|ESP32 NodeMCU|Wi-Fi / Bluetooth, Dual-core|
|**Sensor**|Capacitive Soil Moisture|Chống ăn mòn, điện dung|
|**Actuator**|Relay 5V & Máy bơm mini|12V DC, lưu lượng ổn định|

## 6. Tính năng & Kiểm thử hệ thống (Task List)

-   [x] Kết nối nguồn điện và module vi điều khiển SoC
    
-   [x] Cố định cảm biến độ ẩm vào giá thể trồng lan
    
-   [ ] Kiểm thử đường ống dẫn nước tự động
    
-   [x] **Xử lý sự cố (Edge Case):** RAM bị đầy dữ liệu nhật ký (log), hệ thống sẽ tự động ghi đè dữ liệu cũ (FIFO) hoặc nén log gửi về Cloud để giải phóng bộ nhớ cục bộ, đảm bảo vi điều khiển không bị treo.
