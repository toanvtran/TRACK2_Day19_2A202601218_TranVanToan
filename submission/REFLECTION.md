# Reflection — Lab 19

**Tên:** _Trần Văn Toàn_
**Cohort:** _A20_
**Path đã chạy:** _lite_

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Với `exact` query — chứa nguyên văn từ khoá kỹ thuật có trong corpus — **BM25**
thắng hoặc ngang hybrid, vì tín hiệu keyword đã đủ mạnh để định vị đúng doc.
Với `paraphrase` query — diễn đạt lại bằng từ không xuất hiện verbatim — **vector**
đáng lẽ thắng, nhưng ở lab này `bge-small-en-v1.5` là model English-trained nên
semantic recall trên tiếng Việt còn yếu (~24–32%); đổi sang `bge-m3` sẽ cải thiện
rõ. Với `mixed` query — vừa có từ exact vừa có ý paraphrase — **hybrid thắng rõ**
(~100% vs 97–98% các mode thuần), vì RRF gộp được hai nguồn tín hiệu bù cho nhau.
Hybrid thắng *trung bình* nhờ robust trên mọi kiểu query, nên production mặc định
dùng nó.

Tôi không dùng hybrid khi: (1) truy vấn là mã/ID/từ khoá chính xác (SKU, error
code) — pure BM25 vừa nhanh vừa đúng; (2) tra cứu ngữ nghĩa thuần trên corpus đồng
nhất ngôn ngữ với model tốt — pure vector đủ, tránh chi phí chạy hai retriever +
fusion khi latency budget eo hẹp.

---

## Điều ngạc nhiên nhất khi làm lab này

Post-filter recall sập âm thầm về 0.00 khi filter chặt (~4% corpus) mà không có
exception nào — chất lượng giảm im lặng là loại lỗi khó phát hiện nhất.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
