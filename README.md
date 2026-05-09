# ESP32 Mini Gateway Education

> Tài liệu giới thiệu dành cho người dùng

## 1. ESP32 Mini Gateway Education là gì?

**ESP32 Mini Gateway Education** là một bộ gateway học tập giúp bạn kết nối cảm biến hoặc các node IoT nhỏ với cloud thông qua WiFi và MQTT.

Bạn có thể hiểu đơn giản như sau:

```text
Cảm biến / Sensor Node
→ ESP32 Mini Gateway
→ WiFi
→ MQTT
→ Dashboard IoT
```

Gateway đóng vai trò như một “trạm trung gian”. Nó nhận dữ liệu từ cảm biến hoặc node không dây, sau đó gửi dữ liệu đó lên cloud để bạn có thể xem trên dashboard, tạo cảnh báo hoặc lưu lại lịch sử dữ liệu.

---

## 2. Thiết bị này dùng để làm gì?

ESP32 Mini Gateway Education phù hợp để học và thực hành các bài toán IoT cơ bản như:

* Đọc dữ liệu từ cảm biến.
* Gửi dữ liệu từ ESP32 lên MQTT broker.
* Kết nối nhiều node cảm biến về một gateway.
* Làm dashboard theo dõi dữ liệu.
* Tạo cảnh báo khi có sự kiện xảy ra.
* Hiểu mô hình Sensor → Gateway → Cloud.

Một số ví dụ thực tế:

```text
Cảm biến cửa mở
→ Gateway
→ Gửi cảnh báo
```

```text
Cảm biến nhiệt độ
→ Gateway
→ Hiển thị biểu đồ nhiệt độ
```

```text
Node đo độ ẩm đất ngoài vườn
→ Gateway
→ Theo dõi trên dashboard
```

---

## 3. Ai nên sử dụng?

Tài liệu và bộ code này phù hợp cho:

* Người mới học ESP32.
* Maker muốn học IoT thực tế.
* Học sinh, sinh viên làm bài lab hoặc đồ án nhỏ.
* Giáo viên muốn dạy bài học Sensor to Cloud.
* Người muốn thử nghiệm ý tưởng gateway trước khi làm sản phẩm lớn hơn.

Bạn không cần phải là chuyên gia IoT để bắt đầu. Chỉ cần biết cơ bản về ESP32, Arduino IDE hoặc PlatformIO là có thể thực hành.

---

## 4. Gateway hỗ trợ những chế độ nào?

ESP32 Mini Gateway Education hỗ trợ 3 chế độ học tập chính:

```text
Mode 1: GPIO Sensor
Mode 2: BLE Sensor Node
Mode 3: NRF Sensor Node
```

Ở phiên bản học tập, gateway chỉ cần chạy **một chế độ tại một thời điểm** để dễ hiểu và dễ debug.

---

# 5. Mode 1 – GPIO Sensor

## 5.1. GPIO Sensor Mode là gì?

Ở chế độ này, bạn kết nối cảm biến trực tiếp vào các chân GPIO của ESP32.

Ví dụ:

```text
Cảm biến cửa
→ Chân GPIO của ESP32
→ Gateway đọc trạng thái
→ Gửi dữ liệu lên MQTT
```

Đây là chế độ dễ nhất để bắt đầu vì bạn chỉ cần một ESP32 và một cảm biến đơn giản.

---

## 5.2. Có thể dùng cảm biến nào?

Một số cảm biến phù hợp:

| Cảm biến             | Mục đích                     |
| -------------------- | ---------------------------- |
| Button               | Kiểm tra trạng thái nhấn/thả |
| Magnetic Door Sensor | Phát hiện cửa mở/đóng        |
| PIR Motion Sensor    | Phát hiện chuyển động        |
| DHT11/DHT22          | Đo nhiệt độ, độ ẩm           |
| LDR                  | Đo ánh sáng                  |
| MQ-2/MQ-135          | Đo khí gas/không khí cơ bản  |
| Soil Moisture Sensor | Đo độ ẩm đất                 |

---

## 5.3. Ví dụ: cảm biến cửa

### Kết nối phần cứng

```text
Door Sensor VCC  → 3.3V
Door Sensor GND  → GND
Door Sensor OUT  → GPIO 14
```

### Dữ liệu gửi lên MQTT

```json
{
  "gateway_id": "gw_edu_001",
  "mode": "gpio",
  "source_id": "door_sensor_01",
  "source_type": "door_sensor",
  "metrics": {
    "door_open": true
  }
}
```

