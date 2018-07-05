## spring redis session {#session}

```
cookie 默认名称 SESSION 自定义
```

```
@Bean
public <S extends ExpiringSession> SessionRepositoryFilter<? extends ExpiringSession> springSessionRepositoryFilter(SessionRepository<S> sessionRepository, ServletContext servletContext) {
    SessionRepositoryFilter<S> sessionRepositoryFilter = new SessionRepositoryFilter<S>(sessionRepository);
    sessionRepositoryFilter.setServletContext(servletContext);
    CookieHttpSessionStrategy httpSessionStrategy = new CookieHttpSessionStrategy();
    httpSessionStrategy.setCookieName("MY_SESSION_NAME");
    sessionRepositoryFilter.setHttpSessionStrategy(httpSessionStrategy);
    return sessionRepositoryFilter;
}
```

[https://stackoverflow.com/questions/33095345/how-to-change-spring-session-redis-cookie-name](https://stackoverflow.com/questions/33095345/how-to-change-spring-session-redis-cookie-name)

