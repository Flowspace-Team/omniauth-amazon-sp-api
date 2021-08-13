# OmniAuth::AmazonSpApi

Website authorization flow for Amazon Selling Partner APIs.

https://github.com/amzn/selling-partner-api-docs/blob/main/guides/en-US/developer-guide/SellingPartnerApiDeveloperGuide.md#website-authorization-workflow

# Configuration

Here's an example for adding the middleware to a Rails app in `config/initializers/omniauth.rb`:

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :amazon_sp_api, ENV['AMAZON_SP_API_CLIENT_ID'], ENV['AMAZON_SP_API_CLIENT_SECRET'],
           redirect_uri: "https://example.com/auth/amazon_sp_api/callback", # if you need to override the default URL generated by the gem
           authorize_params: {
             application_id: ENV['AMAZON_SP_API_APPLICATION_ID'],
             version: 'beta', # If you app is in Draft state, this is required.
           },
           client_options: {
             authorize_url: 'https://sellercentral.amazon.com/apps/authorize/consent',
             site: Rails.env.production? ? 'https://sellingpartnerapi-na.amazon.com' : 'https://sandbox.sellingpartnerapi-na.amazon.com'
           }
end
```

## Using devise

Here's an example if you are using Devise. Add this to `config/initializers/devise.rb`:

```ruby
  config.omniauth :amazon_sp_api, ENV['AMAZON_SP_API_CLIENT_ID'], ENV['AMAZON_SP_API_CLIENT_SECRET'], {
    redirect_uri: "https://example.com/auth/amazon_sp_api/callback", # if you need to override the default URL generated by the gem
    authorize_params: {
      application_id: ENV['AMAZON_SP_API_APPLICATION_ID'],
      version: 'beta', # If you app is in Draft state, this is required.
    },
  }
```

Then update your `User` model:

```ruby
  devise(
    # Other Devise options
    omniauth_providers: [:amazon_sp_api],
  )
```
