# Design: Tinh gọn navbar + post song ngữ VI/EN

Date: 2026-06-28
Status: Approved

## Mục tiêu

Tinh gọn thanh navigation và cho phép mỗi bài viết có hai phiên bản ngôn
ngữ (tiếng Việt mặc định, tiếng Anh) với nút chuyển đổi ở đầu bài.

## Yêu cầu

1. Bỏ mục "About" khỏi navbar — tiêu đề "Hieu Duong" đóng vai trò link về
   trang About.
2. Gộp "Posts" và "Tiếng Việt" thành một mục "Posts" duy nhất.
3. Mỗi post có bản tiếng Việt và tiếng Anh; nút chuyển ngôn ngữ ở đầu bài;
   mặc định hiển thị tiếng Việt.
4. Trang listing Posts chỉ liệt kê bản tiếng Việt.

## Thiết kế

### 1. Navbar (`_quarto.yml`)

- Xóa mục `About` (`index.qmd`). Tiêu đề site "Hieu Duong" mặc định link về
  trang gốc `docs/index.html`, vốn được render từ `index.qmd` (trang About) —
  nên không cần mục About riêng.
- Xóa mục `Tiếng Việt` (`vi_posts.qmd`).
- Navbar `left` còn lại: `Portfolio`, `Posts`.
- Các icon mạng xã hội bên `right` giữ nguyên.

### 2. Trang Posts (`posts.qmd`)

- Gộp listing đang hoạt động của `vi_posts.qmd` vào `posts.qmd`.
- Tiêu đề trang: **"Posts"**.
- Listing: `contents: posts/*/index.qmd` để chỉ lấy bản tiếng Việt
  (`index.qmd`), loại trừ `index-en.qmd`.
- Cấu hình listing: `sort: "date desc"`, `type: default`, `categories: true`,
  `sort-ui: false`, `filter-ui: false`, `page-layout: full`.
- Xóa `vi_posts.qmd`.

### 3. Cấu trúc post song ngữ

```
posts/<slug>/
  index.qmd      # Tiếng Việt (mặc định, vào listing)
  index-en.qmd   # English (chỉ truy cập qua nút chuyển)
```

- `index-en.qmd` vẫn được render (project render list bao gồm
  `posts/**/*.qmd`) nhưng không xuất hiện trong listing.
- Nút chuyển ngôn ngữ — div `.lang-switch` ở đầu mỗi file, dùng link tương đối
  trong cùng thư mục:
  - Bản VI (`index.qmd`): `🌐 [English](index-en.html) · **Tiếng Việt**`
  - Bản EN (`index-en.qmd`): `🌐 **English** · [Tiếng Việt](index.html)`
- Thêm style `.lang-switch` vào `styles.css` (kích thước nhỏ, căn nhẹ, tách
  khỏi tiêu đề).

### 4. Dọn dẹp & nội dung

- Xóa 2 post demo mẫu của Quarto: `posts/welcome/`, `posts/post-with-code/`.
- Dịch bài `posts/context-engineering-for-ai-agents/index.qmd` sang tiếng Anh,
  tạo `posts/context-engineering-for-ai-agents/index-en.qmd` (giữ nguyên cấu
  trúc, callout, link; dịch categories sang nhãn tiếng Anh tương ứng).
- Thêm `.lang-switch` vào cả bản VI hiện có và bản EN mới.

### 5. Build & publish

- Chạy `quarto render` để cập nhật `docs/`.
- Xóa các thư mục demo cũ còn sót trong `docs/posts/`.
- Commit cả nguồn (`.qmd`, `_quarto.yml`, `styles.css`) và `docs/` đã render.

## Ngoài phạm vi (YAGNI)

- Không làm bộ chọn ngôn ngữ toàn site (Quarto language profiles).
- Không tự động phát hiện ngôn ngữ trình duyệt.
- Không thêm bản EN cho các bài chưa có (hiện chỉ có một bài thật).
