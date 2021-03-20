调用类中方法:
-------------------------------------------
	<!--  调用静态方法来获取对象  -->
    <bean id="factoryCreateUserDao" class="com.zgh.factory.UserFactory" factory-method="createIUserDao"></bean>
-------------------------------------------
---------------------------------------------------------------------------------------



设置bean的作用范围:
	设置scope属性,属性取值一共有五种:
	singleton:单例的(默认值)
	prototype:多例
	requset:作用于web应用的请求范围
	session:作用于web应用的会话范围
	global-session:作用域集群环境会话范围(全局会话范围),当不是集群环境时它就是session
	-------------------------------------------
	<bean id="IUserDaoImp" class="com.zgh.imp.IUserDaoImp" scope="prototype"></bean>
	-------------------------------------------
---------------------------------------------------------------------------------------



根据构造函数来注入参数来创建类:
	创建类User,属性有int id，String userName，Date time。
	注入构造函数:
	-------------------------------------------
	<!--  ref属性表示根据bean对象的id来创建相应的bean对象用来使用  -->
	<bean id="constructorCreateUser" class="com.zgh.entity.User">
        <constructor-arg name="id" value="1"></constructor-arg>
        <constructor-arg name="userName" value="张三"></constructor-arg>
        <constructor-arg name="time" ref="nowTime"></constructor-arg>
    </bean>
    <bean id="nowTime" class="java.util.Date"></bean>
	-------------------------------------------
根据set方法来注入参数:
	注入set方法:
	-------------------------------------------
	<bean id="nowTime" class="java.util.Date"></bean>
    <bean id="setMethodCreateUser" class="com.zgh.entity.User">
        <property name="id" value="1"></property>
        <property name="userName" value="张三"></property>
        <property name="time" ref="nowTime"></property>
    </bean>
	-------------------------------------------
---------------------------------------------------------------------------------------



复杂类型的注入:
	创建类Complex,属性有:String myArray[],List<String> myList,Set<String> mySet,Map<String,String> myMap,Properties myProperties.
	
	注入复杂类:
	-------------------------------------------
	<bean id="complex" class="com.zgh.entity.Complex">
        <property name="myArray">
            <array>
                <value>AAA</value>
                <value>BBB</value>
            </array>
        </property>
        <property name="myList">
            <list>
                <value>AAA</value>
                <value>BBB</value>
            </list>
        </property>
        <property name="mySet">
            <set>
                <value>AAA</value>
                <value>BBB</value>
            </set>
        </property>
        <property name="myMap">
            <map>
                <entry key="1" value="A"></entry>
                <entry key="2" value="B"></entry>
                <entry key="3" value="C"></entry>
            </map>
        </property>
        <property name="myProperties">
            <props>
                <prop key="A">12138</prop>
                <prop key="B">111</prop>
            </props>
        </property>
    </bean>
	-------------------------------------------
---------------------------------------------------------------------------------------