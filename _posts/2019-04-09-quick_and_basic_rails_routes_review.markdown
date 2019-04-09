---
layout: post
title:      "Quick and Basic Rails Routes Review"
date:       2019-04-09 16:09:58 +0000
permalink:  quick_and_basic_rails_routes_review
---


This week I'm working on my Ruby on Rails project and I just worked out my SessionsController for signing in and out.  I had to double check some routing details so my URL would read `/login` (instead of `/session/new`), but would still take me to my SessionsController #new action. To make my life easier, I've decided to post a super basic cheat sheet of Rails routing helpers with a few variations.  In honor of spring having finally arrived, I'll be using a Bumblebee model:
```
class Bumblebee < ApplicationRecord
end
```

With a simple controller with only and index action to list all of the bumlebees in the hive:
```
class BumblebeesController < ApplicationController

  def index
    @bees = Bumblebee.all
  end

end
```


To create the route, in config/routes.rb:
```
resources :bumblebees, only: [:index]

# same as -->  get 'bumblebees', to: 'bumblebees#index'
# same as -->  get 'bumblebees' => 'bumblebees#index'
# same as -->  get 'bumblebees', to: 'bumblebees#index', as: 'bumblebees'
```

All four of these variations will get your user to the same place. (*Note:* `, to:`vs. ` => ` is simply user preference.)

If you run `rake routes` or `rails routes` in your command line you see the available route as:
```
 | PREFIX     | VERB | URI PATTERN           | CONTROLLER#ACTION |
 |------------|------|-----------------------|-------------------|
 | bumblebees | GET  | /bumblebees(.:format) | bumblebees#index  |
```


**Let's take a quick detour into the two Rails routing helpers.**

`_path` and `_url` are used within your veiws or controllers to link_to or redirect_to other pages.  These helpers use the “PREFIX” from the rake routes table above.  Therefore:

`bumblebees_url` will give you the full url:  http://localhost:3000/bumblebees

`bumblebees_path` will give you the relative path of the page you want to visit: /bumblebees (Usually when you're routing within your own app you will use this one.)

Both will direct your user to the index action within the bumblebees controller.

*Side note:*

`redirect_to  bumblebees_path` is the same as:

`redirect_to controller: 'bumblebees', action: 'index' ` which explicitly tells your controller where to go, but is just kind of ugly.  I think I'll stick with the helpers...

**Ok, let's get back on track.**


Lets say we decide that we want to change our URI pattern?  After all, bumblebees live together in the hive so I'd like my users to see http://localhost:3000/hive in their URL.  We would do this by changing config/routes.rb to read:
```
get '/hive', to: 'bumblebees#index'
```

`rake routes` now reads:
```
 | PREFIX     | VERB | URI PATTERN           | CONTROLLER#ACTION |
 |------------|------|-----------------------|-------------------|
 | hive       | GET  | /hive(.:format)       | bumblebees#index  |
```

 But this changed our prefix!  Now all of our routing helpers are completely ruined!  Everywhere we used `bumblebees_path` or `bumblebees_url` we now have to change to `hive_path` or `hive_url`.  Ugh.  
 
 Not to worry.  We can fix this by naming the routes using the `as:` option!
Change config/routes.rb to read:
```
get '/hive', to: 'bumblebees#index', as: 'bumblebees'
```

This gives us:
```
 | PREFIX     | VERB | URI PATTERN           | CONTROLLER#ACTION |
 |------------|------|-----------------------|-------------------|
 | bumblebees | GET  | /hive(.:format)       | bumblebees#index  |
```

Great!  Now we have the desired URI pattern and the prefix we were using before.

Let's take a quick look at creating a root route:
```
root 'bumblebees#index'
# same as -->  root :to => 'bumblebees#index'
```

This gives us:
```
 | PREFIX     | VERB | URI PATTERN | CONTROLLER#ACTION |
 |------------|------|-------------|-------------------|
 | root       | GET  | /           | bumblebees#index  |
```

*One last side note:*

POST, PATCH, PUT, and DELETE HTTP verbs never give us a prefix but by the sweet, sweet magic of Rails, what has been passed into the form, and the method called on a button_to or submit tag, Rails knows you're not looking for a GET request and directs it appropriately.  But that's a story for another day.

Don't forget, you can always use `rake routes` or `rails routes` in your command line to get a list of the routes and their prefixes that you currently have set up in your project.

For full details and information on nested routes and whole lot more, check out the [Rails Guides](https://guides.rubyonrails.org/routing.html).
