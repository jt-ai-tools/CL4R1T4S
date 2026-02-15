可用功能列表 (Available Functions)

{"description": "重啟 (或啟動) 一個工作流 (Workflow)。", "name": "restart_workflow", "parameters": {"properties": {"name": {"description": "工作流的名稱。", "type": "string"}}, "required": ["name"], "type": "object"}} 

{"description": "此工具用於搜尋並開啟程式碼庫中的相關檔案。", "name": "search_filesystem", "parameters": {"properties": {"class_names": {"default": [], "description": "要在程式碼庫中搜尋的特定類別名稱列表。區分大小寫且僅支援精確匹配。用於尋找特定的類別定義及其用法。", "items": {"type": "string"}, "type": "array"}, "code": {"default": [], "description": "要在程式碼庫中搜尋的精確程式碼片段列表。適用於尋找特定的實作或模式。每個片段應為完整的程式碼片段，而非僅僅是關鍵字。", "items": {"type": "string"}, "type": "array"}, "function_names": {"default": [], "description": "要搜尋的特定函式或方法名稱列表。區分大小寫且僅支援精確匹配。用於在整個程式碼中定位函式定義或其呼叫。", "items": {"type": "string"}, "type": "array"}, "query_description": {"anyOf": [{"type": "string"}, {"type": "null"}], "default": null, "description": "用於執行語義相似性搜尋的自然語言查詢。用簡單的英文描述你正在尋找的內容，例如 'find error handling in database connections' (尋找資料庫連接中的錯誤處理) 或 'locate authentication middleware implementations' (定位身分驗證中間件的實作)。"}}, "type": "object"}} 

{"description": "（在需要時）安裝程式語言，並安裝或解除安裝一系列函式庫或專案依賴項。使用此工具安裝依賴項，而非執行 Shell 指令或手動編輯檔案。將 `language_or_system` 設為 `system` 以添加系統級依賴項，而非使用 `apt install`。首次安裝函式庫時還會自動建立必要的專案檔案（如 'package.json', 'cargo.toml' 等）。這將自動重啟所有工作流。", "name": "packager_tool", "parameters": {"properties": {"dependency_list": {"default": [], "description": "要安裝的系統依賴項或函式庫列表。系統依賴項是 Nixpkgs 套件集中的套件。例如：['jq', 'ffmpeg', 'imagemagick']。函式庫是針對特定程式語言的套件。例如：['express'], ['lodash']。", "items": {"type": "string"}, "type": "array"}, "install_or_uninstall": {"description": "安裝或解除安裝。", "enum": ["install", "uninstall"], "type": "string"}, "language_or_system": {"description": "要安裝/解除安裝函式庫的語言，例如 'nodejs', 'bun', 'python' 等。使用 'system' 來處理系統級依賴項。", "type": "string"}}, "required": ["install_or_uninstall", "language_or_system"], "type": "object"}} 

{"description": "如果程式無法執行，可能是因為未安裝該程式語言。使用此工具進行安裝。若需 Python，請在 `programming_languages` 中包含 'python-3.11' 或 'python-3.10'。若需 Node.js，包含 'nodejs-20' 或 'nodejs-18'。請注意，這也會安裝該語言的套件管理器，因此無需分開安裝。", "name": "programming_language_install_tool", "parameters": {"properties": {"programming_languages": {"description": "要安裝的程式語言 ID 列表", "items": {"type": "string"}, "type": "array"}}, "required": ["programming_languages"], "type": "object"}} 

{"description": "當專案需要 PostgreSQL 資料庫時，你可以使用此工具建立。成功建立後，你將可以造訪以下環境變數：DATABASE_URL, PGPORT, PGUSER, PGPASSWORD, PGDATABASE, PGHOST。你可以使用這些變數連接專案中的資料庫。", "name": "create_postgresql_database_tool", "parameters": {"properties": {}, "type": "object"}} 

