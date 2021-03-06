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
11. 问题记录 `remote: Permission to hcg1023/learn-changesets.git denied to github-actions[bot].fatal: unable to access 'https://github.com/hcg1023/learn-changesets/': The requested URL returned error: 403`
    1. 解决方案：github仓库的settings中，给workflow的权限不够，需要在`Settings/Actions/General/Workflow permissions`中选择`Read and write permissions`和`Allow GitHub Actions to create and approve pull requests`

# 关于 publish
`publish.yml`的作用是执行打包和部署包
**changesets/action会自动判断我们是否需要执行publish，所以不需要担心任何问题**
1. 建议publish在ci之后执行
   1. ```yaml
      on:
        workflow_run: # 在main分支的ci这个工作流，执行完成以后
           workflows: [ci]
           branches: [main]
           types: [completed]
      ```
      
2. 在Create Release Pull Request or Publish to npm这一步，changeset会自动判断当前是开发的pull request合并，或者push，还是`changeset-release/main`这个分支的合并，**如果是其它分支的合并，则不会执行publish，** 这对发布来说很重要，因为我们需要自己控制发布的时机
3. 合并一个开发分支进入main分支以后，changesets会创建或更新它自己的`changeset-release/main`这个分支，并创建或更新它的pull request，这个时候，`changesets/actions`只会执行version这个命令，不会执行publish；我们只要在需要发布的时候，把这个pull request合并进来就可以了
4. 合并到main以后，changesets还会执行publish这个action，但是这一次它会执行publish，而不是version。
5. 在publish的时候，由于我们是使用`pnpm -r publish`，所以需要使用`changeset tag`进行tag和release的发布
6. 我们还可以在publish.yml中，Post Publish to npm的时候，执行一些额外的操作
7. 也可以在这里去执行deploy，比如部署到github pages

# 关于prerelease
1. 要发布一个beta或者next版本的话，需要先执行`changeset pre enter {tag}`
2. 这样就进入了一个beta版本，在这之后的更改，都正常使用`changeset`来进行生成
3. 需要发布的时候，通过`changeset version`更新版本并发布即可
4. 退出prerelease模式，需要执行`changeset pre exit`
5. 执行以后会把当前的prerelease版本变成正式版本
6. 执行发布即可
## 在本项目内的prerelease要怎么做
1. 新建一个分支，开启`changeset pre enter {tag}`模式
2. 开发内容
3. 提交Pull Request到main分支
4. 合并PR
5. main分支进入prerelease模式
6. 合并changeset bot PR
7. 会自动发布对应的prerelease版本
8. 需要发布正式版本的时候，使用分支或者创建一个新分支
9. 执行`changeset pre exit`退出prerelease模式
10. 推送代码，提交PR
11. 合并，自动发布正式版本

## 更改tag，例如从beta到rc
1. 先执行`changeset pre exit`退出prerelease模式
2. 然后在紧接着执行`changeset pre enter next`进入rc的prerelease模式
3. 推送代码

# 关于`changeset-release/main`这个分支
它是changesets自动创建的，不需要我们手动创建

