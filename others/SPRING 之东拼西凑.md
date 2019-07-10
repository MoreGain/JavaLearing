# SPRING 之东拼西凑

### [Spring 的缓存注解 @Cacheable @CacheEvict @CachePut](https://www.cnblogs.com/fashflying/p/6908028.html)

@Cacheable

可用于类和方法，以键值对进行缓存，键有默认策略和自定义策略，值为方法返回结果

- value

  必须指定，对应 cache 的名称，可以为单个也可以为数组

- key

  指定 spring 缓存方法的返回结果是对应的 key 

- condition

