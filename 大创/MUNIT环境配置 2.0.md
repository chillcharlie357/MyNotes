1. 新建conda环境
`conda create -n <name>`
2. 下载python包，建议3.9版本，3.7-3.9应该都适配
   
3. 根据pytorch官网给的建议下载pytorch torchvision torchaudio pytorch-cuda等相关文件
例如，conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
在下载时，不要挂载梯子

4. 根据提示，修改适配性bug
由于pytorch进行过更新和删改，有些包不能用
需要下载
需要删除**某个包**导入torchfile，需要将其中**某个**load 改成safe_load

- 需要下载yaml
`conda install pyyaml`

- 某个load:约105行
应该就是pyyaml里的
`return yaml.safe_load(stream)`
~~conda install torchfile~~ 

pip install torchfile

- 某个包
```py
# from torch.utils.serialization import load_lua  
from torchfile import load as load_lua
```

- 找不到没有`inception_v3`
`from torchvision.models import inception_v3`




- 遇到OSError的时候需要下载cudatoolkit
`conda install cudatoolkit=11.7`


5.以上，应该是能跑了