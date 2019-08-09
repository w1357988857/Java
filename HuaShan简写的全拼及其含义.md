**PO(Persistant Object) 持久对象 **
		用于表示数据库中的一条记录映射成的 java 对象。PO 仅仅用于表示数据，没有任何数据操作。通常遵守 Java Bean 的规范，拥有 getter/setter 方法。

**BO(Business Object) 业务对象 **
		封装对象、复杂对象，里面可能包含多个类 
		主要作用是把业务逻辑封装为一个对象。这个对象可以包括一个或多个其它的对象。

**VO(Value Object) 表现对象**

​		前端界面展示；value object值对象；ViewObject表现层对象；主要对应界面显示的数据对象。对于一个WEB页面，或者SWT、SWING的一个界面，用一个VO对象对应整个界面的值；对于Android而言即是activity或view中的数据元素。

**DTO(Data Transfer Object) 数据传输对象 **
		前端调用时传输；也可理解成“上层”调用时传输; 用于表示一个数据传输对象。DTO 通常用于不同服务或服务不同分层之间的数据传输。

**DAO(Data access object) 数据访问对象 **

​		主要用来封装对数据库的访问。通过它可以把POJO持久化为PO，用PO组装出来VO、DTO；用于表示一个数据访问对象。使用 DAO 访问数据库，包括插入、更新、删除、查询等操作，与 PO 一起使用。DAO 一般在持久层，完全封装数据库操作，对外暴露的方法使得上层应用不需要关注数据库相关的任何信息。

**POJO：**（Plain Ordinary Java Object）简单的Java对象，实际就是普通JavaBeans，是为了避免和EJB混淆所创造的简称，

**EJB：**Enterprise Java Beans(EJB)称为Java 企业Bean，是Java的核心代码，分别是会话Bean（Session Bean），实体Bean（Entity Bean）和消息驱动Bean（MessageDriven Bean）。

