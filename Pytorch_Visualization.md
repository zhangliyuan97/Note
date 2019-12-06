# Pytorch可视化步骤

## 安装tensorboardX

默认已经安装好了pytorch

- 进入虚拟环境

```
conda activate torch 
```

- 安装tensorflow，解决依赖问题

```
conda install tensorflow
```

注意：我安装的顺序是先安装了tensorflow，再安装了pytorch，这时候会提醒给tensorflow降级。所以我不清楚如果顺序颠倒会不会提醒降级，应该是从2.0.0降级到了1.14.0

- 安装tensorboardX

```
pip install tensorboardX
```

## 测试

```python
import torch
import torch.nn as nn
from tensorboardX import SummaryWriter

class Test_model(nn.Module):
    def __init__(self):
        super(Test_model, self).__init__()
        self.layer = nn.Sequential(
            nn.Linear(3, 256),
            nn.ReLU(),
            nn.Linear(256, 256),
            nn.ReLU(),
            nn.Linear(256, 10)
        )
    def forward(self, x):
        return self.layer(x)

model = Test_model()

writer = SummaryWriter('logs')
writer.add_graph(model, input_to_model=torch.randn((3,3)))
writer.add_scalar(tag="test", scalar_value=torch.tensor(1)
                    , global_step=1)
writer.close()
```

- 新建test.py，copy上面的code。（值得说明的是，SummaryWriter括号里的logs，是自定义的文件名称，会自动生成一个文件。）

- 运行test.py

- 在终端中，输入

  ```
   tensorboard --logdir=logs
  ```

  此时会生成一个端口为6006的域名，类似于jupyter notebook的方式，打开它，就可以看到最终结果。