### Kết quả mong muốn

Khi cửa mở hoặc đóng, bạn có thể thấy trạng thái thay đổi trên Serial Monitor và dashboard.

```text
Door Sensor: Open
Door Sensor: Closed
```

---

# 6. Mode 2 – BLE Sensor Node

## 6.1. BLE Sensor Node Mode là gì?

Ở chế độ này, một ESP32 khác sẽ đóng vai trò là **BLE Sensor Node**. Node này đọc dữ liệu từ cảm biến, sau đó gửi dữ liệu về gateway thông qua Bluetooth Low Energy.

Mô hình hoạt động:

```text
ESP32 Sensor Node
→ Gửi dữ liệu qua BLE
→ ESP32 Mini Gateway nhận dữ liệu
→ Gateway gửi dữ liệu lên MQTT
```

---

## 6.2. Khi nào nên dùng BLE Sensor Node?

Bạn nên dùng chế độ BLE khi:

* Muốn đặt sensor ở vị trí không tiện kéo dây về gateway.
* Muốn một gateway nhận dữ liệu từ nhiều ESP32 node nhỏ.
* Muốn học cách ESP32 giao tiếp không dây với nhau.
* Muốn làm bài lab về BLE advertising.

Ví dụ:

```text
ESP32 BLE Node + DHT22
→ Gửi nhiệt độ/độ ẩm qua BLE
→ Gateway nhận
→ Dashboard hiển thị dữ liệu
```

---

## 6.3. Cách gửi dữ liệu đơn giản

Trong phiên bản học tập, node có thể gửi dữ liệu BLE dưới dạng text đơn giản để dễ debug.

Ví dụ payload:

```text
IOTLABS,TEMP_HUM,node_01,28.5,68,92
```

Ý nghĩa:

| Phần     | Ý nghĩa                          |
| -------- | -------------------------------- |
| IOTLABS  | Prefix để gateway nhận biết node |
| TEMP_HUM | Loại dữ liệu                     |
| node_01  | ID của node                      |
| 28.5     | Nhiệt độ                         |
| 68       | Độ ẩm                            |
| 92       | Mức pin                          |

Gateway sẽ nhận payload này, chuyển thành JSON và gửi lên MQTT.

Ví dụ:

```json
{
  "gateway_id": "gw_edu_001",
  "mode": "ble",
  "source_id": "node_01",
  "source_type": "temp_hum",
  "metrics": {
    "temperature": 28.5,
    "humidity": 68,
    "battery": 92
  },
  "rssi": -62
}
```

---

## 6.4. Kết quả mong muốn

Sau khi chạy thành công, bạn sẽ thấy gateway log ra dữ liệu tương tự:

```text
[BLE] Found IoTLabs node: node_01
[BLE] Temperature: 28.5
[BLE] Humidity: 68
[MQTT] Publish success
```

Trên dashboard, bạn có thể xem nhiệt độ, độ ẩm và trạng thái node.

---

# 7. Mode 3 – NRF Sensor Node

## 7.1. NRF Sensor Node Mode là gì?

Ở chế độ này, gateway nhận dữ liệu từ các node sử dụng module **nRF24L01**.

nRF24L01 là module truyền dữ liệu không dây giá rẻ, thường dùng trong các dự án Arduino/ESP32 để gửi dữ liệu giữa các thiết bị.

Mô hình hoạt động:

```text
Sensor Node + nRF24L01
→ Gửi dữ liệu không dây
→ Gateway + nRF24L01 nhận dữ liệu
→ Gateway gửi dữ liệu lên MQTT
```

---

## 7.2. Khi nào nên dùng NRF Sensor Node?

Bạn nên dùng NRF khi:

* Muốn học giao tiếp không dây giữa nhiều node.
* Muốn dùng các node cảm biến giá rẻ.
* Muốn làm mạng sensor nhỏ trong nhà, lớp học, vườn mini.
* Muốn thử nghiệm truyền dữ liệu không dây ngoài BLE.

Ví dụ:

```text
Node đo độ ẩm đất
→ nRF24L01
→ Gateway
→ Dashboard theo dõi độ ẩm đất
```

---

## 7.3. Phần cứng cần chuẩn bị

### Gateway

* 1 ESP32 DevKit.
* 1 module nRF24L01.
* Dây jumper.
* Tụ 10uF–100uF gắn gần module nRF24L01.

### Sensor Node

* 1 Arduino Nano hoặc ESP32 khác.
* 1 module nRF24L01.
* 1 cảm biến mẫu, ví dụ soil moisture sensor hoặc DHT22.
* Nguồn cấp phù hợp.

