在每一轮 Python/uv 开发开始前，先执行一次简短的环境 checklist，申请所有可能需要用到的权限,然后再进入实现工作；不需要在每次单独命令前重复检查。

环境检查只围绕项目的 `.venv`、`.venv\Scripts\python.exe` 和 `uv run` 展开，不检查系统 Python。优先先检查关键路径是否存在、是否可访问，以及项目环境是否明显依赖工作区外的 uv Python 或 uv 缓存目录。

如果判断本轮开发大概率会使用 `.venv` Python、`uv run`、测试命令或激活脚本，应在开局阶段主动申请所需的权限提升，避免后续开发过程中反复被打断。权限申请应优先覆盖高频命令，例如：
- `.\\.venv\\Scripts\\python.exe`
- `uv run python`
- `uv run`
- `powershell -ExecutionPolicy Bypass -Command "& .\\.venv\\Scripts\\Activate.ps1; python"`

把环境问题视为“权限与执行链路问题”优先排查，而不是默认修改缓存位置来规避。除非用户明确要求，不要把修改 `UV_CACHE_DIR`、`UV_PYTHON_INSTALL_DIR` 等路径当作默认解决方案。遇到环境问题时，优先按下面顺序排查并处理：
1. `.venv` 是否存在，`.venv\Scripts\python.exe` 是否可执行
2. `uv run python -V` 是否可运行
3. 是否依赖工作区外的 uv Python 目录或 uv 缓存目录
4. 是否因为 PowerShell 执行策略导致激活脚本失败
5. 一旦确认属于权限/沙箱问题，立即通过正式的权限提升流程解决

可选验证只在项目确实具备对应文件或工具时执行，不默认假设存在 `main.py` 或 `pytest`。每轮开发开始时，先用最小验证命令确认环境链路可用，再开始正式开发。

完成检查后，先给出简短结论：
- `.venv` Python 是否可用
- `uv run` 是否可用
- 是否依赖工作区外路径
- 本轮开发已申请或需要申请哪些权限
- 后续本轮开发统一使用哪条执行路径

然后第第二步,验证可能会用到的命令,并在项目开始之前,一次性,持久申请权限,避免后续项目中弹出权限申请的问题.
可能的测试命令包括但不限于:
    uv run python
    uv run python your_script.py
    uv run pytest
    uv run python -m pytest -q
    python your_script.py
    pytest


