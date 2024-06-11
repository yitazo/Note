## 1. 整合slf4j

### 1.1 导入slf4j

boot默认就带有slf4j，不需要导入额外坐标

### 1.2 使用slf4j

#### 1.2.1 手动创建日志对象

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private final Logger log = LoggerFactory.getLogger(fileName.class);
```

#### 1.2.2 使用lombok创建日志对象

在类上添加@Slf4j注解即可在方法中使用log.debug()等各种方法进行各种等级日志的输出