---

## 7.4. Kết nối nRF24L01 với ESP32 Gateway

Ví dụ wiring:

```text
nRF24L01 VCC  → 3.3V
nRF24L01 GND  → GND
nRF24L01 CE   → GPIO 4
nRF24L01 CSN  → GPIO 5
nRF24L01 SCK  → GPIO 18
nRF24L01 MOSI → GPIO 23
nRF24L01 MISO → GPIO 19
```

Lưu ý quan trọng:

```text
Không cấp 5V trực tiếp cho nRF24L01.
Module này dùng 3.3V.
```

Nên gắn thêm tụ gần chân VCC/GND của module nRF24L01 để tín hiệu ổn định hơn.

---

## 7.5. Payload NRF đơn giản

Node có thể gửi dữ liệu dạng struct đơn giản:

```cpp
struct NrfSensorPayload {
  char nodeId[16];
  char type[16];
  float value1;
  float value2;
  int battery;
};
```

Ví dụ dữ liệu:

```text
nodeId: nrf_soil_01
type: soil_temp
value1: 45
value2: 29.1
battery: 87
```

Gateway chuyển thành JSON:

```json
{
  "gateway_id": "gw_edu_001",
  "mode": "nrf",
  "source_id": "nrf_soil_01",
  "source_type": "soil_temp",
  "metrics": {
    "soil_moisture": 45,
    "temperature": 29.1,
    "battery": 87
  }
}
```

---

## 7.6. Lỗi thường gặp với nRF24L01

nRF24L01 khá nhạy với nguồn và dây nối. Nếu gateway không nhận được dữ liệu, hãy kiểm tra:

* Module đã cấp đúng 3.3V chưa.
* Đã gắn tụ gần module chưa.
* CE và CSN có đúng chân không.
* SCK/MOSI/MISO có đúng chân SPI không.
* Node gửi và gateway nhận có cùng address/channel không.
* Dây nối có quá dài hoặc lỏng không.
* Hai module có đặt quá xa nhau không.

---

# 8. Cách chọn mode

Trong bản học tập, bạn chọn mode bằng file cấu hình trong code.

Ví dụ:

```cpp
#define MODE_GPIO 1
#define MODE_BLE 2
#define MODE_NRF 3

#define GATEWAY_MODE MODE_GPIO
```

Khi muốn chuyển sang BLE:

```cpp
#define GATEWAY_MODE MODE_BLE
```

Khi muốn chuyển sang NRF:

```cpp
#define GATEWAY_MODE MODE_NRF
```

Cách này đơn giản và phù hợp cho người học vì bạn có thể thấy rõ code đang chạy mode nào.

---

# 9. MQTT là gì trong dự án này?

MQTT là giao thức nhẹ thường dùng trong IoT để gửi dữ liệu từ thiết bị lên server/cloud.

Trong dự án này:

```text
ESP32 Mini Gateway = MQTT Client
MQTT Broker = Server nhận dữ liệu
Dashboard = Nơi hiển thị dữ liệu
```

Gateway sẽ publish dữ liệu lên các topic MQTT.

Ví dụ topic:

```text
iotlabs/{orgId}/gateways/{gatewayId}/telemetry
```

Ví dụ topic cho từng sensor/node:

```text
iotlabs/{orgId}/gateways/{gatewayId}/devices/{sourceId}/telemetry
```

---

# 10. Payload dữ liệu chuẩn

Dù dữ liệu đến từ GPIO, BLE hay NRF, gateway nên gửi lên cloud theo một format thống nhất.

Ví dụ payload chuẩn:

```json
{
  "version": "1.0",
  "gateway_id": "gw_edu_001",
  "mode": "gpio",
  "source_id": "door_sensor_01",
  "source_type": "door_sensor",
  "metrics": {
    "door_open": true
  },
  "ts": 1710000000
}
```

Ý nghĩa:

| Field       | Ý nghĩa                              |
| ----------- | ------------------------------------ |
| version     | Phiên bản định dạng dữ liệu          |
| gateway_id  | Mã gateway                           |
| mode        | Chế độ đang chạy: gpio, ble hoặc nrf |
| source_id   | Mã sensor hoặc node                  |
| source_type | Loại sensor hoặc node                |
| metrics     | Dữ liệu chính                        |
| ts          | Thời gian gửi dữ liệu                |

---

# 11. Trạng thái Gateway

Ngoài dữ liệu cảm biến, gateway cũng nên gửi trạng thái hoạt động định kỳ.

