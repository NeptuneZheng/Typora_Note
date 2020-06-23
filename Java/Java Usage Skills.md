# Java Usage Skills

### 1. URLDecoder

> when get prams from URL like below, may exists encoding issue. We choose base64 encode/decode to ensure all chars can transport, however '**=**' is used as a `Fill bit` will be transformed as '**%3D**' in URL.
>
> then we can combine URLDecoder with base64 encode/decode together to solve the issue

```URL
localhost:9528/#/register?token=bkU0Y1ozWXdzTW5DRUtKYVB5UnFHZ255SHJvZGtHOHRBdUFEWXY2dkUreWhGSHMrSFdsMlQ0bjlGY0plejljRlZoMmxxVzZsMjIwdEU1cGpBOHkyZThGZW9HaHVKY2YrYVRMWHVYYVFHOFdTK1c4TkJNa0szNGJZTGt3YmRwZnk1cVBaRFo0bzRObzlCVzRTeFQ1Vks3UjJGYXBBbXl0ZkVqQ005cWcvcXVxR1RvTUY0YWlkVVpTNTc2V0l4SlRxUkdhUGtqcDFDbkFpTHpiKy9TYUM3Mzc4cURvUnJmd2U%3D

```

```groovy
	final Base64.Decoder decoder = Base64.getDecoder()
	final Base64.Encoder encoder = Base64.getEncoder()

	@Autowired
	Gson gson

gson.fromJson(EncryptUtil.decryptPsw(new String(decoder.decode(URLDecoder.decode(registerToken,"UTF-8")),"UTF-8")),Object)
```

