Skip to content





Commands
Search files
Packager files
Commands
Search tools…
Hướng dẫn sử dụng & thêm Provider cho b.js
Wayground Multi-AI Solver Pro v4.0.0 — Userscript gắn lên Wayground/Quizizz, gọi nhiều AI free song song, bỏ phiếu chéo để chọn ra đáp án chính xác nhất.

Mục lục
Tổng quan
[Cài đặt](#2-cài đặt)
[Sử dụng](#3-sử dụng)
[Cấu hình (⚙)](#4-cấu hình-)
Quản lý Providers (🔌)
Thêm provider mới — từng bước
Cách hoạt động bên trong
Xử lý sự cố
1. Tổng quan
Hạng mục	Giá trị
Tên script	Wayground Multi-AI Solver Pro
Phiên bản	4.0.0
Trang chạy	https://*.wayground.com/*, https://*.quizizz.com/*
Quyền (@grant)	GM_getValue, GM_setValue, GM_xmlhttpRequest, GM_registerMenuCommand
Kết nối (@connect)	* (mọi domain API)
Lưu trữ	wg_multi_settings_v40 (cài đặt), wg_multi_cache_v40 (kết quả, TTL 3 ngày)
Tính năng chính:

Tự đọc câu hỏi + đáp án trên trang, gửi đồng thời tới các AI đã bật.
Hỗ trợ 12 provider: Groq, OpenRouter, Cerebras, Gemini, Together, Mistral, DeepSeek, xAI, Fireworks, Hyperbolic, SambaNova, GitHub Models.
Fallback model: nếu model chính lỗi (404/400/429/5xx) sẽ tự thử model tiếp theo trong danh sách.
Bỏ phiếu chéo: đếm phiếu (majority) hoặc cộng confidence (confidence).
Cache kết quả 3 ngày — câu hỏi cũ tra lại tức thì.
UI nổi, kéo thả được, có pháo hoa khi ra đáp án.
⚠️ Bảo mật: file b.js đang bị hard-code 2 API key mặc định (Groq + OpenRouter) trong DEFAULTS. Nếu bạn chia sẻ file, hãy xoá các key này đi, và tự thay bằng key của mình trong modal Providers.

2. Cài đặt
Cài extension quản lý script:
Tampermonkey (khuyến nghị), hoặc
Violentmonkey, hoặc Greasemonkey.
Tạo script mới → xoá nội dung mặc định → dán toàn bộ nội dung b.js → Lưu.
Vào https://wayground.com hoặc https://quizizz.com (bắt đầu một bài quiz).
Panel Multi-AI Solver sẽ tự xuất hiện ở góc trên bên phải.
Hoặc: kéo thả file b.js vào cửa sổ trình duyệt, Tampermonkey sẽ tự bắt và cài.

3. Sử dụng
Panel chính
┌─────────────────────────────────────┐
│ K  Multi-AI Solver        ⟳  ⚙  — │  ← tiêu đề, kéo để di chuyển
├─────────────────────────────────────┤
│ ✓ 3 · ✕ 1 · ⋯ (chip trạng thái)    │
│ Câu hỏi hiển thị tại đây...        │
│ ① Đáp án A                       │  ← click để tự chọn đáp án này
│ ② Đáp án B ✓ 2/3                  │  ← đáp án thắng, kèm số phiếu
│ ③ Đáp án C                       │
│ Lý do: [Groq] ...                  │
├─────────────────────────────────────┤
│ ● Tự giải │ ● Tự chọn │ Providers │  ← 3 nút toggles
└─────────────────────────────────────┘
Nút trên tiêu đề
Nút	Chức năng
⟳	Phân tích lại (bỏ qua cache)
⚙	Mở Cấu hình
—	Thu gọn / mở rộng panel
Toggle ở chân panel
Nút	Ý nghĩa (mặc định)
Tự giải (autoRun, BẬT)	Tự động phân tích mỗi khi sang câu hỏi mới (nhờ MutationObserver).
Tự chọn (autoClick, TẮT)	Tự bấm luôn đáp án thắng. Cẩn thận — nên để tắt nếu muốn kiểm tra tay.
Providers	Mở modal cấu hình API key / bật tắt provider.
Phím tắt & menu
Thao tác	Cách gọi
Phân tích lại	Ctrl + Shift + R hoặc nút ⟳
Cấu hình	Icon Tampermonkey → ⚙ Cấu hình
Quản lý key	Icon Tampermonkey → 🔌 Providers
Bật/tắt tự chọn	Icon Tampermonkey → 👆 Bật/tắt tự chọn
Xoá cache	Icon Tampermonkey → 🗑 Xoá cache
Click vào đáp án trong panel
Bấm vào bất kỳ đáp án nào trong danh sách → script cuộn tới ô tương ứng trên trang và giả lập click (pointerdown/mousedown/pointerup/mouseup/click).

4. Cấu hình (⚙)
Mở bằng nút ⚙ hoặc menu Tampermonkey.

Trường	Mặc định	Giải thích
Consensus	majority	majority = đáp án nào nhiều phiếu nhất thắng. confidence = đáp án nào tổng confidence lớn nhất thắng.
Temperature	0.1	Độ ngẫu nhiên (0–1). Câu trắc nghiệm nên để thấp (≤ 0.3).
Timeout (ms)	45000	Thời gian chờ tối đa mỗi request.
Max song song	6	Số provider được gọi đồng thời (1–20).
Hiện lý do	Có	Có hiển thị khối "Lý do" không.
Nhấn Lưu để áp dụng (lưu ngay vào GM_setValue, không cần reload).

5. Quản lý Providers (🔌)
Mở bằng nút Providers ở chân panel hoặc menu Tampermonkey.

🔌 Providers
─────────────────────────────────────────
[ Groq       ]  ••••________________     ← chip sáng xanh = ĐANG BẬT
[ OpenRouter ]  ••••________________
[ Gemini     ]  ____________________     ← chip mờ + "no key"
...
         [ Lưu tất cả ]  [ Đóng ]
─────────────────────────────────────────
Bấm chip để bật/tắt provider (chip xanh = bật).
Nhập key vào ô bên phải → ô sẽ hiện ••••xxxxxx nếu đã có key cũ.
Nhấn Lưu tất cả: key mới được lưu, provider có key mới sẽ tự bật, và script tự chạy phân tích lại ngay.
Để trống ô key = giữ nguyên key cũ.
Một provider chỉ thực sự được gọi khi vừa bật (chip xanh) vừa có key. Nếu không có provider nào hợp lệ, panel báo: "Chưa bật provider nào có API key".

Lấy key miễn phí
Provider	Địa chỉ lấy key	Key dạng
Groq	https://console.groq.com/keys	gsk_...
OpenRouter	https://openrouter.ai/settings/keys	sk-or-v1-...
Gemini	https://aistudio.google.com/apikey	AIza...
Cerebras	https://cloud.cerebras.ai/	csk-...
Together	https://api.together.xyz/settings/api-keys	...
Mistral	https://console.mistral.ai/api-keys	...
DeepSeek	https://platform.deepseek.com/api_keys	sk-...
xAI	https://console.x.ai/	xai-...
Fireworks	https://fireworks.ai/account/api-keys	fw_...
SambaNova	https://cloud.sambanova.ai/apis	...
GitHub Models	https://github.com/settings/personal-access-tokens	ghp_...
6. Thêm provider mới — từng bướst
Script gọi provider qua 2 đường:

callOpenAICompatible(id, data) — mọi endpoint chuẩn OpenAI (/v1/chat/completions, trả choices[0].message.content).
callGemini(data) — API riêng của Google (generateContent).
Muốn thêm provider chỉ cần thêm 1 entry object vào PROVIDERS — modal Providers sẽ tự hiển thị nó (modal duyệt bằng Object.entries(PROVIDERS)), không phải sửa UI.

Bước 1 — Thêm vào object PROVIDERS
Tìm khối const PROVIDERS = { ... } (khoảng dòng 104) và thêm, ví dụ provider openai (OpenAI chính chủ — chuẩn OpenAI nên chỉ cần 3 dòng):

const PROVIDERS = {
  // ... các provider cũ ...
  openai: {
    label: "OpenAI",                                   // tên hiển thị trong UI
    model: "gpt-4o-mini",                              // model mặc định
    url: "https://api.openai.com/v1/chat/completions", // endpoint
    openai: true,                                      // đánh dấu compatible
  },
};
Bước 2 (khuyến nghị) — Khai báo trong DEFAULTS
Thêm key rỗng + cờ bật/tắt để cấu hình mặc định rõ ràng và modal hiển thị đúng từ đầu:

const DEFAULTS = {
  keys: {
    // ... key cũ ...
    openai: "",
  },
  enabled: {
    // ... cờ cũ ...
    openai: false,
  },
  // ...
};
Bước 2 không bắt buộc: nếu thiếu, state.settings.keys.openai sẽ là undefined → coi như chưa có key, và modal vẫn tự tạo field khi bạn lưu key. Thêm vào DEFAULTS giúp code sạch hơn và tránh "no key" nhấp nháy lạ.

Bước 3 — Cấp key & dùng
Vào Providers → chip OpenAI tự xuất hiện → dán key → Lưu tất cả → script chạy phân tích ngay.

Bước 4 (tùy chọn) — Thêm model dự phòng
Muốn tự chuyển model khi model chính chết? Thêm mảng fallbackModels (thứ tự ưu tiên):

openai: {
  label: "OpenAI",
  model: "gpt-4o-mini",
  fallbackModels: [
    "gpt-4o-mini",
    "gpt-4o-mini-2024-07-18",
    "gpt-4.1-mini",
    "gpt-4.1-nano",
  ],
  url: "https://api.openai.com/v1/chat/completions",
  openai: true,
},
Script duyệt lần lượt; khi model nào thành công thì model đó được ghi vào state.settings.models[id] và được ưu tiên cho các lần sau (tính năng "chốt model sống").

Bước 5 (tùy chọn) — Các trường nâng cao
myprovider: {
  label: "My Provider",
  model: "model-mac-dinh",
  fallbackModels: ["model-so-2", "model-so-3"],
  url: "https://api.example.com/v1/chat/completions",
  openai: true,
  jsonMode: false,              // bỏ `response_format: {type:"json_object"}`
                                //   nếu endpoint không hỗ trợ JSON mode
  extraHeaders: {               // header riêng (nằm ngoài Authorization)
    "HTTP-Referer": "https://wayground.com",
    "X-Title": "Wayground Solver",
  },
},
Bảng trường tham chiếu
Trường	Bắt buộc	Mô tả
label	✅	Tên hiển thị trên chip, status, toast.
model	✅	Model mặc định.
url	✅ (trừ Gemini)	Endpoint chat completions.
fallbackModels	⬜	Mảng model dự phòng, thử theo thứ tự.
openai	⬜	Đánh dấu compatible (hiện chủ yếu để ghi chú).
jsonMode	⬜	false để tắt response_format JSON. Mặc định bật.
extraHeaders	⬜	Object header thêm vào request.
custom	⬜	'gemini' → đi qua callGemini().
Provider KHÔNG compatible OpenAI (VD: Anthropic)
Nếu API không chuẩn OpenAI (không có choices[0].message.content), phải viết hàm gọi riêng như callGemini:

// 1. Khai báo trong PROVIDERS với cờ riêng
anthropic: {
  label: "Anthropic",
  model: "claude-3-5-haiku-latest",
  custom: "anthropic",        // ← đánh dấu để route
},
// 2. Viết hàm gọi (kiểu mẫu lấy từ callGemini)
async function callAnthropic(data) {
  const key = state.settings.keys.anthropic;
  if (!key) throw new Error("Anthropic: thiếu key");
  const { system, user } = buildPrompt(data);
  const raw = await httpRequest({
    url: "https://api.anthropic.com/v1/messages",
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "x-api-key": key,
      "anthropic-version": "2023-06-01",
    },
    data: JSON.stringify({
      model: PROVIDERS.anthropic.model,
      max_tokens: 1500,
      system,
      messages: [{ role: "user", content: user }],
    }),
    timeout: state.settings.timeout,
  });
  const parsed = JSON.parse(raw);
  const text = parsed?.content?.[0]?.text;
  if (!text) throw new Error("response rỗng");
  return parseJsonLoose(text);   // ép về JSON theo schema chung
}
// 3. Đăng ký vào hàm router callProvider
const callProvider = (id, data) => {
  const custom = PROVIDERS[id]?.custom;
  if (custom === "gemini") return callGemini(data);
  if (custom === "anthropic") return callAnthropic(data);
  return callOpenAICompatible(id, data);
};
Lưu ý quan trọng khi viết hàm custom:

Hàm phải throw nếu thiếu key (thiếu key) và throw nếu response rỗng — script dựa vào exception để đánh dấu chip fail.
Phải trả về object JSON đúng schema:
{ "answer_index": 2, "answer_text": "Nguyên văn đáp án", "confidence": 0.9, "reason": "Ngắn gọn" }
Nếu endpoint không trả JSON sạch, dùng parseJsonLoose(text) (hàm có sẵn) để tách JSON khỏi text thừa.
Muốn fallback model thì tự bọc vòng lặp như callGemini (danh sách fallbackModels, bắt lỗi 404/model... thì sang model tiếp).
Checklist nhanh khi thêm provider
 Entry mới trong PROVIDERS (key id không trùng, viết thường, không dấu).
 keys.<id> và enabled.<id> trong DEFAULTS.
 Đã test: chip hiện trong modal, nhập key được, bật được, chip status chuyển ok.
 Nếu dùng jsonMode: false → kiểm tra prompt vẫn ép được model trả JSON (schema ở buildPrompt).
 Bump version script (@version) nếu muốn người dùng nhận bản mới.
7. Cách hoạt động bên trong
Luồng xử lý mỗi câu hỏi:

MutationObserver phát hiện DOM đổi
        │
        ▼
readQuestion()  →  #questionText  (câu hỏi)
                   .options-grid button[role="option"]  (đáp án)
        │
        ▼
signatureOf() = câu hỏi + các đáp án  →  tra cache (TTL 3 ngày)
        │  (trùng → hiển thị ngay, bỏ qua AI)
        ▼
active = PROVIDERS có enabled=true VÀ có key
        │
        ▼
Queue + maxParallel worker song song
   mỗi worker: callProvider(id)
        │        ├─ openrouter: thêm reasoning effort "low"
        │        ├─ có jsonMode → gửi response_format json_object
        │        └─ lỗi model (404/400/429/5xx/...) → thử model kế
        ▼
vote(answers, options)  →  tally theo đáp án
        │
        ├─ consensusMode = "majority"  → nhiều phiếu nhất thắng
        └─ consensusMode = "confidence"→ tổng confidence cao nhất thắng
        │
        ▼
resolveTarget(): khớp theo text (chính xác / contains) → theo index → theo 25 ký tự đầu
        │
        ▼
render + toast + confetti + (autoClick nếu bật) + ghi cache
Một số chi tiết đáng chú ý:

Lỗi 401/403 (sai key) → dừng ngay provider đó, không thử model kế — vì key sai thì model nào cũng sai.
Chạy lại bằng Ctrl+Shift+R hoặc nút ⟳ là force: true → bỏ qua cache.
Token chống chạy đè (runToken): mỗi lần solve là một token; token cũ bị hủy nếu bạn bấm phân tích giữa chừng.
Nếu thiếu GM_* (dùng Greasemonkey cũ/vanilla browser), script tự fallback sang localStorage.
8. Xử lý sự cố
Hiện tượng	Nguyên nhân	Cách sửa
"Chưa bật provider nào có API key"	Chưa có provider nào vừa bật vừa có key.	Mở Providers, nhập key, Lưu tất cả.
Chip provider màu đỏ fail	Xem Console (F12) dòng [MultiAI] <id> <lỗi>.	
↳ 401/403	Key sai/hết hạn/quyền.	Lấy lại key. Lỗi này không thử model khác.
↳ 404 / model not found	Model bị đổi tên/xoá.	Thêm fallbackModels hoặc sửa model.
↳ 429 rate limit	Hết quota free.	Tắt provider đó đi, tăng số provider khác.
↳ Timeout	Mạng chậm / endpoint treo.	Tăng Timeout trong ⚙ (VD 60000–90000).
↳ AI không trả JSON	Model trả text không phải JSON.	Thử model khác trong fallback; hoặc đặt jsonMode: false cho provider đó.
Chip xám no key	Chưa nhập key.	Nhập key.
Panel không hiện	Trang không khớp @match, hoặc script tắt.	Kiểm tra icon Tampermonkey có dấu chấm xanh; vào đúng wayground.com/quizizz.com.
Không tự sang câu mới	Tự giải đang tắt.	Bấm toggle Tự giải.
Đáp án hiển thị sai ô	Selector của trang đã đổi (site update).	Cập nhật selector trong readQuestion().
Đáp án bị cache cũ	Cache TTL 3 ngày.	Menu Tampermonkey → 🗑 Xoá cache, hoặc ⟳.
Tham chiếu nhanh các hằng số
Hằng số	Giá trị	Ở đâu
SETTINGS_KEY	wg_multi_settings_v40	Dòng ~20
CACHE_KEY	wg_multi_cache_v40	Dòng ~21
CACHE_TTL	3 ngày (ms)	Dòng ~22
max_tokens gửi đi	1500 (OpenAI-like) / 600 (Gemini)	Trong hàm call
Debounce observer	700 ms	watch()
Số provider mặc định	12	PROVIDERS


Scroll to latest







HatefulVerifiableTutorial - Replit
