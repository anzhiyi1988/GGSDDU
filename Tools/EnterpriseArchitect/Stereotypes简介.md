| Stereotype  |            | Applies To  | Notes                                                        |
| ----------- | ---------- | ----------- | ------------------------------------------------------------ |
| access      | 访问       | dependency  | 源可以访问目标的公共内容，Public contents of target are accessible to namespace of source |
| bind        | 绑定、捆绑 | dependency  | 源根据给定的参数，实例化目标模板，Source instantiates target template using given parameters |
| call        | 调用       | dependency  | 源调用目标，Source invokes the target                        |
| derive      | 得到、提取 | dependency  | 源可以通过目标计算，Source may be computed from target       |
| friend      |            | dependency  | 源可以看见目标的特殊可见性内容，Source is given special visibility of target |
| import      | 导入       | dependency  | 把目标的公共内容导入到源的空间中，Public contents of target are imported into source namespace |
| instanceOf  | 实例化     | dependency  | 源是目标的一个实例，The source object is an instance of the target |
| instantiate |            | dependency  | 通过操作源，创建目标的示例。Operations on the source class create instances of the target class |
| powertype   |            | dependency  | A classifier whose objects are all children of a given parent |
| refine      |            | dependency  | Source is at a finer degree of abstraction than source       |
| send        |            | dependency  | 源向目标发送一个时间。The source sends the target an event   |
| trace       |            | dependency  | 目标是源的祖先。The target is an historical ancestor of source |
| use         |            | dependency  | 源的语义依赖于目标的公共部分。The semantics of the source depend on the public part of the target |
| import      |            | association | 引入，其实就是类设计中的关联，源引入目标做成员变量。import   |
| metaclass   |            | classifier  | A classifier whose objects are all classes                   |
| subscribe   | 订阅       | association | 当目标中发生事件，会通知源类。Source class will be notified when an event occurs in target |
