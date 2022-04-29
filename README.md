# 学习changesets

1. 初始化一个项目`pnpm init -y`
2. 创建pnpm-workspace.yaml文件
3. 安装changesets`pnpm add -DW @changesets/cli`
   1. -W 参数表示安装到项目根目录
4. 初始化changeset`pnpm changeset init`
5. 新建packages目录
6. 在packages目录下创建一个新的package，例如`package-a`
7. 新建`.github`文件夹，存放github的配置
8. 在`.github`文件夹下新建`workflows`文件夹
9. 在`.github/workflows`文件夹下新建`ci.yml`文件
10. `ci.yml`的内容是执行项目的测试和构建
11. 添加一个`docs`package,然后添加`deploy.yml`
12. `deploy.yml`的作用是执行打包和部署docs包
13. 问题记录 `remote: Permission to hcg1023/learn-changesets.git denied to github-actions[bot].fatal: unable to access 'https://github.com/hcg1023/learn-changesets/': The requested URL returned error: 403`
    1. 出现问题的原因是github checkout 把用户信息缓存下来了，导致 changesets 的action使用了上边的用户信息，就提示无权限了
    2. 解决方案，在checkout的时候添加`persist-credentials: false`
       ```yml
       - name: Checkout Repo
         uses: actions/checkout@v3
         with:
           fetch-depth: 0
           persist-credentials: false
       ```
