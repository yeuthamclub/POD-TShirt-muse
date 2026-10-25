# Studio POD — POD T-Shirt Muse

App tạo thiết kế để sản xuất POD (print-on-demand) t-shirt: từ 1 hình áo mẫu / artwork, tạo ra bản design 2D khổ lớn tràn mặt trước áo, sẵn sàng để in.

## Tính năng hiện tại (bản trong `index.html`)

1. **Nạp ảnh** — kéo-thả hoặc chọn file, có ảnh mẫu thử sẵn.
2. **Cắt khổ lớn tràn mặt trước** — kéo khung cắt tỉ lệ 4:5 (mặc định) hoặc 1:1, có tràn viền bleed 0–8%.
3. **Prompt sửa art** — gõ prompt tiếng Việt tự do hoặc bấm chip gợi ý; app đọc từ khoá trong prompt và áp dụng thật hiệu ứng tương ứng trên pixel (tông màu, pastel, vintage, đen trắng, cartoon/anime, nét đậm, mờ nền, đối xứng, zoom, pattern lặp), đồng thời liệt kê rõ đã áp dụng những gì.
   - Có nút sao chép prompt hoàn chỉnh để dùng tiếp ở AI tạo sinh bên ngoài.
   - Lưu ý trung thực: prompt trong app là xử lý ảnh tại chỗ trên trình duyệt, không phải AI tạo sinh vẽ hình mới.
4. **Làm đẹp + vẽ lại nét** — làm mịn, tăng nét (unsharp mask), sáng / tương phản / bão hoà / nhiệt độ màu / shadows / highlights, làm đẹp 1 chạm, kéo so sánh trước–sau; vẽ lại nét với Line-Art (Sobel), Cartoon phẳng (posterize), Sơn dầu nhẹ.
5. **Mockup** — xem trước design tràn mặt trước trên áo trắng / đen / xám.
6. **Xuất file in** — đúng 1 file khổ lớn 4500×5400 px (tuỳ chọn 5000×5400 / 6000×6000), 300 DPI nhúng trong file PNG/JPG, tên file kèm prompt rút gọn và tải kèm file `.txt` ghi nguyên văn prompt đã dùng.
7. **Panel "Tương tác trực tiếp với Muse — sửa cấu trúc app nhanh"** (trong Bước 3, dưới khung Prompt) — soạn lệnh sửa app với tiền tố `[Studio POD — sửa cấu trúc app]`, sao chép và dán vào chat Muse tại https://muse.ai để Muse sửa app ngay trong phiên. App tĩnh không tự gửi tin cho Muse được; panel ghi rõ giới hạn này.

## Cách chạy

Mở trực tiếp file `index.html` trên trình duyệt, hoặc bật GitHub Pages cho repo này (Settings → Pages → Branch `main`, thư mục `/ (root)`) để có link web chạy app.

Toàn bộ xử lý ảnh chạy ngay trên trình duyệt bằng Canvas — không tải ảnh của người dùng lên máy chủ nào.

## Hướng phát triển tiếp

- Nối khung Prompt với **Meta Model API (model Muse)** qua một backend nhỏ giữ API key, để prompt được xử lý bằng AI thật thay cho / bổ sung cho xử lý từ khoá tại chỗ. `muse.ai` không có bản nhúng cho web ngoài, nên API là đường tích hợp chính thức.
- Backend mẫu (Node/Cloudflare Worker) sẽ nhận prompt + thông số ảnh, gọi Meta Model API, trả về kế hoạch hiệu ứng / prompt tạo sinh đã tinh chỉnh cho app áp dụng.
