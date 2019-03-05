---
layout: post
title:      "Creating attr_accessor In A Dynamic ORM"
date:       2019-03-05 16:19:07 +0000
permalink:  creating_attr_accessor_in_a_dynamic_orm
---


The idea of dynamic object relational mapping (ORM) within Ruby is pretty sweet, right?  You basically create a table and let Ruby deal with all the SQL mumbo-jumbo to build the ORM methods for you.  Now, I say “SQL mumbo-jumbo” with all due respect.  Yes, it is important to know SQL and understand how it works, but using dynamic ORMs is just such a time saver!  It does all the work for you while keeping your application organized and connected to your database.  To continue the theme of doing as little as possible while keeping our code DRY, dynamic programming uses metaprogramming.  This helps us keep everything as generic as possible so we only have to write the code once in a parent class, and then share it with all the other/child classes.

There's obviously a lot of dynamic ORM methods we could talk about, but this post is only concerned with one.  I following along with the ORM methods in the parent class (#table_name, #column_names, #initialize, #save, etc.) really well.  But then there was the one bit of code we need to put in our child class.  We needed it to build the attr_accessors specific to that class (based on the table's column names):

```
require_relative "../parent.rb"

class Child < Parent

  self.column_names.each do |col_name|
    attr_accessor col_name.to_sym
  end

end
```

I was a little stumped.
Logically, I understand what is being accomplished and how:

```	
  # Iterate through the column_names of the table.
  self.column_names.each do |col_name|
	# Set an attr_accessor for each one, making sure to convert them to symbols.
    attr_accessor col_name.to_sym
  end
```

But...it's in a class...

But...it's not a typical class method...

So...how do we call it to make sure our attr_accessors get built???

And then it hit me!  We don't!  When the application is launched it basically runs through all the code once, *including* the code in all the classes.  That little nugget automatically runs and builds the attr_accessors upon launch!

If you think about it, it's the same way our attr_accessor are built normally with the `attr_accessor :attribute1 :attribute2 :etc` sitting at the top of the class.  Ruby builds those reader and writer methods when it runs across that code at launch.

An even simpler example:

```
class Tattoo_You

  puts "If you start me up..."

  def self.credits
    puts "The Rolling Stones"
  end
  
end

puts "You make a grown man cry"
```

Which when run, gives me:

```
  => If you start me up...
  => You make a grown man cry
```

You have to specifically call class methods:

```
Tattoo_You.credits

  => The Rolling Stones
```

If creating the attr_accessors had be built into a class method (like #credits here), it would need to be specifically called before your application could use those attr_accessors...and goodness knows a few of us out there would forget to do that from time to time.

Sneaky, Ruby, sneaky...getting everything set up behind the scenes, completely efficiently, without any extra work for us.  I appreciate it, Ruby.  Good lookin' out.
