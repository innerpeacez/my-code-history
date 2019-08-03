#### 由来

作为一个 Java 程序员，最近一直接触 kubernetes, 每次都是用的一键部署脚本搭的 k8s 集群，也一直用的挺欢，直到今天我手贱把全局 crd 文件给删了，我才知道有必要 一步一步搭一个的重要性了（我不知道我删了啥），但是看了教程之后，发现要操作好多机器，真的麻烦，对运维这个职业由衷的尊敬，千言万语化成两个字：牛逼。

然后我就想在一台机器上一把执行所有机器，然后这个简单的分发脚本到远程节点并执行的 shell 脚本就诞生了

```shell
#! /bin/bash
# 远程节点信息
nodes=`cat node.txt`
# 需要远程执行的脚本文件
script=$1
# 远程节点存放脚本的路径
script_path=$2

scp_script() {
  local _node=$1
  ssh $_node  "mkdir -p ${script_path}"
  scp ./$script $_node:${script_path}/${script}
}

run_script() {
  local _node=$1
  ssh $_node "/bin/bash ${script_path}/${script}"
}

remove_remote_script() {
  local _node=$1
  ssh $_node "sudo rm -rf ${script_path}/"
}

log() {
  echo `date "+%Y-%m-%d %H:%M:%S"` ':' $1
}

start_job() {
  if [ -z "$nodes" ] 
  then
    log "node is empty"
    exit 1
  fi

  if [ -z "$script" ]
  then 
    log "script is empty"
    exit 1
  fi

  if [ -z "$script_path" ]
  then
    log "script_path is empty"
    exit 1
  fi

  local _nodes=$nodes
  for node in $_nodes;
  do
    scp_script $node
    run_script $node
    remove_remote_script $node
    echo "$node success"
  done
}

start_job
```

注意：执行这个脚本最好先配置好免密登录，不然有点麻烦，如果你只想分发文件并不想执行文件，只要注释掉 start_job() 函数中的以下两行

- run_script $node
- remove_remote_script $node

当然写这种东西还是有点刺激的，写完之后，想要和旁边的运维小伙伴吹吹牛皮，然后运维小伙伴和我说有个叫 ansible 的东西，不用自己写，what fuck ? 为什么不早说，记录一下，有点简陋的脚本

