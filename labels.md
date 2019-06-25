标签其实就一对 key/value ，被关联到对象上，比如Pod,标签的使用我们倾向于能够标示对象的特殊特点，并且对用户而言是有意义的（就是一眼就看出了这个Pod是尼玛数据库），但是标签对内核系统是没有直接意义的。标签可以用来划分特定组的对象（比如，所有女的），标签可以在创建一个对象的时候直接给与，也可以在后期随时修改，每一个对象可以拥有多个标签，但是，key值必须是唯一的



"labels": {

 "key1" : "value1",

 "key2" : "value2"

 }

我们最终会索引并且反向索引（reverse-index）labels，以获得更高效的查询和监视，把他们用到UI或者CLI中用来排序或者分组等等。我们不想用那些不具有指认效果的label来污染label，特别是那些体积较大和结构型的的数据。不具有指认效果的信息应该使用annotation来记录。



Motivation

Label可以让用户将他们自己的有组织目的的结构以一种松耦合的方式应用到系统的对象上，且不需要客户端存放这些对应关系（mappings）。



服务部署和批处理管道通常是多维的实体（例如多个分区或者部署，多个发布轨道，多层，每层多微服务）。管理通常需要跨越式的切割操作，这会打破有严格层级展示关系的封装，特别对那些是由基础设施而非用户决定的很死板的层级关系。



Label例子



“release” : “stable”, “release” : “canary”, …

 “environment” : “dev”, “environment” : “qa”, “environment” : “production”

 “tier” : “frontend”, “tier” : “backend”, “tier” : “middleware”

 “partition” : “customerA”, “partition” : “customerB”, …

 “track” : “daily”, “track” : “weekly”

Label的语法和字符集

Label其实是一对 key/value，有效的Label keys必须是部分：一个可选前缀+名称，通过/来区分，名称部分是必须的，并且最多63个字符，开始和结束的字符必须是字母或者数字，中间是字母数字和”\_”，”-“，”.”，前缀是刻有可无的，如果指定了，那么前缀必须是一个DNS子域，一系列的DNSlabel通过”.”来划分，长度不超过253个字符，“/”来结尾。如果前缀被省略了，这个Label的key被假定为对用户私有的，自动系统组成部分（比如kube-scheduler, kube-controller-manager, kube-apiserver, kubectl）,这些为最终用户添加标签的必须要指定一个前缀，Kuberentes.io 前缀是为Kubernetes 内核部分保留的。



合法的label值必须是63个或者更短的字符。要么是空，要么首位字符必须为字母数字字符，中间必须是横线，下划线，点或者数字字母。



Label选择器

与name和UID不同，label不提供唯一性。通常，我们会看到很多对象有着一样的label。



通过label选择器，客户端/用户能方便辨识出一组对象。label选择器是kubernetes中核心的组织原语。



API目前支持两种选择器：基于相等的和基于集合的。一个label选择器一可以由多个必须条件组成，由逗号分隔。在多个必须条件指定的情况下，所有的条件都必须满足，因而逗号起着AND逻辑运算符的作用。



一个空的label选择器（即有0个必须条件的选择器）会选择集合中的每一个对象。



一个null型label选择器（仅对于可选的选择器字段才可能）不会返回任何对象。



Equality-based requirement

基于相等性或者不相等性的条件允许用label的键或者值进行过滤。匹配的对象必须满足所有指定的label约束，尽管他们可能也有额外的label。有三种运算符是允许的，“=”，“==”和“!=”。前两种代表相等性（他们是同义运算符），后一种代表非相等性。例如：



environment = production

 tier != frontend

第一个选择所有键等于 environment 值为 production 的资源。后一种选择所有键为 tier 值不等于 frontend 的资源，和那些没有键为 tier 的label的资源。



要过滤所有处于 production 但不是 frontend 的资源，可以使用逗号操作符， environment=production,tier!=frontend 。



environment=production,tier!=frontend

基于set的条件

基于集合的label条件允许用一组值来过滤键。支持三种操作符: in ， notin ,和 exists\(仅针对于key符号\) 。例如：



environment in \(production, qa\)

tier notin \(frontend, backend\)

partition

!partitio

第一个例子，选择所有键等于 environment ，且value等于 production 或者 qa 的资源。 第二个例子，选择所有键等于 tier 且值是除了 frontend 和 backend 之外的资源，和那些没有label的键是 tier 的资源。 第三个例子，选择所有所有有一个label的键为partition的资源；值是什么不会被检查。 第四个例子，选择所有的没有lable的键名为 partition 的资源；值是什么不会被检查。



类似的，逗号操作符相当于一个AND操作符。因而要使用一个 partition 键（不管值是什么），并且 environment 不是 qa 过滤资源可以用 partition,environment notin \(qa\) 。



基于集合的选择器是一个相等性的宽泛的形式，因为 environment=production 相当于environment in \(production\) ，与 != and notin 类似。



基于集合的条件可以与基于相等性 的条件混合。例如， partition in \(customerA,customerB\),environment!=qa 。



API

LIST 和WATCH过滤

LIST和WATCH操作，可以使用query参数来指定label选择器来过滤返回对象的集合。两种条件都可以使用： 基于相等性条件： ?labelSelector=environment%3Dproduction,tier%3Dfrontend基于集合条件的：



labelSelector=environment+in+%28production%2Cqa%29%2Ctier+in+%28frontend%29

两种label选择器风格都可以用来通过REST客户端来列表或者监视资源。比如使用 kubectl 来针对 apiserver ，并且使用基于相等性的条件，可以用：



$ kubectl get pods -l environment=production,tier=frontend

or using set-based requirements: 或者使用基于集合的条件：



$ kubectl get pods -l 'environment in \(production\),tier in \(frontend\)'

如以上已经提到的，基于集合的条件表达性更强。例如，他们可以实现值上的OR操作：



$ kubectl get pods -l 'environment in \(production, qa\)'

或者通过exists操作符进行否定限制匹配：



$ kubectl get pods -l 'environment,environment notin \(frontend\)'

Set references in API objects

一些Kubernetes对象，比如service和replication controlle的，也使用label选择器来指定其他资源的集合，比如pods。



Service and ReplicationController

一个service针对的pods的集合是用label选择器来定义的。类似的，一个replicationcontroller管理的pods的群体也是用label选择器来定义的。



对于这两种对象的Label选择器是用map定义在json或者yaml文件中的，并且只支持基于相等性的条件：



“selector”: {

“component” : “redis”,

}



或者



selector:

component: redis



这个选择器（分别是位于json或者yaml格式的）相等于 component=redis 或者 component in\(redis\) 。



Job and other new resources

Job和其他新的资源

较新的资源，如job，也支持基于集合的条件。



selector:

matchLabels:相当于一个

component: redis

matchExpressions:

– {key: tier, operator: In, values: \[cache\]}

– {key: environment, operator: NotIn, values: \[dev\]}



matchLabels 是一个键值对的映射。一个单独的 {key,value} 相当于 matchExpressions 的一个元素，它的键字段是”key”,操作符是 In ，并且值数组值包含”value”。 matchExpressions 是一个pod的选择器条件的列表。合法的操作符包含In, NotIn, Exists, and DoesNotExist。在In和NotIn的情况下，值的组必须不能为空。所有的条件，包含 matchLabels andmatchExpressions 中的，会用AND符号连接，他们必须都被满足以完成匹配。

