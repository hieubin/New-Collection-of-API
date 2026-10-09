# Báo Cáo Bài Tập: Day8 - Prisma MVC (RESTful API)

**Họ và tên:** [Điền tên của bạn]  
**Mã sinh viên:** [Điền mã sinh viên]  
**Môn học:** [Điền tên môn học]  

## 1. Giới thiệu dự án
Dự án xây dựng RESTful API sử dụng mô hình MVC, kết hợp với ORM Prisma. Trong bài tập này, hệ thống tập trung vào việc xử lý các thao tác CRUD và bảo mật thông qua Authentication (JWT Token) và Authorization (Phân quyền người dùng).

Báo cáo dưới đây trình bày kết quả kiểm thử API chức năng xóa bài viết (Delete Post) với các kịch bản phân quyền khác nhau thông qua công cụ Postman.

---

## 2. Kết quả Kiểm thử API (Postman) - Chức năng Xóa Bài Viết

### Kịch bản 1: Cố gắng xóa bài viết khi chưa xác thực (Unauthorized)
- **Endpoint:** `DELETE http://localhost:3000/posts/1`
- **Mô tả:** Người dùng gửi request xóa bài viết nhưng không cung cấp Token xác thực (`Auth Type: No Auth`).
- **Kết quả:** 
  - Hệ thống từ chối yêu cầu và trả về mã lỗi HTTP Status **401 Unauthorized**.
  - Message phản hồi: `"message": "Unauthorized"`.
- **Hình ảnh minh chứng:**
<img width="1917" height="893" alt="Screenshot 2026-09-12 095302" src="https://github.com/user-attachments/assets/d1d16f80-a7b4-47cf-8847-685d4b3858b2" />


### Kịch bản 2: Cố gắng xóa bài viết khi không đủ quyền (Forbidden)
- **Endpoint:** `DELETE http://localhost:3000/posts/1`
- **Mô tả:** Người dùng gửi request kèm theo Bearer Token hợp lệ, nhưng tài khoản này không có quyền thực hiện hành động xóa (có thể đây là User thường cố gắng xóa bài viết của người khác, hoặc không có quyền Admin).
- **Kết quả:** 
  - Hệ thống nhận diện được user, nhưng từ chối cấp quyền thực thi và trả về mã lỗi HTTP Status **403 Forbidden**.
  - Message phản hồi: `"message": "Forbidden"`.
- **Hình ảnh minh chứng:**
<img width="1917" height="850" alt="Screenshot 2026-09-12 095319" src="https://github.com/user-attachments/assets/57ec2b3f-36fc-4318-b594-4bcf7a325a10" />


### Kịch bản 3: Xóa bài viết thành công (No Content)
- **Endpoint:** `DELETE http://localhost:3000/posts/11`
- **Mô tả:** Người dùng gửi request xóa bài viết (ID = 11) với Bearer Token hợp lệ và tài khoản có đủ quyền hạn (ví dụ: tác giả bài viết hoặc Admin).
- **Kết quả:** 
  - API xử lý xóa dữ liệu thành công trong cơ sở dữ liệu.
  - Hệ thống trả về mã HTTP Status **204 No Content** (do body response trống sau khi xóa).
- **Hình ảnh minh chứng:**
<img width="1917" height="890" alt="Screenshot 2026-09-12 095715" src="https://github.com/user-attachments/assets/142fa084-bd3c-4147-b511-a3fb8dbc00fc" />

---

## 3. Kết luận
Thông qua các bài test trên Postman, hệ thống API đã hoạt động đúng theo kịch bản bảo mật đề ra:
- **Xác thực (Authentication):** Hoạt động tốt, chặn đứng các request không có token (trả về lỗi 401).
- **Phân quyền (Authorization):** Hoạt động tốt, kiểm soát chặt chẽ quyền hạn của người dùng đối với từng tài nguyên cụ thể (trả về lỗi 403 nếu vi phạm).
- **Thực thi luồng chuẩn (Happy Path):** Xóa dữ liệu thành công và phản hồi đúng chuẩn RESTful API với mã trạng thái 204.
