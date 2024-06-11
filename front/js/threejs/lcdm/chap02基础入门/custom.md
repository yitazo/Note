## 性能监视器

```
import Stats from "three/examples/jsm/libs/stats.module";
let stats = new Stats();
document.querySelector("#canvas").appendChild(stats.domElement);
```

监视器插入元素完成后需要在请求动画帧里面调用stats.update();

## 元素插入问题

在vue中不能将canvas画布以及stats性能监视器添加到body或者app这个div里，否则开发环境下更改数据会导致刷新后再添加一幅canvas或一个性能监视器进去

