# Hệ thống Quản lý Thư viện và Đặt mượn Sách (Phần 1: RESTful API & Client)

> Học phần: **Phát triển ứng dụng phân tán** — Đề tài 20  
> Sinh viên: **Phạm Bảo Lâm** — MSSV: **dtc245200287** — Lớp: **CNTT K23E**

## 1. Tổng quan kiến trúc
Hệ thống xây dựng theo mô hình **Client–Server** giao tiếp qua HTTP/JSON:
- **`Library.Api`**: Dịch vụ RESTful Web API bằng ASP.NET Core Minimal API (Routing, Route Constraints `:int:min(1)` / `:guid`, DTO, Validation, chuẩn hóa HTTP Status Codes `200`, `201`, `400`, `404`, `409`).
- **`Library.Client`**: Ứng dụng Console dùng `HttpClient` trao đổi dữ liệu JSON với API.

## 2. Danh sách API (`http://localhost:8081`)

| Method | Endpoint | Mô tả | Mã HTTP |
|---|---|---|---|
| `GET` | `/health` | Kiểm tra API hoạt động | `200` |
| `GET` | `/books?keyword=` | Danh sách sách và tìm kiếm theo từ khóa | `200` |
| `GET` | `/books/{id}` | Chi tiết sách (`id >= 1`) | `200`, `404` |
| `POST` | `/reservations` | Tạo yêu cầu đặt mượn sách | `201`, `400`, `404`, `409` |
| `GET` | `/reservations/{id}` | Tra cứu trạng thái yêu cầu (`id` dạng GUID) | `200`, `404` |
| `GET` | `/users/{userId}/reservations` | Danh sách yêu cầu của người dùng | `200` |
| `POST` | `/reservations/{id}/borrow` | Xác nhận cho mượn (`Reserved` → `Borrowed`) | `200`, `404`, `409` |
| `POST` | `/reservations/{id}/return` | Xác nhận trả sách (`Borrowed` → `Returned`) | `200`, `404`, `409` |

## 3. Hướng dẫn chạy và kiểm thử bằng `curl`
```bash
# Terminal 1: Chạy RESTful API
dotnet run --project src/Library.Api --urls http://localhost:8081

# Terminal 2: Chạy Console Client hoặc test bằng curl
dotnet run --project src/Library.Client
curl -i http://localhost:8081/books
curl -i http://localhost:8081/books/999
curl -i -X POST http://localhost:8081/reservations -H "Content-Type: application/json" -d '{"userId":0,"bookId":2}'
curl -i -X POST http://localhost:8081/reservations -H "Content-Type: application/json" -d '{"userId":1,"bookId":2}'

```
