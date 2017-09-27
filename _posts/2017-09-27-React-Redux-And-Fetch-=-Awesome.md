>React, Redux And Fetch is love

We know React is a great platform to make UI interactive and experience flawless, But redux is a catalyst which makes your process
much easy. 
I stumbled upon a bug which was killing my time and stopped me almost for a week in development process.
There is a end point in my parse api which will be returning queries. This is how it looks:
Request ->

{% highlight javascript %}
function getdata(){
var data = JSON.stringify({limit: 20, order: "-createdAt" , _method: "GET"});
  const URL = Parse.serverURL+"/classes/sync" ;
  return fetch(URL, { method: 'POST',body: data,
  headers: headers})
     .then( response => return response.json());
     }
{% endhighlight %}

Response ->

{% highlight json %}

{"results":[{"objectId":"przMgvyI1N","executionStatus":["basic-switch-identify-multiple-versions-channels"].........

{% endhighlight %}

Fetch Module was built on Promises. This was a big problem as the react render doesn't wait for a particular time. Async requests
take time to fetch the data, Before react renders the data. Anything assigned like `data = getData()` would return the `data` as `undefined`.

Tried functions such as setInterval to low level functions for accessing the object after fetching. Then redux came into action and solved my problem in matter of minutes

Updated my fetch function to:

{% highlight javascript %}

function getdata(){
var data = JSON.stringify({limit: 20, order: "-createdAt" , _method: "GET"});
  const URL = Parse.serverURL+"/classes/sync" ;
  return fetch(URL, { method: 'POST',body: data,
  headers: headers})
     .then( response => Promise.all([response, response.json()]));
     }
{% endhighlight %}

Added couple of actions to redux:


{% highlight javascript %}

function fetchRequest(){
  return {
    type: "FETCH_REQUEST"
  }
}

function fetchSuccess(payload) {
  return {
    type: "FETCH_SUCCESS",
    payload
  }
}

function fetchError() {
  return {
    type: "FETCH_ERROR"
  }
}
{% endhighlight %}

Now it is time for a reducer :


{% highlight javascript %}

const reducer = (state = {}, action) => {
  switch (action.type) {
    case "FETCH_REQUEST":
      return state;
    case "FETCH_SUCCESS": 
      return { state, fetch: action.payload};
    default:
      return state;
  }
} 

{% endhighlight %}



{% highlight jsx %}

function fetchWithRedux() {
  return (dispatch) => {
    dispatch(fetchRequest());
    return getdata().then(([response, json]) =>{
      if(response.status === 200){
        dispatch(fetchSuccess(json))
      }
      else{
        dispatch(fetchError())
      }
    })
  }
}
.....
....
....
componentDidMount(){
    this.props.fetchWithRedux()
  }
  render(){
    console.log(this.props.fetch)
   if(this.props.fetch !== undefined){
   return(
        <ul>
        {this.props.fetch.results[0].objectId}
        </ul>
    )
   }
   else
   {
    return (<ul>loading</ul>)
   }
  }
  {% endhighlight %}
  
  
  And the end result is first `loading....` and then the `objectId`. 
  The journey of react-redux and fetch continues.
  
  Thanks for this snippet http://jsfiddle.net/cdagli/b2uq8704/6/ by Caner Dağlı 
