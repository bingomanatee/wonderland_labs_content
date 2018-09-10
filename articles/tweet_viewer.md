Twitter's default UI is pretty harsh to scan. You can download your tweets, but thats a bit of a chore. 

I've created a simple Tweet datagrid; all you have to do is to log on and click the "My Tweets" link on the left navigation under your login name. 

### A note on access and privacy

The API I use to acquire the tweets is twitter's [REST API](https://dev.twitter.com/docs/api). That is, I do not use your login information to 

* access any personal information/Direct Messages from your twitter account
* write to your account in any way, or write tweets in your name
* access private/deleted tweets

The code behind this tool is [Open Source](https://github.com/bingomanatee/wonderland_labs/tree/master/frames/social) for those with any doubts. 

### Access

You can only view the tweets by logging in, and you can only view your own tweets. You cannot share the link to let others view your tweets (though a "Publish" option is in the works.)

### Tips and Tricks

the datagrid is a stock Angular datagrid; you can sort by columns, and group tweets by dragging column heads into the group bar at the top. 

You can view a tweet "ticket" by clicking on any row.