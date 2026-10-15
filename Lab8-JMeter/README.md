# LAB 8 – WEBSITE LOAD TESTING VỚI APACHE JMETER

## 1. Giới thiệu

### 1.1. Mục tiêu

Bài thực hành nhằm tìm hiểu và sử dụng Apache JMeter để kiểm thử tải (Load Testing) cho website và dịch vụ web thông qua các yêu cầu HTTP.

Các mục tiêu chính:
- Làm quen với giao diện và các thành phần cơ bản của Apache JMeter.
- Tạo kịch bản gửi HTTP Request bằng phương thức GET.
- Thiết lập số lượng người dùng ảo và thời gian chạy kiểm thử.
- Thu thập, phân tích kết quả thông qua View Results Tree và Aggregate Report.
- Đánh giá kết quả dựa trên thời gian phản hồi, số lượng request và tỷ lệ lỗi.

### 1.2. Công cụ sử dụng

- **Công cụ:** Apache JMeter 5.6.3
- **Giao thức:** HTTP/HTTPS
- **Phương thức:** GET
- **Các Listener:** View Results Tree, Aggregate Report

---

## 2. Kiểm thử tải website Phenikaa

### 2.1. Đối tượng kiểm thử

- **Website:** [https://phenikaa-uni.edu.vn/vi](https://phenikaa-uni.edu.vn/vi)
- **Phương thức HTTP:** GET
- **Content Encoding:** UTF-8

### 2.2. Cấu hình Thread Group

| Tham số | Giá trị |
|---|---:|
| Number of Threads (users) | 100 |
| Ramp-up Period | 10 giây |
| Loop Count | Infinite |
| Specify Thread lifetime | Bật |
| Duration | 60 giây |

### 2.3. Kết quả kiểm thử

Kết quả được ghi nhận từ Aggregate Report của Apache JMeter.

| Chỉ số | Kết quả |
|---|---:|
| Tổng số request | 4.088 |
| Average | 1.353 ms |
| Median | 466 ms |
| 90% Line | 1.454 ms |
| 95% Line | 10.061 ms |
| 99% Line | 10.195 ms |
| Min | 42 ms |
| Maximum | 10.628 ms |
| Error % | 99,05% |
| Throughput | 67,6 request/giây |

### 2.4. Nhận xét

Trong lần kiểm thử này JMeter ghi nhận 4.088 request với thời gian phản hồi trung bình là 1.353 ms. Thời gian phản hồi trung vị là 466 ms trong khi giá trị phân vị 95% đạt 10.061 ms.

Tỷ lệ lỗi được ghi nhận là 99,05% cho thấy phần lớn request trong lần chạy này không thành công. Trong View Results Tree một số request trả về mã HTTP 500 (Internal Server Error).

Do tỷ lệ lỗi cao nên chưa thể kết luận website đáp ứng tốt khi chịu tải. Cần kiểm tra thêm các mã lỗi và điều kiện kiểm thử để xác định nguyên nhân trước khi đưa ra đánh giá cuối cùng về hiệu năng.

---

## 3. Kiểm thử tải dịch vụ OpenWeather

### 3.1. Đối tượng kiểm thử

- **Tên cấu hình:** API_Load_Test
- **Server Name:** `openweathermap.org`
- **Path:** `/current`
- **Phương thức HTTP:** GET
- **Content Encoding:** UTF-8

### 3.2. Cấu hình Thread Group

| Tham số | Giá trị |
|---|---:|
| Number of Threads (users) | 100 |
| Ramp-up Period | 10 giây |
| Loop Count | 1 |
| Specify Thread lifetime | Bật |
| Duration | 60 giây |

### 3.3. Kết quả kiểm thử

| Chỉ số | Kết quả |
|---|---:|
| Tổng số request | 93.309 |
| Average | 59 ms |
| Median | 56 ms |
| 90% Line | 62 ms |
| 95% Line | 65 ms |
| 99% Line | 135 ms |
| Min | 49 ms |
| Maximum | 1.607 ms |
| Error % | 99,83% |
| Throughput | 1.544,5 request/giây |

### 3.4. Nhận xét

Trong lần chạy này JMeter ghi nhận 93.309 request. Thời gian phản hồi trung bình là 59 ms và trung vị là 56 ms. Tuy nhiên tỷ lệ lỗi đạt 99,83% cho thấy phần lớn request bị đánh dấu thất bại.

View Results Tree ghi nhận một số phản hồi HTTP 503 (Service Temporarily Unavailable). Vì vậy thời gian phản hồi thấp không đồng nghĩa với việc dịch vụ hoạt động tốt. Cần xác minh endpoint và kiểm tra nguyên nhân lỗi trước khi đánh giá hiệu năng của API.

---

## 4. Kết luận

Qua bài thực hành em đã làm quen với Apache JMeter 5.6.3 và biết cách tạo Thread Group, cấu hình HTTP Request, sử dụng các Listener để thu thập kết quả kiểm thử.

Kết quả thực nghiệm cho thấy cả hai kịch bản đều có tỷ lệ request lỗi cao. Vì vậy chưa đủ cơ sở để kết luận website hoặc dịch vụ hoạt động ổn định dưới tải. Các bước tiếp theo cần tập trung vào việc xác minh endpoint phân loại mã lỗi HTTP và kiểm thử lại trong phạm vi được phép.

Bài thực hành giúp em hiểu rằng khi đánh giá hiệu năng cần xem xét đồng thời thời gian phản hồi thông lượng và tỷ lệ lỗi thay vì chỉ dựa trên một chỉ số riêng lẻ.

---

## 5. Minh chứng

Các ảnh chụp màn hình được lưu trong thư mục `screenshots/`.

- `website_thread_group.png`: Cấu hình Thread Group cho website.
- `website_http_request.png`: Cấu hình HTTP Request cho website.
- `website_view_results_tree.png`: Kết quả request của website.
- `website_aggregate_report.png`: Báo cáo tổng hợp của website.
- `api_thread_group.png`: Cấu hình Thread Group cho OpenWeather.
- `api_http_request.png`: Cấu hình HTTP Request cho OpenWeather.
- `api_view_results_tree.png`: Kết quả request của OpenWeather.
- `api_aggregate_report.png`: Báo cáo tổng hợp của OpenWeather.

```