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



### spring boot global DateTimeFormat 

> 修复@DateTimeFormat注解失效问题

```
    @Bean
    public ConfigurableWebBindingInitializer getConfigurableWebBindingInitializer() {
        ConfigurableWebBindingInitializer initializer = new ConfigurableWebBindingInitializer();
        FormattingConversionService conversionService = new WebConversionService("yyyy-MM-dd");//gobal without @DateTimeFormat
        new DateFormatterRegistrar().registerFormatters(conversionService); //annotation supported
        //we can add our custom converters and formatters
        conversionService.addConverter(new EnumConverter<>(TradeType.class));
        conversionService.addConverter(new EnumConverter<>(TradePlatform.class));
        //conversionService.addFormatter(...);
        initializer.setConversionService(conversionService);
        //we can set our custom validator
        //initializer.setValidator(....);

        //here we are setting a custom PropertyEditor
        /*    initializer.setPropertyEditorRegistrar(propertyEditorRegistry -> {
            SimpleDateFormat dateFormatter = new SimpleDateFormat("yyyy-MM-dd");
            propertyEditorRegistry.registerCustomEditor(Date.class,
                    new CustomDateEditor(dateFormatter, true));
        });*/
        return initializer;
    }
```



