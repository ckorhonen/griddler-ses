# Griddler::Ses

This is a [Griddler](https://github.com/thoughtbot/griddler) adapter that allows you to parse email replies when used with Amazon SES.


## Installation

Add these lines to your application's Gemfile:

```ruby
gem 'griddler'
gem 'griddler-ses'
```

And then execute:

    $ bundle


## Usage

1. Setup Amazon SES to receive emails -- see http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-setting-up.html

2. From AWS SES -> Rule Sets, choose to "Create Receipt Rule" and choose to use SNS.  Add an action with a new SNS topic that ends in that value which you provides in the settings. For example, by default it is *`_griddler`*. Encoding must be set to Base64 one.

3. Go to AWS SNS (Simple Notification Service) -> Topics.  Click on your `_griddler` topic (depending on how you named it).  Add a subscription that points to your configured griddler endpoint (the default route is `/email_processor`).  You're server must be already running to confirm the subscription request!

4. In your griddler handler, be sure to handle/ignore empty reply emails (ie. check for `email.headers.empty?`) to account for the fact that some hooks are just SNS's subscription handling.

## Configuration

In order to provide some custom configuration, please, add `config/initializers/griddler_ses.rb` file with the following content:

```ruby
Griddler::Ses.config do |config|
  # config.topic_suffix = 'griddler'
  # config.aws_region = ''
  # config.aws_access_key_id = ''
  # config.aws_secret_access_key = ''
end
```

## Processing big emails

By default SNS is able to process email with size less or equal 150Kb (the size is calculated with images, headers, text body). To be able to
accept emails with big images you must to setup flow, when email body is uploaded to the S3 and to the server the information where to find
that email content will be posted. So the flow for such cases will be a little different: there is no content in the payload, you must to
read it from S3 file instead. Flow with S3 has limitations as well: your email can has maximum size 30Mb.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/85x14/griddler-ses.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

