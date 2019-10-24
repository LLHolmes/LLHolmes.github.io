---
layout: post
title:      "Writing RSpec tests…the beginning"
date:       2019-10-24 17:08:56 +0000
permalink:  writing_rspec_tests_the_beginning
---

I’ve been looking for my first job as a software engineer and no matter what type of development you want to focus on, testing is important.  It saves time while making sure your code actually does what it’s intended to do.  More importantly, it makes sure that your code *continues* to do what it’s supposed to do.  If you add a feature and it breaks code that is already functional (and potentially already deployed), testing should catch it before it gets out of hand.  And the beauty of writing tests vs. testing manually is that once a test is written, it can be used over and over again without any extra work.  The alternative is having to manually retest *every* aspect of your code *every* time something is changed.  Yikes.

Like all good coding skills, it’s learned best through practice, so I’ve been tinkering with writing basic tests using my good friends Ruby and RSpec.

### Create a project

First, I created a basic project to practice in.
```
mkdir test-fun
cd test-fun
```
I created a `Gemfile` to which I added the following:
```
source "https://rubygems.org"
gem 'rspec'
```
Then I ran `bundle` (so I wouldn’t forget) and `rspec --init`.

`rspec --init` created a `.rspec` file and a `spec/spec_helper.rb` file for me so I would have all the basics set up.  At this point, you can basically ignore that spec_helper file.

Inside that spec folder, I also created a `practice_spec.rb` file which will hold my tests.

And at the root of my project I created a `lib/practice.rb` file so I would have a place to put my work (the code I'm writing that needs to be tested).

That should be the basics of my little project.  On to the testing!

### Start writing tests

Inside `practice_spec.rb`, I connected the spec_helper and the file my work would be found in by adding:
```
require_relative 'spec_helper'
require_relative '../lib/practice.rb'
```

**Remember**: use `rspec` or `bundle exec rspec` to run the tests in your terminal.

With RSpec, you’ll be describing what you want a method, class, object, etc. to do.  `describe` will accept a class name, or a string.  I’m starting with the basics, so we’ll work with a string for a simple method.  

In `practice_spec.rb` add:
```
describe "#say_hello" do
end
```
The `#` denotes that `say_hello` is an instance method.

**FYI**: When you get into testing classes, you may see a string that begins with a `.` instead, that denotes a class method.

If you run `rspec` at this point, it should pass.  So let’s add a test!
```
describe "#say_hello" do
  it 'returns a greeting' do
    expect(say_hello()).to eq("Hello, friend!")
  end
end
```
That wasn’t so bad, was it?  

`it` says what you want it to accomplish.  When your test fails, this is what will be output as part of the explanation of why it failed.

Now let’s give the test a try:
```
rspec
F

Failures:

  1) #say_hello returns a greeting
     Failure/Error: expect(say_hello()).to eq("Hello, friend!")
     
     NoMethodError:
       undefined method `say_hello' for #<RSpec::ExampleGroups::SayHello:0x00007fcf0f94f940>
     # ./spec/practice_spec.rb:6:in `block (2 levels) in <top (required)>'

Finished in 0.00386 seconds (files took 0.16798 seconds to load)
1 example, 1 failure

Failed examples:

rspec ./spec/practice_spec.rb:5 # #say_hello returns a greeting
```

Sweet!  Our first error.  Let’s make it pass.  

In practice.rb:
```
def say_hello
  "Hello, friend!"
end
```

Now our test is passing.  So let’s add another test!

```
describe "#say_hello" do
  it 'returns a greeting' do
    expect(say_hello()).to eq("Hello, friend!")
  end
  
  it 'returns a greeting to a specific person if one is given' do
    expect(say_hello("Lana")).to eq("Hello, Lana!")
  end
end
```

When we run the test we see that we had 2 examples, and 1 failure:
```
rspec
.F

Failures:

  1) #say_hello returns a greeting to a specific person if one is given
     Failure/Error:
       def say_hello
         "Hello, friend!"
       end
     
     ArgumentError:
       wrong number of arguments (given 1, expected 0)
     # ./lib/practice.rb:1:in `say_hello'
     # ./spec/practice_spec.rb:10:in `block (2 levels) in <top (required)>'

Finished in 0.00362 seconds (files took 0.21786 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/practice_spec.rb:9 # #say_hello returns a greeting to a specific person if one is given
```

So back to our method, to make it work:
```
def say_hello(name="friend")
  "Hello, #{name}!"
end
```

Test again and it’s all green!  Let's add another!...

And so it goes until you have the finished project you’re looking for.

### Keep practicing

[Here is a great tutorial](https://semaphoreci.com/community/tutorials/getting-started-with-rspec) for taking the next step with testing.

Good luck, friends!
