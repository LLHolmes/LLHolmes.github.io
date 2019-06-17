---
layout: post
title:      "Home Sweet Homebuyer’s Helper"
date:       2019-06-17 00:08:38 +0000
permalink:  home_sweet_homebuyer_s_helper
---


I made it!  I just wrapped up my final FlatIron project using React Redux!  I built a Homebuyer’s Helper app that implements the Zillow API to search to find information on an address that the user inputs and also produces a list of comparable properties.  Useful if you’re thinking of buying or selling a home, want to keep tabs on your real estate investments, or need to contest the dreaded assessor’s office!

It’s one page.  Just one page.  (Ok, I was required to have at least three routes so I added a login, signup, and account page…but for the most part it’s just one page…)  And it took me ALL WEEK to build it.  Part of it was that I took the time to figure out how to authenticate users using React.  After that was put it place, I feel like things started to pick up quite a bit.

I think I have three big take aways from this project:
1. Build your back end and have it functioning first before creating your react app.  I apparently like to jump in with both feet, so I just created Rails project, created the react app, and then started to build everything out.  Lesson learned, guys.  I couldn’t figure out how to get just my API json content to show on a route after I had implemented react.  I didn’t have any major issues, but I also had an extremely simple back end and I can see it getting very hairy if your API hits a snag somewhere.  So just build your back end first and then move on to React.

2. We’ve all heard it before, but XML API’s are no where near as slick at JSON.  I’m glad I had this experience to learn a bit about it since I know I’ll have to deal with it in the future, but if everyone could just convert their API’s to JSON, I would really appreciate it, ok?  Do it for me. ;)
(Bonus Zillow information to follow...)

3. I had a revelation when working with the react-router-dom withRouter function.  I was trying and trying to use history.push to change my url and failing spectacularly until I finally understood history as a prop for react Routes.

If you’re in a Route component - for example: 

```<Route exact path =“/signup" component={ Signup } />```

...and you’re in that Signup component, you get history as a prop passed in automatically with the the Route.  You can call this.props.history.push(‘/‘) and your url will change.
If not, you have to use withRouter.  But!  withRouter still needs that history prop!  For example, I had a Account page and on that account page there was another component to delete the account.  So Account was my Route:

```<Route exact path =“/account" component={ Account } />```

...which was given the history prop.  So my unsubscribe component would need to use withRouter, but I also needed to pass that history prop into it:

```<Unsubscribe history={this.props.history} />```

Then I could import withRouter and wrap the component according to [the docs](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/withRouter.md)
and it worked like a charm!
Note:  If you're wrapping a component that is also connecting to Redux, wrap connect and all:

```export default withRouter(connect(null, { unsubscribeUser })(Unsubscribe));```
<br>
<br>
<br>

*Bonus Zillow Information*:

The Zillow API is shockingly undocumented, and since it took some digging, I wanted to list the different routes I found for anyone else digging around.

[API Overview (links to branding requirements, logo, etc.)](https://www.zillow.com/howto/api/APIOverview.htm)

[GetSearchResults (single home from address)](https://www.zillow.com/howto/api/GetSearchResults.htm)

[GetDeepSearchResults (single home from address with a bit more info)](https://www.zillow.com/howto/api/GetDeepSearchResults.htm)

[GetComps (up to 25 comps from zpid)](https://www.zillow.com/howto/api/GetComps.htm)

[GetDeepComps (up to 25 comps from zpid with a bit more info)](https://www.zillow.com/howto/api/GetDeepComps.htm)

[GetZestimate (Zestimate, ZRental estimate, etc.)](https://www.zillow.com/howto/api/GetZestimate.htm)

Please let me know if you found any more options for the Zillow API!

Also, take a look at [my project](https://github.com/LLHolmes/relocation-helper) and let me know what you think!
