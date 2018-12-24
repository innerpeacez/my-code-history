### 引入对应依赖

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.5</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.54</version>
</dependency>
<dependency>
    <groupId>com.cedarsoft.serialization</groupId>
    <artifactId>jackson</artifactId>
    <version>8.7.0</version>
</dependency>
```

### Json工具类

```java
/**
 * @author innerpeacez
 * @since 2018/12/24
 */
public class JsonUtils {

    private static class FastJsonUtil {
        private static <T> String bean2Json(T obj) {
            return JSON.toJSONString(obj);
        }

        public static <T> T json2Bean(String jsonStr, Class<T> objClass) {
            return JSON.parseObject(jsonStr, objClass);
        }
    }

    private static class GsonUtil {
        private static Gson gson = new GsonBuilder().create();

        private static <T> String bean2Json(T obj) {
            return gson.toJson(obj);
        }

        public static <T> T json2Bean(String jsonStr, Class<T> objClass) {
            return gson.fromJson(jsonStr, objClass);
        }
    }

    private static class JacksonUtil {
        private static ObjectMapper mapper = new ObjectMapper();

        private static <T> String bean2Json(T obj) {
            try {
                return mapper.writeValueAsString(obj);
            } catch (JsonProcessingException e) {
                e.printStackTrace();
                return null;
            }
        }

        private static <T> T json2Bean(String jsonStr, Class<T> objClass) {
            try {
                return mapper.readValue(jsonStr, objClass);
            } catch (IOException e) {
                e.printStackTrace();
                return null;
            }
        }
    }
}
```