```shell
#!/usr/bin/bash
path=/home/mangolau/
project_path=/home/mangolau/project_name/

git=giit@8.129.108.170:project_group/project_name.git
branch=V20210304

#同步文件git的过滤条件
#author：git提交作者
#max_count: 该作者最近几条commit
author=$1
max_count=5

#目标目录
target_path=/var/www/project/

#下载项目到本地服务器
#切换到指定目录
rm -rf $project_path
cd $path
git clone $git
cd $project_path
git fetch origin $branch
git checkout $branch

echo "sync stat"
git log --author=$author --oneline --max-count=$max_count | awk '{print $1}' | xargs git show --name-only --pretty=format:'' | sed "/^$/d" | uniq | xargs -t -i cp {} $target_path{}
echo "sync end"

#执行同步完成后的脚本

rm -rf $project_path

echo "sync success"

exit 0
```