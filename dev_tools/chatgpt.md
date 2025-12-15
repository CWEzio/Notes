# Problems

## ChatGPT cannot access the login page
It turns out that this issue is because Openai uses a new domain `cdn.oaistatic.com` to send some CSS data to the client browser, and this domain blocks me since I haven't added it to my clashy rule. After adding it to the clashy rule, the problem is solved (also need to clean the cache). 
You can use `F11` on the ChatGPT website to check the issue in the Dev tools. In this case, the log tells me that the browser has some issues loading data from `cdn.oaistatic.com`.