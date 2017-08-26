> Csurf can be used for protecting requests against csrf by individual routes or all at once. I had been searching for blogs that explain how to bypass protection internally when all routes are protected. No one explained it clearly.

When you enable csurf in your nodejs app. 

{% highlight js %}

var csrf = require('csurf')
var bodyParser = require('body-parser')
var csrfProtection = csrf({ cookie: true })
//error code
app.use(function (err, req, res, next) {
  if (err.code !== 'EBADCSRFTOKEN') return next(err)

  // handle CSRF token errors here
  res.status(403)
  res.send('{"error":"CSRF Token is invalid","code":"403"}')
})

{% endhighlight %}

The body of the request or header should have csrftoken passing through it or you will end with the error

>{ code: '403', message: 'CSRF Token is invalid' }

First I was tangling with this middleware code, Where every request body requests were strictly verified for csrf protection. 

{% highlight js %}

app.use( csrfProtection, function(req, res, next) {
  res.setHeader('X-Xss-Protection','1; block')
  res.setHeader('X-Frame-Options', 'Deny')
  res.setHeader('X-Content-Type-Options', 'nosniff')
  res.setHeader('x-download-options','noopen')
  res.setHeader('x-permitted-cross-domain-policies','none')
});

{% endhighlight %}

I enabled csrfProtection in the main route. So any request made to the server is strictly asked for csrf token.
When cloud jobs are enabled in the parse cloud code the queries were throwing this error.

{% highlight js %}

ParseQuery {
  className: '_User',
  _where: { sessionToken: 'REDACTED' },
  _include: [],
  _limit: -1,
  _skip: 0,
  _extraOptions: {} }
ParseError { code: '403', message: 'CSRF Token is invalid' }

{% endhighlight %}

Looking at this error I was confident that something is wrong with the middleware usage. If you want to enable csurf for every end point but only bypass few, Do the following thing:

{% highlight js %}

app.use(function(req, res, next) {
   if (req.url === '/api/classes/_Anyclass') return next();
    csrfProtection(req,res,next);
});

{% endhighlight %}

OR

{% highlight js %}

app.use(function(req, res, next) {
   if (req.path === '/path/to/file') return next();
    csrfProtection(req,res,next);
});

{% endhighlight %}