Ví dụ:

```json
{
  "gateway_id": "gw_edu_001",
  "mode": "ble",
  "wifi_rssi": -55,
  "uptime": 3600,
  "free_heap": 120000,
  "online": true
}
```

Các thông tin này giúp bạn biết gateway còn hoạt động hay không.

---

# 12. Cấu hình cơ bản

Ví dụ file cấu hình:

```cpp
#define WIFI_SSID "YOUR_WIFI"
#define WIFI_PASSWORD "YOUR_PASSWORD"

#define MQTT_HOST "mqtt.iotlabs.vn"
#define MQTT_PORT 1883
#define MQTT_USERNAME "YOUR_USERNAME"
#define MQTT_PASSWORD "YOUR_PASSWORD"

#define ORG_ID "org_demo"
#define GATEWAY_ID "gw_edu_001"

#define GATEWAY_MODE MODE_GPIO

#define STATUS_INTERVAL_MS 30000
#define TELEMETRY_INTERVAL_MS 5000
```

Bạn chỉ cần thay WiFi, MQTT account và mode muốn chạy.

---

# 13. Log trên Serial Monitor

Khi chạy gateway, bạn nên mở Serial Monitor để xem gateway đang làm gì.

Ví dụ khi khởi động:

```text
[BOOT] ESP32 Mini Gateway Education
[BOOT] Mode: GPIO
[WIFI] Connecting...
[WIFI] Connected. IP: 192.168.1.20
[MQTT] Connecting...
[MQTT] Connected
[GPIO] Door sensor initialized
```

Ví dụ khi gửi dữ liệu:

```text
[GPIO] door_open=true
[MQTT] Publish telemetry success
```

Ví dụ khi lỗi:

```text
[WIFI] Disconnected. Reconnecting...
[MQTT] Publish failed
[BLE] Invalid payload format
[NRF] No data received
```

Serial log là công cụ debug quan trọng nhất cho bản Education.

---

# 14. Cần chuẩn bị những gì?

## 14.1. Bộ cơ bản

| Thiết bị                | Ghi chú                 |
| ----------------------- | ----------------------- |
| ESP32 DevKit            | Dùng làm gateway        |
| Dây USB                 | Nạp code và cấp nguồn   |
| Breadboard              | Thực hành đấu nối       |
| Dây jumper              | Kết nối cảm biến        |
| Button hoặc door sensor | Demo GPIO               |
| DHT11/DHT22             | Demo nhiệt độ/độ ẩm     |
| nRF24L01 x 2            | Demo NRF                |
| ESP32 hoặc Arduino phụ  | Làm BLE/NRF sensor node |

## 14.2. Phần mềm

Bạn cần cài một trong hai công cụ:

* Arduino IDE
* PlatformIO trong VS Code

Ngoài ra, bạn có thể dùng MQTTX để kiểm tra dữ liệu MQTT nếu muốn debug độc lập.

---

# 15. Ba bài demo nên thực hành đầu tiên

## Demo 1 – Cảm biến cửa qua GPIO

Mục tiêu:

```text
Door Sensor
→ ESP32 Gateway
→ MQTT
→ Dashboard
```

Bạn sẽ học được:

* Cách đọc digital input.
* Cách tạo payload JSON.
* Cách publish MQTT.
* Cách xem trạng thái cửa trên dashboard.

---

## Demo 2 – ESP32 BLE Node gửi nhiệt độ

Mục tiêu:

```text
ESP32 BLE Node + DHT22
→ BLE Advertising
→ ESP32 Gateway
→ MQTT
→ Dashboard
```

Bạn sẽ học được:

* Cách một ESP32 gửi dữ liệu qua BLE.
* Cách gateway scan BLE.
* Cách decode dữ liệu BLE.
* Cách gửi dữ liệu node lên cloud.

---

## Demo 3 – NRF Soil Sensor Node

Mục tiêu:

```text
Soil Sensor Node + nRF24L01
→ Gateway + nRF24L01
→ MQTT
→ Dashboard
```

Bạn sẽ học được:

* Cách dùng nRF24L01.
* Cách truyền dữ liệu không dây giữa node và gateway.
* Cách xử lý payload từ node.
* Cách theo dõi dữ liệu độ ẩm đất.

---

# 16. Lộ trình học đề xuất

Bạn nên học theo thứ tự sau:

## Bài 1 – Hiểu mô hình Gateway

* Sensor là gì?
* Node là gì?
* Gateway là gì?
* MQTT là gì?
* Cloud dashboard là gì?

