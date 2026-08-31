# Problems

## ChatGPT cannot access the login page
It turns out that this issue is because Openai uses a new domain `cdn.oaistatic.com` to send some CSS data to the client browser, and this domain blocks me since I haven't added it to my clashy rule. After adding it to the clashy rule, the problem is solved (also need to clean the cache). 
You can use `F11` on the ChatGPT website to check the issue in the Dev tools. In this case, the log tells me that the browser has some issues loading data from `cdn.oaistatic.com`.

## ChatGPT Mac APP / codex cli 'request time out'

**Symptom:** ChatGPT/Codex showed “request timed out 5/5” and waited about 105 seconds before responding.

**Cause:** The app did not inherit the Clash proxy settings and some services does not use the proxy. Besides, the ChatGPT app does not provide an in-app proxy setting.

**Solution:** Launch ChatGPT through a Fish alias that provides the proxy variables:

```fish
alias --save chatgpt 'open -a ChatGPT --env HTTP_PROXY=http://127.0.0.1:7890 --env HTTPS_PROXY=http://127.0.0.1:7890 --env ALL_PROXY=http://127.0.0.1:7890 --env NO_PROXY=localhost,127.0.0.1,::1' 
```

Quit ChatGPT completely, then launch it by typing `chatgpt`.

> `Codex` can have similar problem. Set the proxy variables in the terminal environment.