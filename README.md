# Studio POD — POD T-Shirt Muse

App tạo thiết kế để sản xuất POD (print-on-demand) t-shirt: từ 1 ảnh chụp áo mẫu, TÁCH design khỏi áo thành file in phẳng, nền trong suốt, 300 DPI (Flow V3 — trục chính), kèm bộ công cụ chỉnh tay nâng cao đầy đủ bên dưới.

## Tính năng hiện tại (bản trong `index.html`)

**⚡ Tách design khỏi áo — Flow V3 (trục chính, khu trên cùng)** — nạp ảnh chụp áo mẫu (kéo thả / Ctrl+V / chọn file / link Drive; có sẵn ảnh mẫu thật để thử), app tự đề xuất khung vùng in, người dùng kéo 4 góc cho khít design, 1 nút chạy toàn trình: gỡ nghiêng phối cảnh về mặt phẳng, chuẩn hoá sáng + khoá nền vải thành trong suốt, san phẳng màu 3 mức, tự nâng phân giải (Upscale AI Real-ESRGAN; lỗi thì tự lùi về pica và ghi rõ), quét watermark tự động với cổng khoá xuất như Bước 3, xem đối chiếu 3 ô (ảnh gốc ↔ design trong suốt ↔ mockup áo trắng/đen), tải ZIP (PNG in 4500×5400 300 DPI + file `.txt` hồ sơ) / PNG. Mọi tính năng bên dưới nằm trong khu "Chỉnh tay nâng cao" mặc định thu gọn, dùng chung ảnh nguồn và art đã tách.

**Radar mẫu áo bán chạy** — panel ở đầu trang dùng mỗi ngày: gợi ý ngách theo ngày và theo mùa, 3 thẻ nguồn Amazon / Etsy / Shopify tự dựng link theo ngách đang chọn (Shopify đi qua Google lọc shop thật), khay lưu ý tưởng trong phiên, nút đưa ngách sang khung Prompt sửa art. App không bịa bảng xếp hạng — số liệu xem trực tiếp tại nguồn.

1. **Nạp ảnh** — kéo-thả hoặc chọn file, có ảnh mẫu thử sẵn.
2. **Cắt khổ lớn tràn mặt trước** — kéo khung cắt tỉ lệ 4:5 (mặc định) hoặc 1:1, có tràn viền bleed 0–8%.
3. **Kiểm tra & xoá watermark — tự động** (Bước 3) — app tự quét heuristic trên lưới 6×6 (mật độ cạnh, lệch sáng, mẫu lặp) ngay khi vùng cắt ổn định, không cần bấm nút; hiện bản đồ nhiệt, báo cáo ô nghi ngờ và tự tô sẵn mặt nạ theo các ô đó; xoá bằng inpainting ngay trên trình duyệt (có thêm lựa chọn OpenCV — Telea qua OpenCV.js; kèm OCR tesseract.js khoanh vùng chữ hỗ trợ định vị watermark dạng chữ; chỉ bật sau khi tích xác nhận quyền sử dụng ảnh); **tự kiểm tra lại ngay sau khi xoá** — xuất file tự khoá/mở theo kết quả, chỉ mở khi sạch hoặc người dùng xác nhận đã soi tay từng ô; kết quả kiểm tra trước/sau được ghi vào file `.txt` hồ sơ tải kèm.
4. **Prompt sửa art** — gõ prompt tiếng Việt tự do hoặc bấm chip gợi ý; app đọc từ khoá trong prompt và áp dụng thật hiệu ứng tương ứng trên pixel (tông màu, pastel, vintage, đen trắng, cartoon/anime, nét đậm, mờ nền, đối xứng, zoom, pattern lặp), đồng thời liệt kê rõ đã áp dụng những gì.
   - Có nút sao chép prompt hoàn chỉnh để dùng tiếp ở AI tạo sinh bên ngoài.
   - Lưu ý trung thực: prompt trong app là xử lý ảnh tại chỗ trên trình duyệt, không phải AI tạo sinh vẽ hình mới.
