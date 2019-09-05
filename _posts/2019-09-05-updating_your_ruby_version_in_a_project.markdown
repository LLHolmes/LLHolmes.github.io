---
layout: post
title:      "Updating your Ruby Version in a Project"
date:       2019-09-05 12:40:12 -0400
permalink:  updating_your_ruby_version_in_a_project
---


Here’s another brief addition to “Lana’s Quick And Basic Guide To Blundering Through Learning To Code”.

I’m not going to go over updating Ruby to the latest version, there are a lot of great guides out there for that already (see below for a few I found helpful).  But there’s a tiny little gap that I stumbled across when it came to updating my Ruby version in a project.

I was pretty sure I had updated the Ruby version on my Mac earlier, but coming back to refactor a project later, I got an error upon running ​bundle:

`Your Ruby version is 2.6.1, but your Gemfile specified 2.6.3`

Huh.  I’m swear I updated my Ruby version earlier (why else would I have changed the version in my Gemfile?).

```
ruby -v​

=>  ruby 2.6.1p33 (2019-01-30 revision 66950) [x86_64-darwin17]
```

Ok, maybe I’m mistaken.  I guess I’ll update my Ruby version.

```
brew install ruby​

=>  Updating Homebrew…
=>  Warning: ruby 2.6.3 is already installed and up-to-date
=>  To reinstall 2.6.3, run `brew reinstall ruby`
```

So I’m not delirious!  I have done this before!  So why on Earth is it giving me the earlier error?

What I tried and what I learned:

```
rvm use ruby-2.6.3​

=>  ruby 2.6.3p62 (2019-04-16 revision 67580) [x86_64-darwin17]
```
*This command will change to the designated Ruby version, BUT!  Only temporarily.  It will go back to the default after restarting the terminal.*

This must have been my issue!  I needed to set my default!  To set the new ruby version as my default, I ran:

```
rvm --default use 2.6.3

=>  ruby 2.6.3p62 (2019-04-16 revision 67580) [x86_64-darwin17]
```
*This will set the default Ruby version to 2.6.3, so the even if I restart the terminal, the Ruby version will be set to 2.6.3.*

Great!  Solved!
But then I cd’d into my project and tried to bundle and was given the same error as before.

![](https://media.giphy.com/media/8pMS5BXOUVZyo/giphy.gif)

So there was a difference when running `ruby -v` in the root file vs. in my project folder.

After a bit more googling, I came across this [post on Stack Overflow](https://stackoverflow.com/questions/15708916/use-rvmrc-or-ruby-version-file-to-set-a-project-gemset-with-rvm).

What is this `.rvmrc`​/​`.ruby_version`​they speak of?
Well, [Rails 5.2 automatically generates](https://blog.bigbinary.com/2018/05/07/rails-5_2-adds-ruby-version-file-and-ruby-version-to-gemfile-by-default.html) a `.ruby_version` project file and adds the Ruby version to the Gemfile when creating an app.  `.ruby_version`​ helps version managers (like rvm) know…well...which version of Ruby to use.  When peeking at my file, it contained one line: `ruby-2.6.1`.

AH-HA!!!  That’s where my trouble is located!  I updated it to `ruby-2.6.3`, restarted my terminal, cd’d into the folder, and checked my Ruby version yet again.

![](https://media.giphy.com/media/uxLVaMUiycgpO/giphy.gif)

It was finally correct.  At last, I could `bundle install` successfully without having to manually change the Ruby version and I could be on my merry way.

**So the TL;DR moral of the story...to update your Ruby version:**
1. Update on the command line.  Some resources I found helpful:
  * [How to update Ruby Version 2.0.0 to the latest version in Mac OSX Yosemite? - Stack Overflow](https://stackoverflow.com/questions/38194032/how-to-update-ruby-version-2-0-0-to-the-latest-version-in-mac-osx-yosemite)
  * [UPDATE RUBY TO LATEST VERSION ON MAC OS X - The Coding Pad](http://codingpad.maryspad.com/2017/04/29/update-mac-os-x-to-the-current-version-of-ruby/)
  * [Installing Ruby - ruby-lang.org](https://www.ruby-lang.org/en/documentation/installation/)
2. Set your new version as the default.
  * [Setting the default Ruby - rvm.io](https://rvm.io/rubies/default)
3. Update any projects necessary in both the `Gemfile` **AND** the `.ruby_version`​ file.



***Bonus command:***
```
rvm list​

=>     ruby-2.3.3 [ x86_64 ]
=>     ruby-2.6.1 [ x86_64 ]
=>  =* ruby-2.6.3 [ x86_64 ]
=>  
=>  # => - current
=>  # =* - current && default
=>  #  * - default
```
*This will give you a list of the Ruby versions on your computer as well as tell you which is default and which is currently being used.  Helpful!
*