## Bài 2 – Cài đặt ESP32 Gateway

* Cài Arduino IDE hoặc PlatformIO.
* Nạp code đầu tiên.
* Kết nối WiFi.
* Kết nối MQTT.

## Bài 3 – GPIO Sensor Mode

* Đấu button hoặc door sensor.
* Đọc trạng thái ON/OFF.
* Gửi dữ liệu lên MQTT.

## Bài 4 – DHT Sensor

* Đọc nhiệt độ/độ ẩm.
* Gửi dữ liệu định kỳ.
* Xem dữ liệu trên dashboard.

## Bài 5 – BLE Sensor Node

* Tạo ESP32 BLE node.
* Gửi dữ liệu qua BLE advertising.
* Gateway scan và nhận dữ liệu.

## Bài 6 – NRF Sensor Node

* Kết nối nRF24L01.
* Gửi dữ liệu từ node.
* Gateway nhận và publish MQTT.

## Bài 7 – Tạo cảnh báo

* Cảnh báo cửa mở.
* Cảnh báo nhiệt độ cao.
* Cảnh báo độ ẩm đất thấp.

## Bài 8 – Tổng kết

* Khi nào dùng GPIO?
* Khi nào dùng BLE?
* Khi nào dùng NRF?
* Cách mở rộng thêm sensor mới.

---

# 17. Cấu trúc project gợi ý

```text
esp32-mini-gateway-education/
  README.md
  platformio.ini
  docs/
    01-setup.md
    02-gpio-mode.md
    03-ble-mode.md
    04-nrf-mode.md
    05-mqtt.md
    06-troubleshooting.md
  firmware/
    src/
      main.cpp
      config.h
      wifi_manager.cpp
      mqtt_manager.cpp
      mode_manager.cpp
      gpio_mode.cpp
      ble_mode.cpp
      nrf_mode.cpp
      telemetry.cpp
      logger.cpp
    examples/
      ble_sensor_node/
      nrf_sensor_node/
      gpio_door_sensor/
  diagrams/
    gpio-door-sensor.png
    ble-node-flow.png
    nrf-wiring.png
```

Cấu trúc này giúp bạn dễ tìm code, tài liệu và ví dụ cho từng chế độ.

---

# 18. Những giới hạn cần biết

Đây là phiên bản học tập, vì vậy có một số giới hạn:

* Chưa tối ưu cho chạy 24/7 trong môi trường thực tế.
* Chưa hỗ trợ nhiều node số lượng lớn.
* Chưa có cấu hình qua app hoặc QR code.
* Chưa có cập nhật firmware từ xa.
* Chưa có cơ chế bảo mật nâng cao.
* BLE payload dùng format đơn giản để dễ học.
* NRF có thể không ổn định nếu nguồn hoặc dây nối không tốt.

Các giới hạn này là bình thường với bản học tập. Mục tiêu chính là giúp bạn hiểu nguyên lý và chạy được demo.

---

# 19. Kết quả sau khi hoàn thành

Sau khi học và thực hành xong, bạn sẽ có thể:

* Biết cách biến ESP32 thành một mini gateway.
* Đọc dữ liệu từ cảm biến GPIO.
* Nhận dữ liệu từ ESP32 BLE node.
* Nhận dữ liệu từ node dùng nRF24L01.
* Gửi dữ liệu lên MQTT broker.
* Chuẩn hóa dữ liệu thành JSON.
* Xem dữ liệu trên dashboard.
* Debug lỗi cơ bản qua Serial Monitor.
* Tự mở rộng thêm sensor hoặc node mới.

---

# 20. Kết luận

ESP32 Mini Gateway Education là một dự án phù hợp để bắt đầu học IoT theo hướng thực tế.

Thay vì chỉ học từng cảm biến riêng lẻ, bạn sẽ học được một mô hình hoàn chỉnh hơn:

```text
Sensor
→ Node
→ Gateway
→ MQTT
→ Cloud
→ Dashboard
```

Ba chế độ GPIO, BLE và NRF giúp bạn đi từ dễ đến khó:

```text
GPIO: dễ nhất, cắm sensor trực tiếp
BLE: không dây với ESP32 node
NRF: mạng sensor node giá rẻ dùng nRF24L01
```

Khi hiểu được dự án này, bạn sẽ có nền tảng tốt để xây dựng các hệ thống IoT thực tế như cảnh báo cửa, giám sát nhiệt độ, theo dõi độ ẩm đất, lớp học STEM IoT hoặc các prototype thông minh khác.
