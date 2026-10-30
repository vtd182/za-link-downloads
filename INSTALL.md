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

### Claude Code

Install Claude Code and Node.js first. In the extracted `za-link` folder, register the local server for your user:

macOS / Linux (zsh or bash):

```sh
claude mcp add --transport stdio --scope user za-link -- "$(node -p 'process.execPath')" "$(pwd)/runtime/bin/run.js"
```

Windows (PowerShell):

```powershell
claude mcp add --transport stdio --scope user za-link -- (node -p "process.execPath") "$((Get-Location).Path)\runtime\bin\run.js"
```

This writes an MCP registration, not a skill installation. If `za-link` already exists, inspect it with `claude mcp get za-link` and update the intended entry rather than adding a duplicate. Restart Claude Code and check `/mcp`. Keep the extracted directory in place. See [Claude Code's official MCP guide](https://code.claude.com/docs/en/mcp).

### Claude Desktop

Run `node setup.mjs`. Open Claude Desktop's **Settings → Developer → Edit Config**. Merge the printed `mcpServers.za-link` entry into the existing JSON, preserving other servers. Use the absolute `command` and `args` printed by setup. Fully quit and reopen Claude Desktop, then check that ZA Link's tools are available. See the [official local MCP guide](https://modelcontextprotocol.io/docs/develop/connect-local-servers).

The kit is a local stdio server. Do not paste `127.0.0.1:1802` into a remote connector URL on claude.ai. That port is the plugin bridge, not a remote MCP endpoint. This ZIP is not a one-click `.mcpb` desktop extension.

### Verify the connection

Import and run the bundled Figma plugin (step 4), then ask Claude:

> Check ZA Link health and connected Figma sessions. Tell me the file name, select that session and read my current selection. Do not change the design.

`PLUGIN_NOT_CONNECTED` or zero sessions means the runtime is reachable but the Figma plugin is not connected yet. A successful download or setup command alone does not prove that Claude can read the design. General Figma tools work without internal skills; ZDS/Zinstant implementation workflows require the corresponding internal skill packs and project libraries.

Public-kit validation: the downloaded macOS ARM64 kit was checked for archive/file hashes, setup JSON and a real stdio handshake/tool listing. This is not a fresh-account Claude UI acceptance test. Windows and Intel Mac GUI acceptance remain unverified.

## Tiếng Việt

1. Tải ZIP đúng hệ điều hành và CPU tại [Releases](https://github.com/vtd182/za-link-downloads/releases), kiểm tra SHA256 rồi giải nén vào thư mục cố định.
2. Với Node.js 18+, chạy `node setup.mjs` trong thư mục `za-link`. Chép command/args được in ra vào cấu hình MCP của trợ lý theo đúng định dạng client.
3. Trong Figma Desktop, import `figma-plugin/manifest.json` qua Plugins → Development. Mở file cần dùng, chạy ZA Link và giữ plugin mở.
4. Nhờ agent kiểm tra health, liệt kê file và chọn đúng session trước khi đọc/sửa. Nếu khởi động lại plugin, chọn session mới.

Bộ cài gồm runtime/plugin; skill nội bộ nhận qua kênh riêng của nhóm. Khi nâng cấp, dùng runtime và plugin cùng phiên bản, giữ bộ cài cũ để quay lại nếu cần.

### Dùng với Claude

- **Claude Code:** cần cài CLI và Node.js. Mở terminal trong thư mục `za-link`, chạy lệnh đúng hệ điều hành ở mục Claude Code bên trên. Lệnh đăng ký server cho tài khoản máy; mở lại Claude Code và kiểm tra `/mcp`. Nếu đã có `za-link`, kiểm tra cấu hình cũ trước khi cập nhật.
- **Claude Desktop:** chạy `node setup.mjs`, vào **Settings → Developer → Edit Config**, gộp mục `mcpServers.za-link` vào JSON và giữ nguyên server khác. Thoát hẳn rồi mở lại ứng dụng.
- **Claude trên web:** không dán `127.0.0.1:1802` làm URL connector. Đây là cổng bridge của plugin; bộ cài dùng MCP stdio local, chưa phải extension `.mcpb` cài một lần bấm.

Sau khi import và chạy plugin trong Figma, gửi Claude:

> Kiểm tra health và các session Figma của ZA Link. Báo tên file, chọn session đó rồi đọc selection hiện tại. Chưa chỉnh sửa thiết kế.

Chỉ khi Claude đọc được selection mới xem là kết nối hoàn tất. Không có session hoặc `PLUGIN_NOT_CONNECTED` nghĩa là cần kết nối plugin. Các tool Figma cơ bản không cần skill nội bộ; workflow ZDS/Zinstant vẫn cần skill và thư viện project tương ứng.

Đã kiểm tra ZIP macOS ARM64 tải từ public, checksum, JSON setup và bắt tay MCP stdio/liệt kê tool. Chưa nghiệm thu bằng tài khoản Claude mới hoặc giao diện Windows/Intel Mac.
