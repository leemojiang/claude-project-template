## Python / uv Environment Checklist

在开始任何 Python 相关开发、调试、测试或执行任务前，必须先完成一次“项目环境检查与权限准备”。在环境未确认可用前，不要直接进入功能开发。

### 目标

1. 所有检查都围绕项目的 `.venv`、项目使用的 Python 解释器、以及 `uv run` 路径展开。
2. 优先检查关键路径和权限，再执行验证命令，避免在开发过程中反复被环境问题打断。
3. 如果发现需要访问工作区外的 Python、uv 缓存或 uv 管理目录，应在项目开始阶段主动申请权限提升，而不是等到中途报错后再处理。
4. 后续整个项目都应沿用已经验证通过的环境路径和权限策略。

### 第一步：检查关键路径

优先检查以下路径是否存在、是否可访问、是否可执行：

- 项目虚拟环境目录：
  - `.venv`
- 项目虚拟环境解释器：
  - `.venv\Scripts\python.exe`
- 项目虚拟环境中的测试命令（如果存在）：
  - `.venv\Scripts\pytest.exe`
- uv 可能依赖的外部缓存目录，例如：
  - `C:\Users\Administrator\AppData\Local\uv\cache`
- uv 管理的外部 Python 目录，例如：
  - `C:\Users\Administrator\AppData\Roaming\uv\python\...`

如果你判断以下任一情况成立，应立即准备权限提升，不要继续在沙箱内反复尝试：

- `.venv\Scripts\python.exe` 存在但无法执行
- `uv run` 需要访问工作区外缓存目录
- `uv run` 需要访问 uv 管理的外部 Python 解释器
- 项目的 `.venv` 明显依赖工作区外解释器路径
- PowerShell 激活脚本或解释器调用被权限/策略拦截

### 第二步：优先申请权限提升

如果关键路径检查显示本项目依赖工作区外目录，必须在项目开始时主动提出权限提升请求，并明确说明目的：

- 为了验证 `.venv` 中的 Python 是否可执行
- 为了验证 `uv run` 是否能正常工作
- 为了避免后续开发中频繁被环境问题打断

优先为后续高频命令申请权限，而不是只解决单次报错。

推荐优先申请权限的命令包括：

- `.\\.venv\\Scripts\\python.exe`
- `uv run python`
- `uv run`
- `powershell -ExecutionPolicy Bypass -Command "& .\\.venv\\Scripts\\Activate.ps1; python"`

如果项目后续会大量使用测试命令，并且确认项目中存在 pytest，再考虑追加：

- `.\\.venv\\Scripts\\pytest.exe -q`
- `uv run pytest -q`
- `powershell -ExecutionPolicy Bypass -Command "& .\\.venv\\Scripts\\Activate.ps1; pytest -q"`

### 第三步：执行最小验证

在权限准备完成后，执行最小环境验证。不要假设项目一定有 `main.py`，也不要假设一定安装了 `pytest`。

必做验证：

1. 验证 `.venv` 中的解释器是否可执行
   - 示例命令：
     - `.\\.venv\\Scripts\\python.exe -V`
   - 如需验证激活后的行为，可选使用：
     - `powershell -ExecutionPolicy Bypass -Command "& .\\.venv\\Scripts\\Activate.ps1; python -V"`

2. 验证 `uv run` 是否能调起项目环境中的 Python
   - 示例命令：
     - `uv run python -V`

可选验证：
仅当项目中存在对应文件或工具时再执行，不要默认假设存在。

- 如果项目存在入口脚本，例如 `main.py`、`app.py` 或用户指定的脚本：
  - `.\\.venv\\Scripts\\python.exe .\\<script>`
  - `uv run python .\\<script>`

- 如果项目确认安装了 pytest，或项目目录中存在测试约定并允许测试：
  - `.\\.venv\\Scripts\\pytest.exe -q`
  - `uv run pytest -q`

### 判断规则

1. 如果 `.venv\Scripts\python.exe -V` 失败，而路径存在：
   - 优先判断为解释器执行权限问题、外部解释器依赖问题，或沙箱限制问题
   - 不要直接判断为项目代码问题

2. 如果 `uv run python -V` 失败：
   - 优先检查 uv 缓存目录和 uv 管理的 Python 目录是否需要额外权限
   - 明确区分是 `uv` 的缓存/解释器访问问题，还是项目本身的问题

3. 如果激活脚本失败，但直接调用 `.venv\Scripts\python.exe` 成功：
   - 说明这是 shell 执行策略问题，不等于虚拟环境损坏

4. 如果 `.venv` 明显绑定到了工作区外解释器：
   - 明确告知该项目的 venv 依赖外部解释器路径
   - 如需长期稳定开发，建议后续评估是否重建为更本地化的环境布局

### 汇报格式

完成环境检查后，必须先给出简短结论，再进入正式开发。结论至少应包含：

- `.venv` 的 Python 是否可执行
- `uv run` 是否可用
- 哪些关键路径依赖工作区外目录
- 哪些命令需要权限提升
- 后续开发应沿用哪种执行路径

示例结论格式：

- `.venv` Python：可用 / 不可用
- `uv run`：可用 / 不可用
- 外部依赖路径：`<path list>`
- 已申请或需要申请的权限：`<command list>`
- 推荐后续统一使用：`直接 .venv Python` / `uv run` / `两者都可`

### 工作原则

1. 环境问题优先解决，不要拖到功能开发中途再处理。
2. 只要判断后续开发大概率会重复触发同类权限问题，就应在项目开始阶段主动申请对应权限。
3. 验证应尽量使用最小命令，先验证解释器和执行链路，再验证具体业务脚本或测试。
4. 不要默认项目一定存在 `main.py`、`pytest` 或完整测试集；这些只能作为可选验证项。
