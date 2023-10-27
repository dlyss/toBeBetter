#config
- 打印详细的spirng bean日志
```
#log level
logging:
  level:
    org:
      springframework:
        beans: INFO
```
# 测试(??)
- pom添加springboot-starter-test
- 使用ideal建和main包同级的测试包，包结构和main中的一样？？！！；测试类添加@SpringBootTest注解
```
@SpringBootTest
@AutoConfigureMockMvc
public class MvcMockTest {
    @Autowired
    private MockMvc mockMvc;
    @Test
    public void ListArticles() throws Exception {
      ResultActions resultActions = mockMvc.perform(MockMvcRequestBuilders.get("/articles").param("id", "notUsed_justforexample").header("Authorization",
                "forTest"));
        resultActions.andReturn().getResponse().setCharacterEncoding("UTF-8");
       // resultActions.andExpect(MockMvcResultMatchers.status().isOk()).andDo(print());
        System.out.println(resultActions.andReturn().getResponse().getContentAsString());
    }
}
```
# http请求（restTemplate）
```
//微博示例(post 请求、带请求体httpEntity:请求头header 和/或 请求体body)
        // 根据code换取微博uid和accessToken
        MultiValueMap<String, String> weiboData = new LinkedMultiValueMap<>();
        // 定义微博token请求参数
        weiboData.add(CLIENT_ID, weiboConfigProperties.getAppId());
        weiboData.add(CLIENT_SECRET, weiboConfigProperties.getAppSecret());
        weiboData.add(GRANT_TYPE, weiboConfigProperties.getGrantType());
        weiboData.add(REDIRECT_URI, weiboConfigProperties.getRedirectUrl());
        weiboData.add(CODE, weiBoLoginVO.getCode());
        HttpEntity<MultiValueMap<String, String>> requestEntity = new HttpEntity<>(weiboData, null);
        try {
            return restTemplate.exchange(weiboConfigProperties.getAccessTokenUrl(), HttpMethod.POST, requestEntity, WeiboTokenDTO.class).getBody();
            }catch(Exception e){
             xxx
             }
//示例2
        // 定义请求参数
        Map<String, String> data = new HashMap<>(2);
        data.put(UID, socialTokenDTO.getOpenId());
        data.put(ACCESS_TOKEN, socialTokenDTO.getAccessToken());
        // 获取微博用户信息
        WeiboUserInfoDTO weiboUserInfoDTO = restTemplate.getForObject(weiboConfigProperties.getUserInfoUrl(), WeiboUserInfoDTO.class, data);
```
#mybatis+
