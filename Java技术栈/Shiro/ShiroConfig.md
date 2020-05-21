### shiro的配置类

```java
package com.shouyuan.shuwu.config;

import com.shouyuan.shuwu.realm.UserRealm;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.LinkedHashMap;
import java.util.Map;

@Configuration
public class ShiroConfig {
    /**
    *@date:2020/5/17
    *@description:密码加密验证
    */
    @Bean("hashedCredentialsMatcher")
    public HashedCredentialsMatcher getHashedCredentialsMatcher(){
        HashedCredentialsMatcher hashedCredentialsMatcher = new HashedCredentialsMatcher();
        //加密次数
        hashedCredentialsMatcher.setHashIterations(1024);
        //加密方法
        hashedCredentialsMatcher.setHashAlgorithmName("MD5");
        //是否盐值加密
        hashedCredentialsMatcher.setStoredCredentialsHexEncoded(true);
        return hashedCredentialsMatcher;
    }

    /**
    *@date:2020/5/17
    *@description:shiro过滤器，所有的请求都要通过这里的验证
    */
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager){
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();

        //设置安全器
        shiroFilterFactoryBean.setSecurityManager(defaultWebSecurityManager);
        /**
         * 内置过滤器
         * anon：匿名用户可访问
         * authc：认证用户可访问
         * user：使用rememberMe可访问
         * perms：对应权限可访问
         * role：对应角色权限可访问
         **/
        Map<String, String> filterMap = new LinkedHashMap<>();
        filterMap.put("/login","anon");
        filterMap.put("/**","authc");
        filterMap.put("/logout", "logout");

        //访问没有权限的页面时跳转到login页面
        shiroFilterFactoryBean.setLoginUrl("/login");
        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterMap);
        return shiroFilterFactoryBean;
    }

    /**
    *@date:2020/5/17
    *@description:核心类，任务下发作用
    */
    @Bean("securityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManger(@Qualifier("userRealm") UserRealm userRealm){
        //关联realm
        DefaultWebSecurityManager securityManager =
                new DefaultWebSecurityManager();
        // 关联realm.
        securityManager.setRealm(userRealm);
        return securityManager;
    }


    /**
    *@date:2020/5/17
    *@description:向容器中注入userRealm用于验证（可以有多个realm）
    */
    @Bean("userRealm")
    public UserRealm getUserRealm(@Qualifier("hashedCredentialsMatcher") HashedCredentialsMatcher hashedCredentialsMatcher){
        UserRealm userRealm = new UserRealm();
        userRealm.setCredentialsMatcher(hashedCredentialsMatcher);
        return userRealm;
    }
}

```

