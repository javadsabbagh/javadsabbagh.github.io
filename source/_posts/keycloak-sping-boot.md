---
title: Connect Spring Boot-2.x to Keycloak-4.6
date: 2019-10-22 15:39:21
tags: [spring, security,keycloak]
categories:
- spring
---

1. Add following dependency to pom.xml
```xml
<dependency>
    <groupId>org.keycloak.bom</groupId>
    <artifactId>keycloak-adapter-bom</artifactId>
    <version>4.6.0.Final</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

```xml
<dependency>
    <groupId>org.keycloak</groupId>
    <artifactId>keycloak-spring-boot-starter</artifactId>
</dependency>
```

```java
import org.kodnuts.common.web.filter.CorsFilter;
import org.keycloak.adapters.KeycloakConfigResolver;
import org.keycloak.adapters.springboot.KeycloakSpringBootConfigResolver;
import org.keycloak.adapters.springsecurity.KeycloakSecurityComponents;
import org.keycloak.adapters.springsecurity.authentication.KeycloakAuthenticationProvider;
import org.keycloak.adapters.springsecurity.client.KeycloakClientRequestFactory;
import org.keycloak.adapters.springsecurity.client.KeycloakRestTemplate;
import org.keycloak.adapters.springsecurity.config.KeycloakWebSecurityConfigurerAdapter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.beans.factory.config.ConfigurableBeanFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.authority.mapping.SimpleAuthorityMapper;
import org.springframework.security.core.session.SessionRegistryImpl;
import org.springframework.security.web.access.channel.ChannelProcessingFilter;
import org.springframework.security.web.authentication.session.RegisterSessionAuthenticationStrategy;
import org.springframework.security.web.authentication.session.SessionAuthenticationStrategy;
import org.springframework.util.StringUtils;

@Configuration
@EnableWebSecurity
@ComponentScan(basePackageClasses = KeycloakSecurityComponents.class)
public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter {
    @Autowired
    public KeycloakClientRequestFactory keycloakClientRequestFactory;
    @Value("${spring.http.allowedOrigins:}")
    private String allowedOrigins;

    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) {
        KeycloakAuthenticationProvider keycloakAuthenticationProvider = keycloakAuthenticationProvider();
        SimpleAuthorityMapper grantedAuthorityMapper = new SimpleAuthorityMapper();
        grantedAuthorityMapper.setPrefix("ROLE_");
        keycloakAuthenticationProvider.setGrantedAuthoritiesMapper(grantedAuthorityMapper);
        auth.authenticationProvider(keycloakAuthenticationProvider);
    }

    @Bean
    @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
    public KeycloakRestTemplate keycloakRestTemplate() {
        return new KeycloakRestTemplate(keycloakClientRequestFactory);
    }

    @Bean
    @Override
    protected SessionAuthenticationStrategy sessionAuthenticationStrategy() {
        return new RegisterSessionAuthenticationStrategy(new SessionRegistryImpl());
    }

    @Bean
    public KeycloakConfigResolver KeycloakConfigResolver() {
        return new KeycloakSpringBootConfigResolver();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
        http
                .csrf().disable()
                .authorizeRequests()
                .antMatchers("/favicon.ico").permitAll()
                .antMatchers("/change-lang").permitAll()
                //
                .antMatchers("/").permitAll()
                .antMatchers("/js/**").permitAll()
                .antMatchers("/css/**").permitAll()
                .antMatchers("/config/**").permitAll()
                .antMatchers("/images/**").permitAll()
                .antMatchers("/fonts/**").permitAll()
                .antMatchers("/lib/**").permitAll()
                .antMatchers("/help/**").permitAll()
                .antMatchers("/**/**.js").permitAll()
                .antMatchers("/**/**.css").permitAll()
                .antMatchers("/**/**.html").permitAll()
                //Swagger
                .antMatchers("/swagger-resources/**").permitAll()
                .antMatchers("/swagger-ui.html").permitAll()
                .antMatchers("/webjars/**").permitAll()
                .antMatchers("/v2/api-docs").permitAll()
                .antMatchers(HttpMethod.GET, "/actuator/health/**").permitAll()
                .anyRequest().authenticated();
        if (StringUtils.hasText(allowedOrigins)) {
            http.addFilterBefore(new CorsFilter("/**",
                            allowedOrigins,
                            "POST, GET, OPTIONS, DELETE, PUT",
                            "Authorization, Content-Type, Accept, Accept-Language, x-requested-with, Cache-Control, Content-Disposition",
                            true,
                            3600)
                    , ChannelProcessingFilter.class);
        }

    }

}
```

```ini
#####KeyCloak#####
keycloak.realm=Test-Realm
keycloak.auth-server-url=http://192.168.100.111/auth
keycloak.ssl-required=external
keycloak.resource=my-app
keycloak.public-client=true
keycloak.bearer-only=true
keycloak.credentials.secret=c66ac43f-5ceb-449c-a200-6ec67001e80e
```

```java
import org.kodnuts.common.model.User;
import org.kodnuts.common.security.SecurityContext;
import org.keycloak.KeycloakPrincipal;
import org.keycloak.adapters.springsecurity.account.SimpleKeycloakAccount;
import org.keycloak.representations.AccessToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.stereotype.Component;

import java.security.Principal;

@Component
public class SecurityContext {
    @Override
    public User getUser() {
        AccessToken token = null;
        try {
            Principal principal = ((SimpleKeycloakAccount) SecurityContextHolder.getContext().getAuthentication().getDetails()).getPrincipal();
            token = ((KeycloakPrincipal) principal).getKeycloakSecurityContext().getToken();
        } catch (Exception e) {
            System.out.println(e);
        }
        if (token == null || token.getOtherClaims() == null) {
            throw new Exception("operation.center.generate.deposit.file.setup.error");
        }
        Long userId = Long.valueOf(token.getOtherClaims().getOrDefault("userId", null).toString());
        Long organId = Long.valueOf(token.getOtherClaims().getOrDefault("organId", null).toString());
        Long organCode = Long.valueOf(token.getOtherClaims().getOrDefault("organCode", null).toString());
        Integer personalCode = Integer.valueOf(token.getOtherClaims().getOrDefault("personalCode", null).toString());
        String mac = token.getOtherClaims().getOrDefault("serverMac", null).toString();
        if (organId == null || userId == null || organCode == null || personalCode == null|| StringUtils.isEmpty(mac)) {
            throw new Exception("operation.center.generate.deposit.file.setup.error");
        }
        User user = new User(userId);
        user.setOrganId(organId);
        user.setOrganCode(organCode);
        user.setPersonalCode(personalCode);
        user.setMac(mac);
        return user;
    }
}
```

```java
import java.util.Objects;

@Data
public class User {
    private Long id;
    private String userName;
    private String password;
    private String firstName;
    private String lastName;
    private Object status;
    private Byte statusCode;
    private String keycloakId;
    private String email;
    private Object organ;
    private Long organId;
    private Long organCode;
    private Integer personalCode;
}
```