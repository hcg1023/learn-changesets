# 学习changesets

1. 初始化一个项目`pnpm init -y`
2. 创建pnpm-workspace.yaml文件
3. 安装changesets`pnpm add -DW @changesets/cli`
   1. -W 参数表示安装到项目根目录
4. 初始化changeset`pnpm changeset init`
5. 新建packages目录
6. 在packages目录下创建一个新的package，例如`package-a`
