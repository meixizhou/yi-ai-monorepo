# 发包工具 
## rush
https://rushjs.io/zh-cn/pages/intro/get_started/

## Turborepo
https://turborepo.com/

## learn


# pnpm 中 `i` 和 `add` 命令的区别

在 pnpm 里，`i` 和 `add` 虽然都能安装依赖，但用途和场景有区别：

## 1. `pnpm i`（`pnpm install`）
- **主要功能**：安装 `package.json` 中已有的依赖。
- **使用场景**：
  - 项目第一次下载下来（比如从 Git clone），你要把 `package.json` 里的依赖全部安装好。
  - 或者你删掉了 `node_modules`，需要重新安装。
- **特点**：
  - 不会改动 `package.json` 和 `pnpm-lock.yaml`（除非有 `peerDependencies` 或 lock 不一致的情况）。
  - 只会根据现有依赖清单安装。

## 2. `pnpm add <package>`
- **主要功能**：安装新依赖，并写入 `package.json`。
- **使用场景**：
  - 想引入一个新的库，比如 `pnpm add axios`。
  - 可以指定依赖类型：
    - `pnpm add axios -D` → `devDependencies`
    - `pnpm add axios -g` → 全局安装
- **特点**：
  - 会更新 `package.json` 和 `pnpm-lock.yaml`。
  - 会安装最新版本（或你指定的版本）。

## ✅ 一句话总结：
- `pnpm i` → 装现有依赖（不会改 `package.json`）。
- `pnpm add` → 装新依赖并写进 `package.json`。


1. pnpm add lerna     

![alt text](image.png)
 
这个错误提示是 pnpm 在 Monorepo 工作区中给出的保护机制，防止你不小心将依赖安装到工作区根目录。
解决方法很简单，按照提示添加 -w 或 --workspace-root 标志，明确指定要安装到根目录：
bash
pnpm add lerna -D -w
解释：
-D 表示将 lerna 安装为开发依赖（推荐，因为 lerna 是构建工具）
-w 明确告诉 pnpm 要安装到工作区根目录
Lerna 通常是作为 Monorepo 项目的根目录工具使用的，所以这个安装方式是正确的。安装完成后，你就可以在根目录的 package.json 中看到 lerna 的依赖记录了。

pnpm add lerna --filter @yideng/cli

2. npx lerna init
3. pnpm add @yideng/libs --filter @yideng/hooks --workspace
4. pnpm --filter @yideng/hooks run dev
5. 配置lerna.json
```js
{
  "$schema": "node_modules/lerna/schemas/lerna-schema.json",
  "npmClient": "pnpm",
  "useWorkspaces": true,
  "version": "independent",
  "packages": ["packages/*", "apps/*"],
  "ignoreChanges": ["**/node_modules/**", "**/__snapshots__/**"],
  "command": {
    "publish": {
      "conventionalCommits": true,
      "message": "[skip ci] chore: release"
    }
  }
}
```