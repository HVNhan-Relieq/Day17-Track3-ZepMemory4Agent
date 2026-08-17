# Day 17 — Multi-Memory Agent với Zep (bài nộp)

## Trạng thái

4 hàm trong `src/memory_student.py` đã hoàn tất (không còn `NotImplementedError`), bonus
`retrieve_for_case` trong `src/demo_ui.py` đã viết. `pytest -q`: **11 passed, 1 skipped**
(golden chưa phát hành). No-memory baseline chạy đủ: **2/11**.

Benchmark student chưa chạy được: tài khoản Zep Cloud mới tạo có độ trễ ingestion lớn —
`src.seed` timeout ở stage 2 khi chờ index `ASYNC-FIX-20` (240s), chạy lại với
`ZEP_POLL_TIMEOUT=900` vẫn chưa xong trước hạn nộp. Đây là độ trễ phía cloud, không phải
lỗi retrieval; lệnh tái lập:
`docker compose run --rm -e ZEP_POLL_TIMEOUT=900 app python -m src.seed` rồi
`docker compose run --rm app python -m src.evaluate --impl student --reuse-seeded`.

## Phân tích benchmark (baseline đo được)

`reports/benchmark_no_memory.json`: 2/11 pass, hit rate **18.2%**. Chỉ E01 và E10 pass vì
evidence còn nằm trong short-term buffer local. Chín case còn lại fail: E02/E03/E08/E09
(cross-session, long-term), E04/E05 (episodic), E06/E11 (semantic), E07 (mixed).

No-memory báo token reduction gần như tuyệt đối vì nó retrieve gần như rỗng — reduction
chỉ có nghĩa khi đọc kèm hit rate. Retrieval rỗng rất rẻ nhưng sai; đó là lý do scorer
dùng `must_contain_all`/`must_not_contain` thay vì LLM judge.

E07 (mixed) cần ghép long-term + semantic, hai evidence bắt buộc là `Python` (preference
của Minh, từ user graph) và `Idempotency-Key` (payment rule, từ shared graph). Nếu một
marker có trong raw layer nhưng mất ở merged text thì phải đọc `budget_breakdown` để biết
layer nào bị trim, thay vì nâng `LAB_CONTEXT_TOKENS`.

## Compaction (E10)

`src.demo_short_term`: buffer giữ 16 message, 0 durable note, 0 compaction — token tăng
tuyến tính. Summary giữ 6 message sau 2 compaction; sliding giữ 6 message sau 10 compaction.
Cả hai vẫn giữ `REVIEW-DEADLINE-1600` (Friday, 16:00) trong `DURABLE_NOTES` vì
`extract_durable_notes` bắt marker viết hoa và các từ khóa deadline/constraint. Buffer
không bền vững: nó không nén, và khi vượt context sẽ mất chính constraint quan trọng nhất.

## Reflection

**Layer quan trọng nhất:** long-term (4/11 case: E02, E03, E08, E09) — nó vừa phải nhớ qua
session, vừa phải cách ly user. E08 minh họa conflict rule "recency + scope": constraint mới
của BLUEBIRD-42 (TypeScript/NestJS) thắng preference Python chung, nhưng không xóa preference
đó ở scope khác. E09 minh họa isolation: `ORCHID-27` của Minh không được xuất hiện trong câu
trả lời cho Lan — đây là lý do phải search bằng `user_id`, không phải `graph_id`.

**Trade-off Zep Context Block vs Redis + Qdrant:** Zep tự trích fact, gắn validity range và
tổng hợp context xuyên thread — đổi lại là độ trễ ingestion bất định (đúng vấn đề tôi gặp)
và phụ thuộc vendor. Redis + Qdrant cho độ trễ thấp, kiểm soát hoàn toàn, nhưng phải tự viết
extraction, conflict resolution và deletion.

**Guardrail chống memory poisoning / background write tự cấp quyền:** `require_memory_consent`
chặn durable write khi user chưa opt-in; `minimize_pii` redact email/phone trước khi gửi lên
Zep; `heartbeat --dry-run` chỉ đọc và in action; `forget.py` + `--verify-only` chứng minh
right-to-be-forgotten. Theo `control_plane/AGENTS.md`, heartbeat không bao giờ được tự cấp
thêm quyền, và durable record phải giữ source + timestamp + validity để audit được.
