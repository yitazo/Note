BufferGeometry不是继承自Object3的



顶点位置：他没有.position位置属性，由顶点数据的位置属性构成BufferGeometry，通过.attribute.position赋值Float32Array数组即可，数组里每三个值构成一个坐标值，如果是被网格模型之类引用，则每三个值构成三角形，三角形连接在一起形成网格模型

索引：当顶点数据存在大量重复点时，可以使用索引减少点数据量编写，通过点



