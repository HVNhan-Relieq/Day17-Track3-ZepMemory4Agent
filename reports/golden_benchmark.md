# Lab 17 Golden Set Report

- Implementation: `student`
- Kind: `golden`
- Cases: **20**
- Passed: **20/20**
- Evidence hit rate: **100.0%**
- Average retrieval latency: **885.6 ms**
- Average token reduction vs full source context: **6.3%**
- Golden bonus: **10/10** (100% required)

| Case | Layer | Pass | Latency ms | Retrieved tokens | Token reduction | Missing / Error |
| --- | --- | --- | ---: | ---: | ---: | --- |
| G01 | short_term | PASS | 0.2 | 227 | 0.0% |  |
| G02 | short_term | PASS | 0.0 | 133 | 0.0% |  |
| G08 | long_term | PASS | 1510.4 | 788 | 0.0% |  |
| G09 | long_term | PASS | 1382.1 | 1382 | 0.0% |  |
| G12 | semantic | PASS | 243.6 | 418 | 8.9% |  |
| G14 | semantic | PASS | 243.1 | 270 | 30.2% |  |
| G15 | semantic | PASS | 256.5 | 270 | 41.2% |  |
| G19 | mixed | PASS | 1573.9 | 581 | 0.0% |  |
| G03 | long_term | PASS | 1357.2 | 1366 | 0.0% |  |
| G04 | long_term | PASS | 1313.1 | 1379 | 0.0% |  |
| G05 | long_term | PASS | 1300.9 | 1380 | 0.0% |  |
| G10 | episodic | PASS | 246.2 | 289 | 0.0% |  |
| G11 | episodic | PASS | 244.8 | 289 | 0.0% |  |
| G13 | semantic | PASS | 245.8 | 416 | 26.4% |  |
| G16 | mixed | PASS | 1579.3 | 581 | 0.0% |  |
| G18 | mixed | PASS | 487.5 | 500 | 11.5% |  |
| G20 | mixed | PASS | 1762.8 | 831 | 0.0% |  |
| G06 | long_term | PASS | 1266.9 | 1378 | 0.0% |  |
| G07 | long_term | PASS | 1237.6 | 1374 | 0.0% |  |
| G17 | mixed | PASS | 1461.1 | 581 | 8.1% |  |

## Evidence excerpts

### G01 - short_term

`<SESSION_SUMMARY> user: Constraint HOLD-ALPHA-0900: standup is 09:00 sharp and must not be forgotten. | assistant: Noted standup constraint. | user: Constraint HOLD-BETA-STAGING: writes go to staging DB only. | assistant: Noted staging constraint. | user: Filler A about button padding. | assistant: Filler A. | user: Filler B about color tokens. | assistant: Filler B. | user: Filler C about copy tone. | assistant: Filler C. </SESSION_SUMMARY> <DURABLE_NOTES> - user: Constraint HOLD-ALPHA-0900: standup is 09:00 sharp and must not be forgotten. - assistant: Noted standup constraint. - user: Constraint HOLD-BETA-STAGING: writes go to staging DB only. - assistant: Noted staging constraint. </DURA`

### G02 - short_term

`<RECENT_TURNS> user: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. assistant: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. user: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. assistant: Toi se uu tien timeline khi giai thich coroutine va Task. user: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. </RECENT_TURNS>`

### G08 - long_term

`<USER_SUMMARY> Lan's current project is LOTUS-88. They prioritize using Java and Spring Boot for backend development and do not use Python in the backend.  Lan prefers Java and Spring Boot and explicitly avoids using Python for backend development. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 11:00:00     Source: message     Content: [user] {   "user_id": "lan-lab17",   "first_name": "Lan",   "last_name": "Tran",   "user_alias": "Lan Tran" }: Toi la Lan. Du an cua toi la LOTUS-88. Toi uu tien Java va Spring Boot, va khong dung Python trong vi du backend.   - Created At: 2026-08-01 11:00:20     Source: messag`

### G09 - long_term

`<USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episodes are sour`

### G12 - semantic

`EPISODE: {"id":"kb-payment-retry","entity":"Payment API Retry Policy","summary":"For POST /payments, every retryable request MUST send the same Idempotency-Key. Retry only HTTP 429 or transient 5xx errors, use exponential-backoff, and stop after max-3-retries. Marker: PAYMENT-RULE-3.","source":"internal-api-guideline-v3","updated_at":"2026-08-10T00:00:00Z"} metadata= EPISODE: For POST /payments, every retryable request MUST send the same Idempotency-Key. Retry only HTTP 429 or transient 5xx errors, use exponential-backoff, and stop after max-3-retries. Marker: PAYMENT-RULE-3. metadata= EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal `

### G14 - semantic

`EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL.","source":"memory-governance-policy","updated_at":"2026-08-12T00:00:00Z"} metadata= EPISODE: Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL. metadata= EPISODE: {"id":"kb-context-budget","entity":"Memory Context Budget","summary":"Reserve bounded context for memory. This lab uses short-term 10 percent, long-term 4 percent, episodi`

