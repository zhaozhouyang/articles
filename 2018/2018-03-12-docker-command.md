# Docker Command
## run
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
[OPTIONS] 常用：-it (就是-i[以交互模式运行容器] -t[为容器重新分配一个伪输入终端]), --rm (运行完后自动删除容器)
[COMMAND] 常用： /bin/bash，这样运行后直接进入bash
最后就变成了
```
docker run -it --rm IMAGE /bin/bash
```
如果要后台运行：-d，这时
## exec
```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```
对于已经运行起来的container，可以通过exec让其执行命令，比如
```
docker exec -it 63ca68061f64 /bin/bash
```
可以进入bash

https://stackoverflow.com/questions/26401649/building-a-new-docker-image-with-the-same-name-as-an-existing-ones



docker system df
