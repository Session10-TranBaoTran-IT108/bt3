# Session08 - Ex03: So sánh Markdown và HTML dưới góc nhìn Token

## Phần 1. Phân tích bài toán

### 1. Token là gì

AI (mô hình ngôn ngữ lớn) không đọc từng chữ cái mà chia văn bản thành các đơn vị nhỏ gọi là token. Một token có thể là một từ, một phần của từ hoặc một ký hiệu. Chi phí sử dụng API và thời gian xử lý đều tính theo số token đầu vào và đầu ra: càng nhiều token thì càng tốn tiền và càng chậm.

### 2. Cùng một định dạng, số ký tự khác nhau

Ví dụ với câu "Rất quan trọng" in đậm:

- Markdown: `**Rất quan trọng**` (thêm 4 ký tự định dạng)
- HTML: `<strong>Rất quan trọng</strong>` (thêm 17 ký tự định dạng)

Ví dụ với tiêu đề và danh sách:

Markdown:

```markdown
## Các loại rau
- Rau muống
- Cải xanh
```

HTML:

```html
<h2>Các loại rau</h2>
<ul>
  <li>Rau muống</li>
  <li>Cải xanh</li>
</ul>
```

Bản HTML dài hơn nhiều vì mỗi phần tử phải có thẻ mở và thẻ đóng, chưa kể thuộc tính (`class`, `style`, `id`) thường đi kèm.

### 3. Vì sao Markdown giúp AI xử lý nhanh hơn

- Ký hiệu Markdown ngắn như `**`, `#`, `-` thường chỉ chiếm 1 token, trong khi thẻ HTML như `<strong>` hay `</li>` bị tách thành nhiều token (dấu `<`, tên thẻ, dấu `>`, dấu `/`).
- Cùng một nội dung, Markdown cho ra ít token hơn nên tiết kiệm chi phí và giảm độ trễ khi gửi và nhận.
- Ít token định dạng nghĩa là "nhiễu" ít hơn, phần lớn token dành cho nội dung thật nên AI dễ tập trung vào ý chính.
- Mô hình được huấn luyện trên rất nhiều dữ liệu Markdown (tài liệu kỹ thuật, GitHub, diễn đàn) nên hiểu cấu trúc này rất tốt.
- Cửa sổ ngữ cảnh (context window) có giới hạn. Dùng Markdown giúp nhét được nhiều nội dung hữu ích hơn vào cùng một lượt hỏi.

Lưu ý: số token cụ thể phụ thuộc vào từng mô hình và bộ tách token (tokenizer), nên các con số ở trên mang tính minh họa về xu hướng chung.

## Phần 2. So sánh và đánh giá

### Bảng so sánh Markdown và HTML

| Tiêu chí | Markdown | HTML |
|----------|----------|------|
| Số ký tự định dạng (in đậm) | 4 ký tự (`**...**`) | 17 ký tự (`<strong>...</strong>`) |
| Số token tiêu tốn | Ít | Nhiều |
| Chi phí API | Thấp | Cao hơn |
| Tốc độ xử lý | Nhanh hơn | Chậm hơn |
| Độ dễ đọc khi chưa render (human-readable) | Cao, nhìn văn bản thô vẫn hiểu | Thấp, bị rối bởi thẻ |
| Độ dễ học | Rất dễ, học trong vài phút | Cần học thẻ và thuộc tính |
| Khả năng tùy biến giao diện | Hạn chế | Rất mạnh (màu sắc, bố cục, CSS) |
| Phù hợp với | Nội dung văn bản, tài liệu, câu trả lời của AI | Trang web hoàn chỉnh, giao diện phức tạp |

### Giải pháp tối ưu cho hệ thống GreenTech

Đề xuất: dùng Markdown làm định dạng chính để trao đổi dữ liệu với AI, và chỉ chuyển sang HTML ở bước cuối cùng khi hiển thị lên giao diện web hoặc ứng dụng.

Quy trình:

1. Người nông dân đặt câu hỏi.
2. Hệ thống gửi câu hỏi và dữ liệu tham khảo cho AI dưới dạng Markdown.
3. AI trả lời bằng Markdown.
4. Ứng dụng dùng thư viện chuyển đổi Markdown sang HTML để hiển thị đẹp trên màn hình.

### Lập luận

- Tiết kiệm chi phí: hệ thống có thể phục vụ nhiều lượt hỏi mỗi ngày, giảm token ở mỗi lượt sẽ giảm đáng kể tổng chi phí.
- Phản hồi nhanh hơn: ít token hơn nên AI nhận và trả lời nhanh, phù hợp khi người nông dân hỏi trên điện thoại, mạng yếu.
- Dễ kiểm tra và bảo trì: đội ngũ phát triển đọc được dữ liệu thô bằng mắt, không cần công cụ render, dễ phát hiện lỗi.
- Nội dung trả lời chủ yếu là văn bản (tiêu đề, danh sách, bảng, in đậm), đúng những thứ Markdown làm tốt.
- Vẫn giữ được giao diện đẹp vì HTML chỉ xuất hiện ở khâu hiển thị, không tốn token của AI.

Trường hợp cần HTML: khi phải tạo bố cục phức tạp, có màu sắc và nút bấm riêng, thì dùng HTML hoặc CSS ở tầng giao diện, không đưa vào dữ liệu gửi cho AI.

## Kết luận

Markdown cho cùng một nội dung nhưng ít ký tự và ít token hơn HTML, đồng thời dễ đọc với con người khi chưa render. Với hệ thống GreenTech, giải pháp tối ưu là gửi và nhận Markdown với AI, rồi chuyển sang HTML khi hiển thị.
