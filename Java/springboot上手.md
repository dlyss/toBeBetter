# 测试
- pom添加springboot-starter-test
- 测试类添加@SpringBootTest注解，注意：扫描Bean时要将该测试类排除，部分工程使用：com.xx.xx.*方式扫码，会将该测试类包含，导致启动失败
- 排除方式：@ComponentScan(excludeFilters = @ComponentScan.Filter(type= FilterType.REGEX,pattern = "com.minzheng.blog.test*"))
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
