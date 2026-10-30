# Install ZA Link

## English

1. Download the ZIP matching your OS and CPU from [Releases](https://github.com/vtd182/za-link-downloads/releases). macOS Apple Silicon uses `darwin-arm64`; Intel uses `darwin-amd64`. Verify the archive against `SHA256SUMS.txt`.
2. Extract it into a permanent directory. Install Node.js 18+ if needed, then run `node setup.mjs` inside the extracted `za-link` folder. The helper prints configuration; it does not edit client settings.
3. Register the printed command and arguments as a stdio MCP server named `za-link` in your AI client. Use your client's configuration format (for example TOML in Codex). Refresh the MCP connection.
4. In Figma Desktop, choose Plugins → Development → Import plugin from manifest and select `figma-plugin/manifest.json`. Open your design and run ZA Link. Keep the plugin open. Default endpoint: `127.0.0.1:1802`; no pairing code.
5. Ask your assistant to call `get_runtime_health` and `get_runtime_sessions`, identify the file, then use `set_runtime_session` to select its current session. Read a selected frame and screenshot before editing.

After restarting the Figma plugin, discover and select its new session. A pinned disconnected session fails instead of silently choosing a different file. `activeSession` in MCP diagnostics is your client's selected target; compare it with the live session list.

For updates, extract a new kit, change the client path, restart the matching plugin and verify the contract. Keep the previous kit for rollback. Linux packages contain the runtime; this guide uses Figma Desktop on macOS or Windows and does not promise a Linux Figma client.

Skills are separate. No internal skills, hooks or agent account credentials are included. The bundled README covers runtime details. Check your organization's development-plugin permissions and normal OS software approval flow if installation is restricted.

## Tiếng Việt

1. Tải ZIP đúng hệ điều hành và CPU tại [Releases](https://github.com/vtd182/za-link-downloads/releases), kiểm tra SHA256 rồi giải nén vào thư mục cố định.
2. Với Node.js 18+, chạy `node setup.mjs` trong thư mục `za-link`. Chép command/args được in ra vào cấu hình MCP của trợ lý theo đúng định dạng client.
3. Trong Figma Desktop, import `figma-plugin/manifest.json` qua Plugins → Development. Mở file cần dùng, chạy ZA Link và giữ plugin mở.
4. Nhờ agent kiểm tra health, liệt kê file và chọn đúng session trước khi đọc/sửa. Nếu khởi động lại plugin, chọn session mới.

Bộ cài gồm runtime/plugin; skill nội bộ nhận qua kênh riêng của nhóm. Khi nâng cấp, dùng runtime và plugin cùng phiên bản, giữ bộ cài cũ để quay lại nếu cần.
