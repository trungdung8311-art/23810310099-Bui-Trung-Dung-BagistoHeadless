# Bài Thực Hành Bagisto Headless Commerce

**Sinh viên**: Bùi Trung Dũng  
**MSSV**: 23810310099
**Lớp**: D18CNPM2

---

### Phần 1: Cài đặt Hệ thống

**Ảnh chụp màn hình danh sách sản phẩm trong Admin:**

![Alt text](Screenshot 2026-04-03 211054.png)

### Phần 2: Khai thác GraphQL API

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
      name: "BuiTrungDung"
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

##CÂU HỎI BẮT BUỘC

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