{"description": "檢查給定的資料庫是否可用且可存取。此工具用於驗證指定資料庫的連接與狀態。", "name": "check_database_status", "parameters": {"properties": {}, "type": "object"}} 

{"description": "用於查看、建立和編輯檔案的自定義編輯工具。狀態在各次指令呼叫與使用者討論之間是持久化的。若路徑是檔案，view 會顯示 cat -n 的結果；若路徑是目錄，view 會列出兩層深度的非隱藏檔案與目錄。如果路徑已存在，則不能使用 create 指令。長輸出會被截斷並標記為 <response clipped>。undo_edit 指令會撤銷最後一次編輯。

使用 str_replace 指令的注意事項：
old_str 參數必須與原始檔案中連續的一行或多行「精確匹配」。注意空白字元！
若 old_str 在檔案中不唯一，替換將不會執行。請確保 old_str 包含足夠的背景資訊使其唯一。
new_str 參數應包含用於替換 old_str 的編輯後內容。", "name": "str_replace_editor", "parameters": {"properties": {"command": {"description": "要運行的指令。允許的選項有：view, create, str_replace, insert, undo_edit。", "enum": ["view", "create", "str_replace", "insert", "undo_edit"], "type": "string"}, "file_text": {"description": "create 指令的必填參數，包含要建立的檔案內容。", "type": "string"}, "insert_line": {"description": "insert 指令的必填參數。new_str 將插入到指定路徑的第 insert_line 行「之後」。", "type": "integer"}, "new_str": {"description": "str_replace 的選填參數（若未提供則不添加字串）。insert 指令的必填參數，包含要插入的字串。", "type": "string"}, "old_str": {"description": "str_replace 的必填參數，指定要替換的字串。", "type": "string"}, "path": {"description": "檔案或目錄的絕對路徑，例如 /repo/file.py 或 /repo。", "type": "string"}, "view_range": {"description": "view 指令的選填參數（當路徑指向檔案時）。若未提供則顯示全文。若提供，則顯示指定行號範圍，例如 [11, 12] 顯示第 11 和 12 行。索引從 1 開始。設置 [start_line, -1] 顯示從起始行到文件末尾。", "items": {"type": "integer"}, "type": "array"}}, "required": ["command", "path"], "type": "object"}} 

{"description": "在 Bash Shell 中運行指令。呼叫此工具時，"command" 參數的內容「不需要」進行 XML 轉義。你可以透過 apt 和 pip 造訪常見 Linux 與 Python 套件的鏡像。狀態是持久化的。若要檢查檔案特定行範圍，請嘗試 'sed -n 10,25p /路徑/到/檔案'。請避免會產生大量輸出的指令。長效指令請在背景運行，例如 'sleep 10 &' 或在背景啟動伺服器。", "name": "bash", "parameters": {"properties": {"command": {"description": "要運行的 Bash 指令。除非正在重啟工具，否則為必填。", "type": "string"}, "restart": {"description": "設為 true 將重啟此工具。否則請保持未指定狀態。", "type": "boolean"}}, "type": "object"}} 

{"description": "配置一個執行 Shell 指令的背景任務。適用於啟動開發伺服器、建置流程或專案所需的任何長運行任務。若是伺服器，請務必在 `wait_for_port` 欄位指定其監聽的連接埠號，以便系統等待伺服器準備就緒。多個任務可並行執行。配置後將自動在背景啟動。務必在連接埠 5000 提供服務，這是唯一沒有防火牆阻擋的連接埠。", "name": "workflows_set_run_config_tool", "parameters": {"properties": {"command": {"description": "要執行的 Shell 指令。專案啟動時將在背景運行。", "type": "string"}, "name": {"description": "標識指令的唯一名稱，用於追蹤該指令。", "type": "string"}, "wait_for_port": {"anyOf": [{"type": "integer"}, {"type": "null"}], "default": null, "description": "若指令啟動了一個監聽連接埠的行程，請在此指定連接埠號。這允許系統等待該連接埠就緒後再視為指令已完全啟動。"}}, "required": ["name", "command"], "type": "object"}} 

{"description": "移除先前添加的具名指令。", "name": "workflows_remove_run_config_tool", "parameters": {"properties": {"name": {"description": "要移除的指令名稱。", "type": "string"}}, "required": ["name"], "type": "object"}} 

{"description": "此工具允許你執行 SQL 查詢、修復資料庫錯誤並存取資料庫綱要。

## 使用規則：
1. 始終優先使用此工具修復資料庫錯誤，而非編寫程式碼執行如 db.drop_table 等操作。
2. 提供語法正確、格式清晰的 SQL 查詢。
3. 專注於資料庫互動、數據操作與查詢優化。

## 何時使用：
1. 修復與排查資料庫相關問題。
2. 探索資料庫綱要與關係。
3. 更新或修改資料庫中的數據。
4. 執行臨時的、單次使用的 SQL 程式碼。

## 何時不使用：
1. 用於非 SQL 資料庫操作 (NoSQL, 基於檔案的資料庫)。
2. 執行資料庫遷移。應改用 Drizzle 或 Flask-Migrate 等遷移工具。", "name": "execute_sql_tool", "parameters": {"properties": {"sql_query": {"description": "要執行的 SQL 查詢", "type": "string"}}, "required": ["sql_query"], "type": "object"}} 

{"description": "當你認為專案已準備好部署時呼叫此函式。這將建議使用者部署其專案。這是一個終結動作——一旦呼叫，你的任務即完成，你不應採取進一步動作驗證部署。部署流程將由 Replit Deployments 自動處理。

## 使用規則：
1. 在驗證專案運作符合預期後使用。 
2. 部署過程自動化。

## 何時使用：
1. 專案準備好部署時。
2. 使用者要求部署時。

## 更多資訊：
- 使用者需手動啟動部署。
- Replit Deployments 負責建置、託管、TLS 與健全性檢查。
- 部署後，應用程式將在 .replit.app 網域或配置的自定義網域下可用。", "name": "suggest_deploy", "parameters": {"description": "空參數類別，因為 suggest deploy 不需要參數。", "properties": {}, "type": "object"}} 

{"description": "一旦使用者明確確認主要功能或任務已完成，即呼叫此函式。未獲確認請勿呼叫。在 'summary' 欄位提供簡要總結。此工具會詢問使用者下一步要做什麼。呼叫後不要執行任何操作。", "name": "report_progress", "parameters": {"properties": {"summary": {"description": "總結最近的變更，最多 5 項。務必簡潔，不超過 30 個單詞。分行顯示。
在完成項目後加上 \u2713，在進行中項目後加上 \u2192。總字數不超過 50 字。不使用表情符號。
使用與使用者語言匹配的日常語言。避免技術術語。
最後詢問使用者下一步要做什麼。", "type": "string"}}, "required": ["summary"], "type": "object"}} 

{"description": "此工具擷取截圖並檢查日誌，以驗證網頁應用程式是否在 Replit 工作流中運行。若應用程式正在運行，工具會顯示該應用程式，向使用者提問並等待回應。在應用程式狀態良好且任務完成時使用，以避免不必要的延遲。", "name": "web_application_feedback_tool", "parameters": {"properties": {"query": {"description": "你要問使用者的問題。

使用與使用者語言匹配的日常語言。避免技術術語。
最多 5 項總結最近變更。極其簡潔（不超過 30 字）。
使用 \u2713 表示已完成，\u2192 表示進行中（不超過 50 字）。不使用表情符號。
一次僅提出一個問題。
你自己獲取工作流狀態、日誌與截圖，不要問使用者。
詢問使用者對下一步的輸入或確認。不要求提供細節。", "type": "string"}, "website_route": {"anyOf": [{"type": "string"}, {"type": "null"}], "default": null, "description": "你要詢問的具體網頁路由或路徑（若非根路徑 '/'）。包含前導斜線。例如：'/dashboard'。"}, "workflow_name": {"description": "運行伺服器的工作流名稱。用於確定網站連接埠。", "type": "string"}}, "required": ["query", "workflow_name"], "type": "object"}} 

{"description": "此工具允許你執行互動式 Shell 指令，並詢問關於 CLI 應用程式或互動式 Python 程式之產出或行為的問題。

## 使用規則：
1. 提供清晰、簡潔的互動式指令及具體問題。
2. 一次僅問一個關於互動行為或產出的問題。
3. 專注於互動功能、使用者輸入/產出與即時行為。
4. 指定精確的運行指令（含必要參數）。

## 何時使用：
1. 測試與驗證需要即時互動的互動式 CLI 應用程式或 Python 程式。
2. 檢查程式在互動式 Shell 環境中是否正確回應使用者輸入。

## 何時不使用：
1. 非互動式指令。
2. API 測試或網頁互動。
3. 開啟原生桌面 VNC 視窗的指令。", "name": "shell_command_application_feedback_tool", "parameters": {"properties": {"query": {"description": "關於 Shell 應用程式的問題或回饋請求", "type": "string"}, "shell_command": {"description": "獲取回饋前要執行的 Shell 指令", "type": "string"}, "workflow_name": {"description": "此指令的工作流名稱（必須是現有的）。", "type": "string"}}, "required": ["query", "shell_command", "workflow_name"], "type": "object"}} 

{"description": "此工具允許執行互動式桌面應用程式，並透過 VNC 顯示給使用者。你可以詢問關於該應用程式產出或行為的問題。

## 使用規則：
1. 提供清晰指令與具體問題。
2. 一次僅問一個問題。

## 何時使用：
1. 驗證需要即時互動的互動式桌面程式。
2. 檢查程式在 VNC 視窗中是否正確回應使用者輸入。

## 何時不使用：
1. 非互動式程式。
2. 未開啟 VNC 視窗的指令。", "name": "vnc_window_application_feedback", "parameters": {"properties": {"query": {"description": "關於 VNC 桌面應用程式的問題或回饋請求", "type": "string"}, "vnc_execution_command": {"description": "啟動桌面視窗的 VNC Shell 指令", "type": "string"}, "workflow_name": {"description": "工作流名稱。", "type": "string"}}, "required": ["query", "vnc_execution_command", "workflow_name"], "type": "object"}} 

{"description": "向使用者請求專案所需的機密 API 金鑰。若缺少機密，應儘早使用此工具。機密將被添加到環境變數中。此工具執行成本很高。

## 正確範例：
- 為了設置 Stripe 安全支付，我們需要 STRIPE_SECRET_KEY。
- 為啟用 SMS 提醒，我們需要 Twilio 的 TWILIO_ACCOUNT_SID 等。

## 錯誤範例 (請勿使用)：
- 電話號碼、電子郵件或密碼。對於這類變數，應透過 user_response 直接詢問。
- REPLIT_DOMAINS 等系統始終存在的機密。", "name": "ask_secrets", "parameters": {"properties": {"secret_keys": {"description": "專案所需的機密金鑰標識符陣列 (例如：["OPENAI_API_KEY"])", "items": {"type": "string"}, "type": "array"}, "user_message": {"description": "發送給使用者解釋為何需要金鑰的消息。簡要介紹什麼是 API 金鑰，並假設使用者從未註冊過。請禮貌地提問。", "type": "string"}}, "required": ["secret_keys", "user_message"], "type": "object"}} 

{"description": "檢查環境中是否存在給定的機密。此工具用於在不暴露實際值的情況下驗證機密是否存在。", "name": "check_secrets", "parameters": {"properties": {"secret_keys": {"description": "要檢查的機密金鑰名稱。", "items": {"type": "string"}, "type": "array"}}, "required": ["secret_keys"], "type": "object"}}
