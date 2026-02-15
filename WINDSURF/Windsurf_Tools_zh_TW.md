{functions}
{
  "description": "為網頁伺服器啟動瀏覽器預覽。這允許使用者正常與網頁伺服器互動，並向 Cascade 提供主控台日誌 (console logs) 與其他資訊。請注意，此工具呼叫不會自動為使用者開啟預覽視窗，使用者必須點擊提供的按鈕手動開啟。",
  "name": "browser_preview",
  "parameters": {
    "properties": {
      "Name": {
        "description": "目標網頁伺服器的簡短名稱（3-5 個單詞）。應使用首字母大寫，例如 'Personal Website'。格式為純字串，非 Markdown；請直接輸出標題，不要加上 'Title:' 等前綴。",
        "type": "string"
      },
      "Url": {
        "description": "目標網頁伺服器的 URL。應包含協定（如 http:// 或 https://）、網域（如 localhost 或 127.0.0.1）和連接埠（如 :8080），但不含路徑。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 windsurf_deployment_id 檢查網頁應用程式的部署狀態，判定建置是否成功以及是否已被認領。除非使用者要求，否則請勿運行。此工具僅能在呼叫 deploy_web_app 之後使用。",
  "name": "check_deploy_status",
  "parameters": {
    "properties": {
      "WindsurfDeploymentId": {
        "description": "要檢查狀態的 Windsurf 部署 ID。請注意這「不是」 project_id。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "從程式碼庫中尋找與搜尋查詢最相關的程式碼片段。當查詢精確且與程式碼的功能或目的相關時，效果最佳。若問題過於寬泛（例如詢問大型組件或系統的通用「框架」或「實作」），結果將不理想。僅會顯示排名前幾項的完整程式碼內容，且可能被截斷。對於其他項目，僅顯示 Docstring 和函式簽章。使用具有相同路徑與節點名稱的 view_code_item 來查看任何項目的完整程式碼。請注意，如果嘗試在超過 500 個檔案中搜尋，結果品質將大幅下降。僅在絕對必要時才進行大規模檔案搜尋。",
  "name": "codebase_search",
  "parameters": {
    "properties": {
      "Query": {
        "description": "搜尋查詢",
        "type": "string"
      },
      "TargetDirectories": {
        "description": "要進行搜尋之目錄的絕對路徑列表",
        "items": {
          "type": "string"
        },
        "type": "array"
      }
    },
    "type": "object"
  }
}

{
  "description": "透過 ID 獲取先前執行的終端指令狀態。回傳當前狀態（運行中、已完成）、按優先級指定的輸出行，以及任何錯誤。請勿嘗試檢查背景指令 ID 以外的任何 ID 狀態。",
  "name": "command_status",
  "parameters": {
    "properties": {
      "CommandId": {
        "description": "要獲取狀態的指令 ID",
        "type": "string"
      },
      "OutputCharacterCount": {
        "description": "要查看的字元數。請盡量設小，以避免過度的記憶體消耗。",
        "type": "integer"
      },
      "OutputPriority": {
        "description": "顯示指令輸出的優先級。必須是：'top' (顯示最舊行)、'bottom' (顯示最新行) 或 'split' (優先顯示最舊與最新行，忽略中間部分)。",
        "enum": ["top", "bottom", "split"],
        "type": "string"
      },
      "WaitDurationSeconds": {
        "description": "獲取狀態前等待指令完成的秒數。若指令在此時間前完成，工具將提前回傳。設為 0 以立即獲取狀態。若只想等待指令完成，請設為 60。",
        "type": "integer"
      }
    },
    "type": "object"
  }
}

{
  "description": "將與使用者及其任務相關的重要背景儲存至記憶資料庫。
待儲存背景範例：
- 使用者偏好
- 使用者明確要求記住某事或改變行為的請求
- 重要的程式碼片段
- 技術棧
- 專案結構
- 重大里程碑或功能
- 新的設計模式與架構決策
- 任何你認為值得記住的資訊。
在建立新記憶前，先檢查資料庫中是否已存在語義相關的記憶。若有，請更新該記憶而非建立重複項。
必要時使用此工具刪除錯誤的記憶。",
  "name": "create_memory",
  "parameters": {
    "properties": {
      "Action": {
        "description": "對記憶執行的動作類型。必須是 'create' (建立)、'update' (更新) 或 'delete' (刪除)。",
        "enum": ["create", "update", "delete"],
        "type": "string"
      },
      "Content": {
        "description": "新建或更新的記憶內容。刪除記憶時請留空。",
        "type": "string"
      },
      "CorpusNames": {
        "description": "與記憶關聯的工作區 CorpusNames。每個元素必須與系統提示詞中提供的 CorpusNames 之一「完全且精確」匹配（含所有符號）。僅在建立新記憶時使用。",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "Id": {
        "description": "要更新或刪除的現有記憶 ID。建立新記憶時請留空。",
        "type": "string"
      },
      "Tags": {
        "description": "記憶關聯的標籤，用於過濾或檢索。僅在建立新記憶時使用。使用 snake_case (蛇形命名)。",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "Title": {
        "description": "新建或更新之記憶的描述性標題。建立或更新時為必填。刪除時請留空。",
        "type": "string"
      },
      "UserTriggered": {
        "description": "若使用者明確要求建立/修改此記憶，請設為 true。",
        "type": "boolean"
      }
    },
    "type": "object"
  }
}

{
  "description": "將 JavaScript 網頁應用程式部署至 Netlify 等部署提供商。網站無需預先建置，僅需原始碼。嘗試部署前，請務必先執行 read_deployment_config 工具，並確保所有缺失檔案已建立。若是部署至現有站點，使用 project_id 標識；若是新站點，請將 project_id 留空。",
  "name": "deploy_web_app",
  "parameters": {
    "properties": {
      "Framework": {
        "description": "網頁應用程式的框架。",
        "enum": ["eleventy", "angular", "astro", "create-react-app", "gatsby", "gridsome", "grunt", "hexo", "hugo", "hydrogen", "jekyll", "middleman", "mkdocs", "nextjs", "nuxtjs", "remix", "sveltekit", "svelte"],
        "type": "string"
      },
      "ProjectId": {
        "description": "部署配置文件中的專案 ID（若存在）。對於新站點或重命名站點，請保持「為空」。若是重新部署，請使用配置文件中相同的 ID。",
        "type": "string"
      },
      "ProjectPath": {
        "description": "網頁應用程式的完整絕對路徑。",
        "type": "string"
      },
      "Subdomain": {
        "description": "用於 URL 的子網域或專案名稱。若使用 project_id 部署至現有站點，請保持「為空」。對於新站點，子網域應具備唯一性且與專案相關。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 fd 在指定目錄中搜尋檔案與子目錄。
搜尋使用智慧大小寫切換，且預設會忽略被 gitignore 的檔案。
Pattern (模式) 與 Excludes (排除) 均使用 glob 格式。若搜尋副檔名 (Extensions)，無需同時指定 Pattern。
為避免輸出過多，結果上限為 50 個匹配項。使用各類參數根據需要過濾搜尋範圍。
結果將包含類型、大小、修改時間與相對路徑。",
  "name": "find_by_name",
  "parameters": {
    "properties": {
      "Excludes": {
        "description": "選填，排除符合給定 glob 模式的檔案/目錄",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "Extensions": {
        "description": "選填，包含的檔案副檔名 (不含點號 .)，匹配的路徑必須至少符合其中一項",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "FullPath": {
        "description": "選填，是否需要完整絕對路徑符合 glob 模式，預設為僅需檔名符合。開啟此旗標時請注意 glob 模式，例如開啟後 '*.py' 將無法匹配 '/foo/bar.py'，應使用 '**/*.py'。",
        "type": "boolean"
      },
      "MaxDepth": {
        "description": "選填，搜尋的最大深度",
        "type": "integer"
      },
      "Pattern": {
        "description": "選填，要搜尋的模式，支援 glob 格式",
        "type": "string"
      },
      "SearchDirectory": {
        "description": "要進行搜尋的目錄",
        "type": "string"
      },
      "Type": {
        "description": "選填，類型過濾器，enum=file,directory,any",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 ripgrep 在檔案或目錄中尋找精確的模式匹配。
結果以 JSON 格式回傳，每個匹配項包含：
- Filename (檔名)
- LineNumber (行號)
- LineContent (行內容)
總結果上限為 50 個匹配項。使用 Includes 選項按檔案類型或特定路徑過濾以精煉搜尋。",
  "name": "grep_search",
  "parameters": {
    "properties": {
      "CaseInsensitive": {
        "description": "若為 true，執行不區分大小寫的搜尋。",
        "type": "boolean"
      },
      "Includes": {
        "description": "要搜尋的檔案或目錄。支援檔案模式（如 '*.txt'）或特定路徑。若在單個檔案內搜尋請留空。",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "MatchPerLine": {
        "description": "若為 true，回傳每個符合查詢的行、行號與程式碼片段（相當於 'git grep -nI'）。若為 false，僅回傳包含查詢的檔案名稱（相當於 'git grep -l'）。",
        "type": "boolean"
      },
      "Query": {
        "description": "要在檔案中尋找的搜尋詞或模式。",
        "type": "string"
      },
      "SearchPath": {
        "description": "要搜尋的路徑。可以是目錄或檔案。此為必填參數。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "列出目錄內容。路徑必須是現有目錄的絕對路徑。對於目錄下的每個子項，產出包含：相對路徑、檔案或目錄標識、檔案大小 (bytes)、以及子項數量（若是目錄，遞迴計算）。",
  "name": "list_dir",
  "parameters": {
    "properties": {
      "DirectoryPath": {
        "description": "要列出內容的路徑，應為目錄的絕對路徑",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "讀取網頁應用程式的部署配置，並判斷應用程式是否已準備好部署。應僅在準備呼叫 deploy_web_app 工具前使用。",
  "name": "read_deployment_config",
  "parameters": {
    "properties": {
      "ProjectPath": {
        "description": "網頁應用程式的完整絕對路徑。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "從 URL 讀取內容。URL 必須是可透過網頁瀏覽器存取的有效 HTTP 或 HTTPS 連結。",
  "name": "read_url_content",
  "parameters": {
    "properties": {
      "Url": {
        "description": "要讀取內容的 URL",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用此工具編輯現有檔案。務必遵守以下規則：
1. 「不要」對同一個檔案發出多個並行的工具呼叫。
2. 要編輯同一個檔案中多個不相鄰的程式碼行，請在單次呼叫中完成。將每次編輯指定為獨立的 ReplacementChunk。
3. 對於每個 ReplacementChunk，指定 TargetContent (目標內容) 與 ReplacementContent (替換內容)。在 TargetContent 中指定要編輯的精確程式碼行，這些行「務必與現有檔案內容完全匹配」。在 ReplacementContent 中提供對應的替換內容，這必須是目標內容的完整且可直接套用的替換版本。
4. 若在單個檔案中進行多次編輯，請指定多個獨立的 ReplacementChunks。「不要」嘗試用新內容替換整個檔案，這非常昂貴。
5. 不可編輯檔案副檔名：[.ipynb]
應在其他參數前指定：[TargetFile]",
  "name": "replace_file_content",
  "parameters": {
    "properties": {
      "CodeMarkdownLanguage": {
        "description": "程式碼塊的 Markdown 語言標識，例如 'python' 或 'javascript'",
        "type": "string"
      },
      "Instruction": {
        "description": "你對檔案所做更改的描述。",
        "type": "string"
      },
      "ReplacementChunks": {
        "description": "要替換的區塊列表。對於非連續編輯，建議提供多個區塊。這必須是一個 JSON 陣列，而非字串。",
        "items": {
          "additionalProperties": false,
          "properties": {
            "AllowMultiple": {
              "description": "若為 true，檔案中所有符合 'targetContent' 的實例都會被替換。若為 false 且發現多個匹配項，將回傳錯誤。",
              "type": "boolean"
            },
            "ReplacementContent": {
              "description": "用於替換目標內容的新內容。",
              "type": "string"
            },
            "TargetContent": {
              "description": "要被替換的精確字串。這必須是檔案中精確的字元序列，包含空白。務必包含任何前導空白，否則將無法運作。若 AllowMultiple 為 false，此字串在檔案中必須是唯一的 substring，否則會報錯。",
              "type": "string"
            }
          },
          "required": ["TargetContent", "ReplacementContent", "AllowMultiple"],
          "type": "object"
        },
        "type": "array"
      },
      "TargetFile": {
        "description": "要修改的目標檔案。務必將目標檔案指定為第一個參數。",
        "type": "string"
      },
      "TargetLintErrorIds": {
        "description": "若適用，指定此編輯旨在修復的 Lint 錯誤 ID（源自最近的 IDE 回饋）。若你認為編輯能修復 Lint 錯誤，請指定 ID；若完全無關則不需提供。原則：若編輯受 Lint 回饋影響，請包含 Lint ID。請誠實判斷。",
        "items": {
          "type": "string"
        },
        "type": "array"
      }
    },
    "type": "object"
  }
}

{
  "description": "「提議」代表使用者運行的指令。作業系統：mac。Shell：bash。
**「絕不可提議 cd 指令」**。
如果你擁有此工具，表示你「確實」有能力直接在使用者系統上執行指令。
確保精確指定應在 Shell 中運行的 CommandLine。
請注意，指令執行前必須獲得使用者批准。使用者可能會拒絕不合意的指令。
指令在批准前「不會」執行。使用者可能不會立即批准。
若步驟處於「等待使用者批准 (WAITING for user approval)」狀態，則尚未開始運行。
指令將以 PAGER=cat 運行。對於通常依賴分頁且可能產出極長內容的指令（如 git log），請限制輸出長度（例如使用 git log -n <N>）。",
  "name": "run_command",
  "parameters": {
    "properties": {
      "Blocking": {
        "description": "若為 true，指令將阻塞直到完成。期間使用者無法與 Cascade 互動。僅在指令預計短時間內結束，或你必須先看到輸出才能回應使用者時才設為 true。對於啟動網頁伺服器等長運行行程，請設為 false (非阻塞)。",
        "type": "boolean"
      },
      "CommandLine": {
        "description": "要執行的精確命令列字串。",
        "type": "string"
      },
      "Cwd": {
        "description": "指令執行的當前工作目錄",
        "type": "string"
      },
      "SafeToAutoRun": {
        "description": "若你確信此指令在「無需」使用者批准的情況下運行是安全的，請設為 true。若指令具有破壞性副作用（如刪除檔案、變更狀態、安裝依賴、外部請求等），則為不安全。僅在極度確信安全時才設為 true。即使使用者要求，若你認為不安全，絕不可設為 true。嚴禁自動運行潛在不安全的指令。",
        "type": "boolean"
      },
      "WaitMsBeforeAsync": {
        "description": "僅在 Blocking 為 false 時適用。指定在將指令轉為完全非同步前等待的毫秒數。適用於應非同步運行但可能迅速報錯的指令，讓你在這段時間內捕捉錯誤。不要設得太長，以免造成等待。",
        "type": "integer"
      }
    },
    "type": "object"
  }
}

{
  "description": "執行網頁搜尋，獲取與查詢相關的網頁文件列表，支援選填的網域過濾。",
  "name": "search_web",
  "parameters": {
    "properties": {
      "domain": {
        "description": "建議搜尋優先考慮的可選網域",
        "type": "string"
      },
      "query": {
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "如果你未呼叫其他工具且正在向使用者提問，使用此工具提供少量的建議答案。例如 Yes/No 或其他簡單的多選選項。僅在你確信使用者會從建議中選擇時才審慎使用。若使用者的下一次輸入可能是包含更多細節的長篇回應，則不要提供建議。例如，假設使用者接受了你的建議，而你隨後還需要再問一個後續問題，那麼這個建議就是不好的，一開始就不該提供。盡量不要連續多次使用此工具。",
  "name": "suggested_responses",
  "parameters": {
    "properties": {
      "Suggestions": {
        "description": "建議列表。每個項目最多幾個單詞，不要超過 3 個選項。",
        "items": {
          "type": "string"
        },
        "type": "array"
      }
    },
    "type": "object"
  }
}

{
  "description": "查看程式碼項節點的內容，例如檔案中的類別或函式。必須使用完全限定的程式碼項名稱，例如 grep_search 工具回傳的名稱。例如，若要查看 `Foo` 類別中的 `bar` 函式定義，請使用 `Foo.bar` 作為 NodeName。若 codebase_search 已顯示過內容，則不要重複請求。若符號未找到，工具將回傳空字串。",
  "name": "view_code_item",
  "parameters": {
    "properties": {
      "File": {
        "description": "要編輯之節點的絕對路徑，例如 /path/to/file",
        "type": "string"
      },
      "NodePath": {
        "description": "檔案內節點的路徑，例如 package.class.FunctionName",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "查看檔案內容。行號從 0 開始。產出包含從 StartLine 到 EndLine (含) 的內容，以及範圍外內容的總結。一次最多查看 200 行。

使用此工具收集資訊時，你有責任確保獲取「完整」背景。具體而言：
1) 評估已查看內容是否足以繼續任務。
2) 若不足且懷疑關鍵內容在未顯示行中，請主動再次呼叫工具查看那些行。
3) 如有疑問，請再次呼叫。記住局部視圖可能會遺漏關鍵依賴、匯入或功能。",
  "name": "view_file",
  "parameters": {
    "properties": {
      "AbsolutePath": {
        "description": "要查看檔案的絕對路徑。",
        "type": "string"
      },
      "EndLine": {
        "description": "查看的結束行（含）。與 StartLine 的距離不可超過 200 行。",
        "type": "integer"
      },
      "IncludeSummaryOfOtherLines": {
        "description": "若為 true，除了精確行內容外，還會獲得全檔案內容的精簡總結。",
        "type": "boolean"
      },
      "StartLine": {
        "description": "查看的起始行",
        "type": "integer"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 URL 與區塊位置查看特定網頁文件的內容區塊。該 URL 必須已先透過 read_url_content 工具讀取過。",
  "name": "view_web_document_content_chunk",
  "parameters": {
    "properties": {
      "position": {
        "description": "要查看的區塊位置",
        "type": "integer"
      },
      "url": {
        "description": "該區塊所屬的 URL",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用此工具建立新檔案。若父目錄不存在，將自動為你建立。
遵循以下指引：
1. 「絕不」使用此工具修改或覆寫現有檔案。呼叫前務必確認 TargetFile 尚不存在。
2. 「務必」將 TargetFile 指定為第一個參數。在提供任何程式碼內容前先指定完整路徑。
應在其他參數前指定：[TargetFile]",
  "name": "write_to_file",
  "parameters": {
    "properties": {
      "CodeContent": {
        "description": "要寫入檔案的程式碼內容。",
        "type": "string"
      },
      "EmptyFile": {
        "description": "設為 true 以建立空檔案。",
        "type": "boolean"
      },
      "TargetFile": {
        "description": "要建立並寫入程式碼的目標檔案。",
        "type": "string"
      }
    },
    "type": "object"
  }
}
{/functions}
