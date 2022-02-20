# org.thanos.base

####How to Prohibit File Submissions Containing Sensitive Words (如何禁止包含敏感词汇的文件提交)
1. 修改本地代码库目录 org.thanos.base\.git\hooks\文件夹中的 pre-commit.sample文件名为pre-commit
2. 复制如下脚本内容到上述重命名的pre-commit文件中，并进行保存

```
#!/bin/sh # 
# An example hook script to verify what is about to be committed.
# Called by "git commit" with no arguments. The hook should
# exit with non-zero status after issuing an appropriate message if
# it wants to stop the commit. # 
# To enable this hook, rename this file to "pre-commit".
username=$(git config --get user.name)
echo "author $username"
filenamelist=$(git diff --cached --name-only)
# echo "文件列表 $filenamelist"
#echo $(cat .gitForbiddenKeywords)
result=$(grep -iE $(cat .gitForbiddenKeywords) $filenamelist)
if [ "$result" != "0" ]
then
echo  
"Error: Attempt to add files which contain forbidden Keywords.

Please check these files content:  
$result"
exit 1
fi
$result > dev/null
```
3. 在.gitForbiddenKeywords文件中以|分隔添加需要过滤禁止提交的敏感词，如 HW|ZTE|TENGXUN
4. 若需强制提交某次包含敏感词的文件，需用命令行 ```git commit --no-verify m"comment-msg"``` 进行提交