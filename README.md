### Install

First, you should install the Ruby. On the ubuntu, it should be:
```
sudo apt install ruby
```

Then, install the bundler and jekyll
```
gem install bundler jekyll
```

Next, update the gem package
```
bundle install
```

Last, execute the server
```
bundle exec jekyll serve
```

### Errors
1. Package installation error like:
```
An error occurred while installing google-protobuf (3.22.3), and Bundler cannot continue.
```
Delete the Gemfile.lock