Create new rails app
`bundle exec railties/exe/rails new ../openipsum-api --api --edge`

Update bundle

```
source 'https://rubygems.org'

gem 'rails', github: "rails/rails"
gem 'sprockets-rails', github: "rails/sprockets-rails"
gem 'sprockets', github: "rails/sprockets"
gem 'sass-rails', github: "rails/sass-rails"
gem 'arel', github: "rails/arel"
gem 'rack', github: "rack/rack"
gem 'sqlite3'
gem 'active_model_serializers', '~> 0.10.0.rc2'
gem 'rack-cors'
gem 'devise', github: 'twalpole/devise', branch: 'rails5'

group :development, :test do
  gem 'pry-rails'
  gem 'faker'
  gem 'byebug'
end

group :development do
  gem 'spring'
end

gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

Install devise
`rails g devise:install`


```
# before_action :authenticate_user_from_token!
  # respond_to :json


  # def authenticate_user_from_token!
  #   auth_token = request.headers['Authorization']

  #   if !auth_token
  #     return auth_error
  #   end

  #   unless auth_token.include? 'Bearer: '
  #     auth_error
  #   end

  #   token = auth_token.split('Bearer: ').last
  #   user = User.find_by_authentication_token token

  #   if !user
  #     return auth_error
  #   end

  #   sign_in user, store: false

  # end
  ```

```
  before_save :ensure_authentication_token

  def ensure_authentication_token
    if authentication_token.blank?
      self.authentication_token = generate_authentication_token
    end
  end

  private

    def generate_authentication_token
      loop do
        token = Devise.friendly_token
        break token unless User.where(authentication_token: token).first
      end
    end
```
