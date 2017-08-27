---
Title : Access Error in developer portal,One of facebook acquisitions. 
Description: developer Auth token not expired after removing developer from shared apps
---
> Developer Auth token not expired after removing developer from shared apps,One of facebook acquisitions.

Usually Developer portals have invitation option for admin roles or developer roles. One of the facebook acqusitions is having a weird
functionality of adding users directly without permission, but this is not main issue. When invited developer is provided with a auth bearer to make api calls
.This is how it functionality works

{% highlight shell %}

curl 

\ -H 'Authorization: Bearer AUTH_BEARER_DEVELOPER_TOKEN' 

\ 'https://api.acqusition.com/message?v=27/08/2017&q=lol'

{% endhighlight %}

Response

{% highlight json %}

{"msg_id":"0VC6sbwXPAGdN9cVo",
"_text":"lol",
"outcomes":[
            { "confidence":0.52,
              "intent":"lol",
              "_text":"lol",
              "entities":{}
           }],
"WARNING":"DEPRECATED"}

{% endhighlight %}

Even after removing the developer, If he still has access to the previous bearer api calls were returning response.
When reported to facebook they replied it might be due to cache issues and may take minutes to delete. For a reason I took 3 days and still
the auth bearer was working.Team looked into the issue and fixed it.

Timeline:
Reported on 31 January 2017<br>
Triaged on 1 February 2017<br>
Team Asked for Clarification on 16 March 2017<br>
Further Clarification sent on 18 March 2017<br>
Fixed on 18 May 2017<br>
Bounty awarded on 6 June 2017
