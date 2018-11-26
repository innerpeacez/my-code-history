### RestTemplate访问Basic加密的eureka

报错信息：org.springframework.web.client.HttpClientErrorException: 401 null

两种解决办法：

1.

```java
restTemplate.getInterceptors().add(new BasicAuthorizationInterceptor(username, password));
restTemplate.getForObject(url, String.class);
```

2.

```java
String auth_Str = username + ":" + password;
byte[] encodedAuth = Base64.encodeBase64(auth_Str.getBytes(Charset.forName("US-ASCII")));
String authHeader = "Basic " + new String(encodedAuth);
HttpHeaders headers = new HttpHeaders();
headers.set("authorization", authHeader);
restTemplate.exchange(url, HttpMethod.GET, new HttpEntity<>(null, headers),String.class).getBody();
```