### G15 - semantic

`EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL.","source":"memory-governance-policy","updated_at":"2026-08-12T00:00:00Z"} metadata= EPISODE: Do not persist personal data without explicit opt-in. A deletion request must remove user-scoped memory and be verified across every store. Marker: DELETE-VERIFY-ALL. metadata= EPISODE: {"id":"kb-context-budget","entity":"Memory Context Budget","summary":"Reserve bounded context for memory. This lab uses short-term 10 percent, long-term 4 percent, episodi`

### G19 - mixed

`<LONG_TERM> <USER_SUMMARY> Lan's current project is LOTUS-88. They prioritize using Java and Spring Boot for backend development and do not use Python in the backend.  Lan prefers Java and Spring Boot and explicitly avoids using Python for backend development. </USER_SUMMARY>  <EPISODES> Episodes are source message or document excerpts shown in selection order.   - Created At: 2026-08-01 11:00:00     Source: message     Content: [user] {   "user_id": "lan-lab17",   "first_name": "Lan",   "last_name": "Tran",   "user_alias": "Lan Tran" }: Toi la Lan. Du an cua toi la LOTUS-88. Toi uu tien Java va Spring Boot, va khong dung Python trong vi du backend.   - Created At: 2026-08-01 11:00:20     So`

### G03 - long_term

`<USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episodes are sour`

### G04 - long_term

`<USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episodes are sour`

### G05 - long_term

`<USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episodes are sour`

### G10 - episodic

`EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. EPISODE: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. EPISODE: Toi se uu tien timeline khi giai thich coroutine va Task. EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. EPISODE: Hom nay toi debug async HTTP. Toi da thu tang timeout len 60s nhung van fail. EPISODE: Hay kiem tra connection pool, lifecycle cua client va concurrency. EPISODE: Cach hieu qua la reuse a`

### G11 - episodic

`EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. EPISODE: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. EPISODE: Toi se uu tien timeline khi giai thich coroutine va Task. EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. EPISODE: Hom nay toi debug async HTTP. Toi da thu tang timeout len 60s nhung van fail. EPISODE: Hay kiem tra connection pool, lifecycle cua client va concurrency. EPISODE: Cach hieu qua la reuse a`

### G13 - semantic

`EPISODE: {"id":"kb-async-http","entity":"Async HTTP Incident Playbook","summary":"When async HTTP calls time out, inspect connection pooling, downstream saturation and concurrency before increasing timeout. Reuse a long-lived client session where possible. Marker: CONN-POOL-FIRST.","source":"incident-playbook-2026","updated_at":"2026-08-11T00:00:00Z"} metadata= EPISODE: When async HTTP calls time out, inspect connection pooling, downstream saturation and concurrency before increasing timeout. Reuse a long-lived client session where possible. Marker: CONN-POOL-FIRST. metadata= EPISODE: {"id":"kb-memory-privacy","entity":"Agent Memory Privacy Rule","summary":"Do not persist personal data witho`

### G16 - mixed

`<LONG_TERM> <USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episo`

### G18 - mixed

`<EPISODIC> EPISODE: Ten du an ca nhan cua toi la ORCHID-27. Toi thich Python va khong thich Java. Khi giai thich code, hay dung vi du ngan. EPISODE: Da hieu: demo ca nhan ORCHID-27, uu tien Python, tranh Java, vi du ngan. EPISODE: Toi dang hoc async/await va hay nham coroutine voi Task. Neu sau nay gap chu de nay, hay giai thich bang timeline. EPISODE: Toi se uu tien timeline khi giai thich coroutine va Task. EPISODE: TODO: hoan thanh benchmark report truoc thu Sau luc 16:00. Day la open loop LAB-REPORT-1600. EPISODE: Hom nay toi debug async HTTP. Toi da thu tang timeout len 60s nhung van fail. EPISODE: Hay kiem tra connection pool, lifecycle cua client va concurrency. EPISODE: Cach hieu qua`

### G20 - mixed

`<LONG_TERM> <USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episo`

### G06 - long_term

`<USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episodes are sour`

### G07 - long_term

`<USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episodes are sour`

### G17 - mixed

`<LONG_TERM> <USER_SUMMARY> The user's personal project is named ORCHID-27. The user has a deadline to complete a benchmark report by Saturday at 16:00, identified as open loop LAB-REPORT-1600. The user is currently debugging async HTTP requests, specifically a connection churn issue identified as ASYNC-FIX-20. Increasing the timeout did not resolve the issue, but reusing the aiohttp ClientSession and setting concurrency to 20 did. The user asked to check the connection pool, client lifecycle, and concurrency.  Minh prefers Python and dislikes Java. When explaining code, use short examples. Minh will prioritize the timeline when explaining coroutine and Task. </USER_SUMMARY>  <EPISODES> Episo`
