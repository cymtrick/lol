---
title : Access Error in developer portal in one of the facebook acquisitions. 
description: Developer Auth token not expired after removing developer from shared apps
url : https://prashanthvarmadomma.github.io/Access-Error-In-Developer-Portal-Facebook-Acqusition.html
---
> Developer Auth token not expired after removing developer from shared apps.

Usually Developer portals have invitation option for admin roles or developer roles. One of the facebook acqusitions is having a weird
functionality of adding users directly without permission, but this is not main issue. When invited developer is provided with a auth bearer to make api calls
.This is how functionality works

{% highlight shell %}

curl 

\ -H 'Authorization: Bearer AUTH_BEARER_DEVELOPER_TOKEN' 

\ 'https://api.acqusition.com/message?v=27/08/2017&q=lol'

{% endhighlight %}

Response

{% highlight json %}

{
"msg_id":"0VC6sbwXPAGdN9cVo",
"_text":"lol",
"outcomes":[
            { "confidence":0.52,
              "intent":"lol",
              "_text":"lol",
              "entities":{}
           }],
"WARNING":"DEPRECATED"}

{% endhighlight %}

Even after removing the developer, If he still has access to the previous bearer, api calls were returning response.

When reported to facebook they replied it might be due to cache issues and may take minutes to delete. For a reason I took 3 days and still
the auth bearer was working.

Team looked into the issue and fixed it.

After fix:

{% highlight json %}

{"error":"Bad auth, check token\/params","code":"no-auth"}

{% endhighlight %}

Timeline:
Reported on 31 January 2017<br>
Triaged on 1 February 2017<br>
Team Asked for Clarification on 16 March 2017<br>
Further Clarification sent on 18 March 2017<br>
Fixed on 18 May 2017<br>
Bounty awarded on 6 June 2017
