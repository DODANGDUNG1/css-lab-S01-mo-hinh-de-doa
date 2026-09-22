# Lab S1: Mô hình Đe dọa - Hệ thống Điểm danh Sinh viên UEH

## 1. Giới thiệu hệ thống
Hệ thống được chọn để phân tích mô hình đe dọa là **Hệ thống Điểm danh Sinh viên UEH**, bao gồm 3 thành phần cốt lõi:
- **C1 - Trình duyệt Người dùng (Frontend):** Giao diện web/mobile cho sinh viên thực hiện điểm danh.
- **C2 - Máy chủ Ứng dụng API (Backend):** Xử lý logic nghiệp vụ, tiếp nhận yêu cầu điểm danh và xác thực phiên làm việc.
- **C3 - Cơ sở dữ liệu (Database):** Lưu trữ thông tin tài khoản, danh sách lớp học và lịch sử điểm danh.

---

## 2. Các mối đe dọa được chọn xử lý và ước lượng chi phí
Dựa trên việc tính toán mức độ rủi ro (Tác động $\times$ Khả năng), nhóm/cá nhân đã lựa chọn **3 mối đe dọa trọng tâm** để đưa ra phương án xử lý theo nguyên lý thứ bảy (phòng thủ chủ động, tối ưu chi phí):

1. **Mối đe dọa T01 (Tampering - T1190):** Giả mạo yêu cầu điểm danh bằng cách sửa đổi gói tin HTTP.
   - *Phát biểu kiểm được:* Một sinh viên cố tình gửi yêu cầu điểm danh giả mạo bằng cách sửa đổi gói tin HTTP nếu hệ thống không xác thực token phiên làm việc.
   - *Ước lượng chi phí xử lý:* **2 triệu VNĐ** (dựa trên chi phí nhân công cấu hình middleware kiểm tra token xác thực trên toàn bộ API).

2. **Mối đe dọa T02 (Elevation of Privilege - T1539):** Đánh cắp phiên làm việc do thiếu cờ bảo mật HttpOnly trên cookie.
   - *Phát biểu kiểm được:* Kẻ tấn công đánh cắp phiên đăng nhập của giảng viên khi hệ thống lưu trữ cookie phiên làm việc mà không bật cờ bảo mật HttpOnly.
   - *Ước lượng chi phí xử lý:* **1.5 triệu VNĐ** (dựa trên thời gian cấu hình lại cờ bảo mật cho hệ thống cookie phản hồi).

3. **Mối đe dọa T03 (Information Disclosure - T1590):** Lộ thông tin sinh viên qua lỗ hổng SQL Injection trong truy vấn.
   - *Phát biểu kiểm được:* Hệ thống làm lộ thông tin nhạy cảm của sinh viên qua lỗi câu lệnh SQL Injection trong chức năng tìm kiếm lịch sử điểm danh nếu thiếu cơ chế bind parameter.
   - *Ước lượng chi phí xử lý:* **3 triệu VNĐ** (dựa trên thời gian rà soát và viết lại các câu lệnh truy vấn an toàn tại tầng cơ sở dữ liệu).

---

## 3. Lý do lựa chọn 3 mối đe dọa trên
Ba mối đe dọa T01, T02 và T03 được ưu tiên chọn xử lý vì chúng có điểm rủi ro tổng hợp cao nhất (tác động nghiêm trọng đến tính toàn vẹn và bảo mật dữ liệu cá nhân của sinh viên UEH). Việc khắc phục các lỗ hổng này mang lại hiệu quả bảo mật cao nhất với chi phí đầu tư hợp lý, tuân thủ chặt chẽ các nguyên lý thiết kế hệ thống an toàn từ cốt lõi.