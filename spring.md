## springboot build.gradle with profile  {#session}

```
$ bootRun -Dspring.profile.active=dev


//build.gradle
bootRun {
	systemProperties = System.properties
}

```

##  {#session}

##  {#session}

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

### 

### spring global string to enum converter factory  自动转化枚举类型作为参数

```
public class StringToEnumConverterFactory implements ConverterFactory<String, Enum> {

    @Override
    public <T extends Enum> Converter<String, T> getConverter(Class<T> targetType) {
        return new StringToEnumConverter<>(targetType);
    }

    private final class StringToEnumConverter<T extends Enum> implements Converter<String, T> {

        private Class<T> enumType;

        StringToEnumConverter(Class<T> enumType) {
            this.enumType = enumType;
        }

        public T convert(String source) {
            return (T) Enum.valueOf(this.enumType, source.trim());
        }
    }
}
```

```
 conversionService.addConverterFactory(new StringToEnumConverterFactory());
```



