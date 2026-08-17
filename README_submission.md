# Day 17 — Multi-Memory Agent với Zep (bài nộp)

Student: **11/11 PASS, hit rate 100%**, avg latency 666 ms, avg token reduction 14.2%
(`reports/benchmark.json`). Golden: **20/20, bonus 10/10** (`reports/golden_benchmark.json`).
No-memory: 2/11, 18.2%. `pytest -q`: 12 passed.

## Phân tích benchmark

Cả bốn layer đều 100%. Tốn nhất là long-term: E03 1404 tok, E08 1396, E02 1395, reduction 0%,
latency ~1.1 s (prime thread + `get_user_context` + edge search). Semantic rẻ nhất: E11 146 tok
/ 74.2%, E06 148 tok / 67.8%, ~0.2 s vì chỉ một `graph.search`.

E07 ghép long-term + semantic; hai evidence bắt buộc là `Python` (user graph) và
`Idempotency-Key` (shared graph). `budget_breakdown`: long-term raw 1412 tok trim còn 324
(limit 320), semantic 148/240 nguyên vẹn. Vậy `Python` phải nằm ở **đầu** context block mới
sống sót — lý do `trim` giữ head.

No-memory đạt reduction 81.8% chỉ vì retrieve gần như rỗng, trả giá bằng 2/11. Reduction chỉ
có nghĩa khi đọc kèm hit rate; đó cũng là lý do scorer dùng `must_contain_all` thay vì LLM judge.

## E08 recency, E10 compaction

E08 là conflict rule "recency + scope": constraint BLUEBIRD-42 (TypeScript/NestJS) ở stage 3
thắng preference Python *cho project đó* mà không xóa preference ở scope khác — E02 vẫn pass
cùng lần chạy. Edge search `limit=20` kèm validity range giữ được cả hai fact.

E10: buffer giữ 16 message, 0 durable note, 0 compaction. Sliding giữ 6 message sau 10 lần
compaction mà `REVIEW-DEADLINE-1600` (Friday, 16:00) vẫn còn trong `DURABLE_NOTES`, vì
`extract_durable_notes` bắt marker viết hoa trước khi evict raw turn. Buffer không nén nên
vượt context là mất đúng constraint quan trọng nhất.

## Reflection

**Layer quan trọng nhất:** long-term (E02, E03, E08, E09) — vừa nhớ xuyên session vừa cách ly
user. E09: `LOTUS-88`/Java/Spring Boot của Lan xuất hiện, `ORCHID-27` của Minh thì không, nhờ
search bằng `user_id` chứ không phải `graph_id`.

**Zep vs Redis + Qdrant:** Zep tự trích fact, gắn validity, tổng hợp xuyên thread; đổi lại
~1.1 s mỗi lần gọi và ingestion trễ bất định — seed phải nâng `ZEP_POLL_TIMEOUT` lên 900 s mới
qua stage 2. Redis + Qdrant latency gần 0, kiểm soát hoàn toàn, nhưng phải tự viết extraction,
conflict resolution, deletion.

**Guardrail:** `require_memory_consent` chặn durable write khi chưa opt-in; `minimize_pii`
redact email/phone trước khi gửi Zep; `heartbeat --dry-run` chỉ đọc; `forget.py --verify-only`
chứng minh right-to-be-forgotten (user absent: True, Redis keys: 0) còn shared KB giữ nguyên
vì chứa domain knowledge, không phải PII.
