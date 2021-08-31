# key的作用
v-for中key属性的值应唯一，起到身份认证的作用，避免v-for“就地更新”策略导致的问题。为了让Vue能跟踪每个节点的身份，从而重用和重新排序现有元素，需要为每项提供一个唯一 key

https://blog.csdn.net/yihanzhi/article/details/112951243

# 什么不能用index作为key
修改数据时，如果被发现与相应key的绑定关系有变化，会被重新渲染，从而影响性能。

如果使用唯一id作为key，修改数据后，剩下的元素因为与key的关系没有发生变化，都不会被重新渲染，从而达到提升性能的目的。

https://blog.csdn.net/AiHuanhuan110/article/details/98223011