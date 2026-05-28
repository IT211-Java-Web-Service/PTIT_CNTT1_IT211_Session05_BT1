# Nguyên Tắc Thiết Kế REST API

---

## 1. Danh từ số nhiều trong URL (không dùng động từ)

**Đúng.** REST API yêu cầu dùng **danh từ số nhiều** cho đường dẫn, vì URL đại diện cho tài nguyên (resource), không phải hành động. Hành động được thể hiện qua HTTP method.

| Loại | Ví dụ |
|------|-------|
| Đúng | `GET /students` |
| Đúng | `DELETE /students/5` |
| Sai | `GET /getStudents` |
| Sai | `POST /deleteStudent/5` |

---

## 2. Ví dụ API Quản Lý Sinh Viên

### Ví dụ đúng
- `GET /students` — Lấy danh sách tất cả sinh viên
- `POST /students` — Tạo mới một sinh viên

### Ví dụ sai
- `GET /getListSinhVien` — Dùng động từ và tiếng Việt lẫn lộn
- `POST /taoSinhVienMoi` — Dùng động từ, không theo chuẩn REST

---

## 3. Các Phương Thức HTTP

| Method | Mục đích chính | Có body không? (thường) |
|--------|---------------|------------------------|
| GET | Lấy dữ liệu từ server | Không |
| POST | Tạo mới một tài nguyên | Có |
| PUT | Cập nhật **toàn bộ** một tài nguyên (thay thế hoàn toàn) | Có |
| PATCH | Cập nhật **một phần** tài nguyên (chỉ các trường được gửi lên) | Có |
| DELETE | Xóa một tài nguyên | Thường không |

---

## 4. Phân Biệt PUT và PATCH

**Dữ liệu ban đầu:**
```json
{ "id": 1, "name": "Bút bi", "price": 5000 }
```

### Dùng `PUT /products/1` với body `{ "name": "Bút mực" }`
```json
// Kết quả sau cập nhật:
{ "id": 1, "name": "Bút mực", "price": null }
```
> PUT **thay thế toàn bộ** tài nguyên. Các trường không được gửi lên (`price`) sẽ bị mất hoặc trở thành `null`.

---

### Dùng `PATCH /products/1` với body `{ "name": "Bút mực" }`
```json
// Kết quả sau cập nhật:
{ "id": 1, "name": "Bút mực", "price": 5000 }
```
> PATCH **chỉ cập nhật trường được gửi lên**. Các trường còn lại (`price`) giữ nguyên giá trị cũ.

### Tóm tắt sự khác biệt

| | PUT | PATCH |
|-|-----|-------|
| Cập nhật | Toàn bộ tài nguyên | Một phần tài nguyên |
| Trường không gửi | Bị xóa / null | Giữ nguyên |
| Khi dùng | Muốn thay toàn bộ object | Chỉ muốn sửa 1–2 trường |

---

## 5. Mã Trạng Thái HTTP (Status Codes)

| Mã | Ý nghĩa | Tình huống ví dụ |
|----|---------|-----------------|
| **200** | OK — Thành công | `GET /students` trả về danh sách sinh viên thành công |
| **201** | Created — Tạo mới thành công | `POST /students` tạo sinh viên mới thành công |
| **204** | No Content — Thành công nhưng không có dữ liệu trả về | `DELETE /students/5` xóa sinh viên thành công, không cần trả về gì |
| **400** | Bad Request — Yêu cầu không hợp lệ | `POST /students` thiếu trường bắt buộc như `name` hoặc `email` sai định dạng |
| **404** | Not Found — Không tìm thấy tài nguyên | `GET /students/999` không tìm thấy sinh viên có id = 999 |
| **500** | Internal Server Error — Lỗi phía server | Server bị lỗi kết nối database khi xử lý yêu cầu |

---

*Tài liệu tổng hợp các nguyên tắc cơ bản khi thiết kế REST API.*
