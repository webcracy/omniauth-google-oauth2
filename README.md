# OmniAuth Google OAuth2 Strategy

This fork adds support for legacy OpenID ids to the omniauth-google-oauth2 gem (https://github.com/zquestz/omniauth-google-oauth2).

**Important**: The current version breaks compatibility with the original code. Automatic scope completion was removed and specs were changed to reflect this. Work around by using full scope URLs.

## Configuration

Example with Rails

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :google_oauth2, ENV["GOOGLE_KEY"], ENV["GOOGLE_SECRET"],
    {
      :name => "google",
      :scope => "openid, https://www.googleapis.com/auth/userinfo.email, https://www.googleapis.com/auth/userinfo.profile, https://www.googleapis.com/auth/chromewebstore.readonly",
      :openid_legacy => true # will set `openid.realm` to value of `callback_url`
      # 'openid.realm' => 'http://yourmanualsetting.com'
    }
end
```

## Auth Hash

The user's `openid_id` will be available in the auth hash at `request.env["omniauth.auth"]["extra"]["raw_openid_info"]["openid_id"]`. 

```ruby
{
    :provider => "google_oauth2",
    :uid => "123456789",
    :info => {
        :name => "John Doe",
        :email => "john@company_name.com",
        :first_name => "John",
        :last_name => "Doe",
        :image => "https://lh3.googleusercontent.com/url/photo.jpg"
    },
    :credentials => {
        :token => "token",
        :refresh_token => "another_token",
        :expires_at => 1354920555,
        :expires => true
    },
    :extra => {
        :raw_openid_info => {
          :openid_id => "https://www.google.com/accounts/o8/id?id=someIdString",
          ...
        },
        :raw_info => {
            ...
        }
    }
}

```

## More info

- https://developers.google.com/+/api/auth-migration#userinfo
- http://www.tbray.org/ongoing/When/201x/2013/04/04/ID-Tokens


## Credits

- omniauth-google-oauth2 (https://github.com/zquestz/omniauth-google-oauth2) by Josh Ellithorpe 
- current modifications by Alex Solleiro (https://github.com/webcracy)


## License

Copyright (c) 2013 by Josh Ellithorpe

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