5. **Làm đẹp + vẽ lại nét** — làm mịn, tăng nét (unsharp mask), sáng / tương phản / bão hoà / nhiệt độ màu / shadows / highlights, làm đẹp 1 chạm, kéo so sánh trước–sau; vẽ lại nét với Line-Art (Sobel), Cartoon phẳng (posterize), Sơn dầu nhẹ. Cùng bước này: resize chất lượng cao bằng pica khi xuất file, vector hoá art sang SVG (imagetracerjs, chọn 4–24 màu), trích bảng màu chủ đạo (color-thief), Upscale AI ×2/×4 bằng Real-ESRGAN qua onnxruntime-web, tách nền bằng AI MediaPipe — tất cả chạy ngay trong trình duyệt, thư viện và model nạp lười từ CDN khi bấm dùng. Trong bước này có mục tra cứu ngoài: 11 công cụ nâng chất lượng & sáng tạo (Upscayl, Topaz Gigapixel, Magnific, Let's Enhance, Claid, Vectorizer.AI, Recraft, Ideogram, Kittl, Cleanup.pictures, remove.bg) — chỉ là link tham khảo, không tích hợp API.
**Editor lớp (Konva + Moveable)** — panel giữa Bước 5 và Bước 6: thêm lớp chữ và lớp ảnh phụ (logo/sticker) lên art, kéo / resize / xoay / warp, quản lý danh sách lớp (ẩn/hiện, lên/xuống, nhân bản, xoá), áp dụng vào art ở độ phân giải đầy đủ để mockup và xuất file dùng tiếp, có hoàn tác áp dụng.

**Xưởng sáng tạo art** — panel sau Bước 5, chạy thuần Canvas 2D trong trình duyệt (không API, không AI tạo sinh): tạo art từ chữ (font Google Fonts nạp lười + font hệ thống, viền, bóng đổ, chữ cong), vẽ tay & sticker kéo/resize/xoay, hiệu ứng pattern lặp liền mạch / kaleidoscope 2-4-8 cánh / halftone / duotone / thay màu theo ngưỡng / khung viền, và nền trong suốt / màu trơn / gradient. Mọi thao tác có hoàn tác và "Áp dụng vào art" kết xuất ở độ phân giải đầy đủ.

**Kết nối Google Drive** — panel lưu link Folder ảnh mẫu và Folder sản phẩm (lưu bằng localStorage, mở app lần sau vẫn còn), nút mở folder trên Drive; ở Bước 1 có ô dán link file ảnh Drive để nạp ảnh mẫu (tự trích file ID, kiểm tra CORS trước khi nạp, báo rõ khi bị chặn). Ghi trung thực: app tĩnh không đăng nhập OAuth vào Drive và không tự tải file lên folder sản phẩm được.

**Nâng cấp chất lượng hệ thống (4 đợt)** — đợt 1: xuất JPG bằng MozJPEG qua jSquash + tối ưu PNG bằng OxiPNG, JPG gắn EXIF 300 DPI bằng piexifjs, codec dùng cho mỗi lần xuất được ghi vào file .txt hồ sơ; đợt 2: tách nền thêm model BiRefNet (mặc định, chất lượng cao) bên cạnh MediaPipe; đợt 3: Comlink + Web Worker cho inpaint và kết xuất hiệu ứng ở độ phân giải đầy đủ (tiến độ thật, huỷ được, lỗi thì rơi về luồng chính); đợt 4: chroma.js (phối màu hài hoà + kiểm tra tương phản), image-q (giới hạn số màu cho in), opentype.js (chuyển chữ thành đường nét khi áp dụng), SVGO (tối ưu SVG sau vector hoá), potrace (trace đen trắng nét sạch).

**Lộ trình gộp vòng 4+5 (6 đợt)** — đợt 1: lưu/mở dự án thiết kế + tự lưu nháp bằng localForage (IndexedDB), xuất/nhập file dự án .json; đợt 2: xuất cả bộ .zip bằng JSZip và xuất PDF in ấn đúng kích thước thật bằng pdf-lib; đợt 3: thêm cách xoá watermark LaMa (AI) chạy ONNX; đợt 4: thêm model tách nền BEN2 và model upscale SwinIR; đợt 5: mã CMYK + Pantone gần nhất cho từng màu trong bảng, xem trước màu khi in (mô phỏng CMYK), và mục kiểm tra trước in (preflight) ở Bước 7; đợt 6: lớp QR / mã vạch CODE128 trong Editor lớp, xuất PSD theo lớp bằng ag-psd, mockup nếp vải (thử nghiệm) bằng PixiJS, và mục tách màu in lụa ở Bước 5.

6. **Mockup** — xem trước design tràn mặt trước trên áo trắng / đen / xám.
7. **Xuất file in** — đúng 1 file khổ lớn 4500×5400 px (tuỳ chọn 5000×5400 / 6000×6000), 300 DPI nhúng trong file PNG/JPG, tên file kèm prompt rút gọn và tải kèm file `.txt` ghi nguyên văn prompt đã dùng cùng kết quả kiểm tra watermark trước/sau. Xuất bị khoá cho tới khi bước kiểm tra watermark đạt "Sạch" hoặc người dùng xác nhận soi tay.
8. **Panel "Tương tác trực tiếp với Muse — sửa cấu trúc app nhanh"** (trong Bước 4, dưới khung Prompt) — soạn lệnh sửa app với tiền tố `[Studio POD — sửa cấu trúc app]`, sao chép và dán vào chat Muse tại https://muse.ai để Muse sửa app ngay trong phiên. App tĩnh không tự gửi tin cho Muse được; panel ghi rõ giới hạn này.

**Sửa lỗi Upscale AI Real-ESRGAN (25/10/2026)** — 2 nguồn model cũ đã chết (Kizi-Art trả HTTP 401; repo Qualcomm không chứa file .onnx nào nên 404), thay bằng 3 nguồn đã kiểm chứng tải ẩn danh + chạy suy luận thật bằng onnxruntime-web 1.20.1: tamnvcc realesr-general-x4v3 (tile cố định 128), Meeperomi x4plus (đầu vào động), imgdesignart x4plus (tile cố định 64). App nay lưu và dùng đúng cỡ tile cố định của từng model khi chạy upscale.

## Cách chạy

Mở trực tiếp file `index.html` trên trình duyệt, hoặc bật GitHub Pages cho repo này (Settings → Pages → Branch `main`, thư mục `/ (root)`) để có link web chạy app.

Toàn bộ xử lý ảnh chạy ngay trên trình duyệt bằng Canvas — không tải ảnh của người dùng lên máy chủ nào.

## Hướng phát triển tiếp

- Đã hoàn thành 3 đợt nhúng thư viện chạy trực tiếp trong web: đợt 1 pica / imagetracerjs / color-thief / tesseract.js; đợt 2 onnxruntime-web (Real-ESRGAN) / MediaPipe / OpenCV.js; đợt 3 Konva + Moveable.
- Hướng nối Meta Model API qua backend đang tạm hoãn theo quyết định của người dùng — hiện dùng Muse trực tiếp trong chat để sửa app và xử lý prompt.
