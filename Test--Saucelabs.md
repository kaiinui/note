Saucelabs
===

色々用意されたブラウザを用いてテストすることが出来る。
テストは Selenium. 実際的には、例えば Ruby なら Capybara 等でテストすることになる。

```rb
// sauce_helper.rb

require "sauce"
require "sauce/capybara"

# Set up configuration
Sauce.config do |c|
  c[:browsers] = [
      ["Windows 8", "Internet Explorer", "10"],
      ["Windows 7", "Firefox", "20"],
      ["OS X 10.8", "Safari", "6"]
  ]
end
```

```rb
// spec_helper.rb

require 'capybara/rspec'
include Capybara::DSL

require_relative 'sauce_helper'

Capybara.default_driver = :sauce
```

```rb
// feature/request_spec.rb

require_relative 'spec_helper'

describe "Request", :sauce => true do
  it "should show /" do
    visit "http://example.com/"
    expect(page).to have_css("img") # Assume it has the Logo
  end
end
```

みたいな感じでやる。

実際的には Request Spec しても仕方ないので、Screenshot を撮って比較する。

テストしたい主眼は、

1. Responsive Web Design (ちゃんと意図通りに Fallback 出来ているか / デザインが崩れていないか)
2. JavaScript のブラウザ互換性

References
---

- Sauce Labs: Selenium Testing, Mobile Testing, JS Unit Testing and More : https://saucelabs.com/
