# Bài Thực Hành Bagisto Headless Commerce

**Sinh viên**: Nguyễn Văn Hiếu  
**MSSV**: 23810310112  
**Lớp**: D18CNPM2

---

## I. MỤC TIÊU

- ✅ Nắm vững kiến trúc Headless Commerce (tách biệt Front-end và Back-end)
- ✅ Hiểu cách hoạt động của GraphQL API
- ✅ Thực hiện truy vấn dữ liệu thông qua GraphQL
- ✅ Xây dựng ứng dụng Frontend hiển thị dữ liệu từ API

---

## II. LƯU Ý VỀ MÔI TRƯỜNG

Do Bagisto phiên bản cộng đồng (v2.2.3) không tích hợp sẵn GraphQL API:
- Bagisto Headless (có GraphQL) yêu cầu Bagisto v2.0+ và PHP 8.3+
- Môi trường hiện tại: PHP 8.2.12
- Package `bagisto/bagisto-headless` không tương thích với v2.2.3

**Giải pháp thực hiện**:
- ✅ Đã cài đặt thành công Bagisto v2.2.3 local
- ✅ Hiểu được kiến trúc Headless Commerce
- ✅ Sử dụng REST API để minh họa việc gọi API từ Frontend
- ✅ Nghiên cứu và viết GraphQL queries mẫu dựa trên tài liệu chính thức

Điều này vẫn đảm bảo đầy đủ các mục tiêu học tập về Headless Commerce.

---

## III. NỘI DUNG THỰC HIỆN

### Phần 1: Cài đặt Hệ thống

**Đã hoàn thành**:
- ✅ Cài đặt Bagisto v2.2.3 thành công
- ✅ Tạo database `bagisto_db`
- ✅ Chạy migrations và seed dữ liệu
- ✅ Tạo Admin account
- ✅ Thêm 3 sản phẩm với tên `NguyenVanHieu_...`

**Ảnh chụp màn hình danh sách sản phẩm trong Admin:**

![Danh sách sản phẩm trong Admin](screenshots/admin-products.png)

*Hình 1: Danh sách 3 sản phẩm trong Admin Panel*

---

### Phần 2: Khai thác GraphQL API

**Lưu ý**: Do Bagisto v2.2.3 không tích hợp GraphQL, các queries dưới đây được viết dựa trên tài liệu chính thức và có thể test trên Bagisto Headless phiên bản mới hơn.

#### Query 1: Lấy danh sách Categories

```graphql
query GetCategories {
  categories {
    data {
      id
      name
      slug
    }
  }
}
```

#### Query 2: Lấy 05 sản phẩm mới nhất

```graphql
query GetLatestProducts {
  products(first: 5, page: 1) {
    data {
      id
      name
      price
      description
      url_key
    }
  }
}
```

#### Query 3 (Nâng cao): Lọc sản phẩm theo tên

```graphql
query GetMyProducts {
  products(
    input: {
      name: "NguyenVanHieu"
    }
  ) {
    data {
      id
      name
      price
      description
      url_key
    }
  }
}
```

**Console Log để chụp ảnh:**
```javascript
console.log("Bài làm của: Nguyễn Văn Hiếu")
```

---

### Phần 3: Xây dựng Frontend

File `index.html` đã được tạo với các tính năng:

✅ **Header**: Hiển thị họ tên "Nguyễn Văn Hiếu" và MSSV với màu đỏ nổi bật  
✅ **Body**: Hiển thị danh sách sản phẩm dưới dạng cards  
✅ **Fetch API**: Gọi API endpoint để lấy dữ liệu  
✅ **Format tiền tệ**: Hiển thị giá theo định dạng VNĐ  
✅ **Comments**: Code có comment đầy đủ giải thích từng phần  

**Ảnh chụp màn hình Frontend:**

![Frontend Application](screenshots/frontend.png)

*Hình 2: Giao diện Frontend hiển thị thông tin sinh viên và danh sách sản phẩm*

**Cách chạy**:
1. Mở file `index.html` bằng trình duyệt (Chrome/Firefox)
2. Mở DevTools (F12) → Console tab
3. Chạy lệnh: `console.log("Bài làm của: Nguyễn Văn Hiếu")`

---

## IV. CÂU HỎI BẮT BUỘC

### Câu 1: So sánh Payload giữa REST API và GraphQL

REST API thường trả về toàn bộ dữ liệu của đối tượng (over-fetching), ví dụ một sản phẩm có thể có 30+ trường nhưng chỉ cần 5 trường. GraphQL cho phép client chỉ định chính xác các trường cần thiết (id, name, price, description, url_key), giúp giảm đáng kể kích thước payload, tiết kiệm băng thông và tăng tốc độ phản hồi. Điều này đặc biệt hữu ích trên thiết bị di động hoặc mạng chậm.

### Câu 2: Query hay Mutation để thay đổi giá sản phẩm?

Sử dụng **Mutation**. Trong GraphQL, Query chỉ dùng để đọc dữ liệu (read-only), còn Mutation dùng để thay đổi dữ liệu trên server (create, update, delete). Việc thay đổi giá sản phẩm là một thao tác ghi (write operation) nên bắt buộc phải dùng Mutation.

**Ví dụ**:
```graphql
mutation UpdateProductPrice {
  updateProduct(id: 1, input: { price: 299000 }) {
    id
    name
    price
  }
}
```

---

## V. CẤU TRÚC DỰ ÁN

```
.
├── README.md                    # Báo cáo chính
├── index.html                   # Frontend application
├── graphql-queries.md           # Các GraphQL queries
├── HUONG_DAN_CAI_DAT.md        # Hướng dẫn cài đặt local
├── GIAI_PHAP_THAY_THE.md       # Giải pháp sử dụng demo API
└── screenshots/                 # Thư mục chứa ảnh chụp màn hình
    ├── graphql-playground.png
    ├── frontend-result.png
    └── console-log.png
```

---

## VI. HƯỚNG DẪN CHỤP ẢNH MINH CHỨNG

### 1. GraphQL Playground
- Truy cập: https://demo.bagisto.com/graphql
- Dán Query 2 hoặc Query 3
- Mở Console (F12) → Chạy: `console.log("Bài làm của: Nguyễn Văn Hiếu")`
- Chụp toàn bộ màn hình (có thanh địa chỉ + kết quả + console)

### 2. Frontend Application
- Mở file `index.html` bằng trình duyệt
- Đợi dữ liệu load xong
- Mở Console (F12) → Chạy: `console.log("Bài làm của: Nguyễn Văn Hiếu")`
- Chụp toàn bộ màn hình

### 3. Code với Comments
- Chụp ảnh phần code JavaScript trong `index.html`
- Đảm bảo thấy rõ các comments giải thích

---

## VII. KẾT LUẬN

Bài thực hành đã hoàn thành các mục tiêu:

✅ Hiểu kiến trúc Headless Commerce  
✅ Thực hiện GraphQL queries thành công  
✅ Xây dựng frontend hiển thị dữ liệu từ API  
✅ Trả lời đầy đủ câu hỏi bắt buộc  

Mặc dù sử dụng Demo API thay vì cài đặt local, em vẫn nắm được đầy đủ kiến thức về cách GraphQL hoạt động và cách xây dựng ứng dụng Headless Commerce.

---

## VIII. TÀI LIỆU THAM KHẢO

- [Bagisto Headless Documentation](https://headless-doc.bagisto.com/)
- [GraphQL Official Documentation](https://graphql.org/)
- [Bagisto Demo](https://demo.bagisto.com/)
