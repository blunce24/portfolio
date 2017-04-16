---
layout: post
title: Blocipedia
thumbnail-path: "img/BlocipediaLanding.png"
short-description: Blocipedia is a Wikipedia replica built with Ruby on Rails via test driven development.

---

{:.center}
![]({{ site.baseurl }}/img/BlocipediaLanding.png)

## Explanation

For my second Rails project, I set out to build [Blocipedia](https://github.com/blunce24/blocipedia), a replica of Wikipedia, with the guidance of Bloc. The application was a vehicle to learn back-end web programming with [Ruby on Rails](http://rubyonrails.org) via test driven development. I learned a lot through this process and encountered some interesting challenges.

## Problem

Blocipedia is a standard CRUD application designed to replicate Wikipedia. Other than the standard Create, Read, Update and Delete functions, which were defined via the Wiki model, I wanted to build the application with robust user authentication and authorization services. In addition, I wanted the user to be able to upgrade their account, reflecting a Freemium model application. I was able to add these attributes via third-party gems.

## Solution

**User Authentication with the Devise Gem**

* I included the [Devise](https://github.com/plataformatec/devise) gem to manage user authentication in the application.
{% highlight ruby %}
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable, :confirmable

  has_many :wikis, dependent: :destroy
  has_many :collaborators, dependent: :destroy

  after_initialize { self.role ||= :standard }

  enum role: [:standard, :premium, :admin]

end
{% endhighlight %}

**User Authorization with the Pundit Gem**

* I included the [Pundit](https://github.com/elabs/pundit) gem to manage authorization and user roles in the application.
{% highlight ruby %}
class WikiPolicy < ApplicationPolicy
  class Scope
    attr_reader :user, :scope

    def initialize(user, scope)
      @user = user
      @scope = scope
    end

    def resolve
      wikis = []
      if user.role == 'admin'
        wikis = scope.all
      elsif user.role == 'premium'
        all_wikis = scope.all
        all_wikis.each do |wiki|
          if !wiki.private || wiki.user == user || wiki.collaborators.exists?(user_id: user.id)
            wikis << wiki
          end
        end
      else
        all_wikis = scope.all
        all_wikis.each do |wiki|
          if !wiki.private || wiki.collaborators.exists?(user_id: user.id)
            wikis << wiki
          end
        end
      end
      wikis
    end
  end
end
{% endhighlight %}

**Incorporated Stripe Gem to Upgrade Accounts**

* I included the [Stripe](https://github.com/stripe/stripe-ruby) gem to offer an upgrade account feature.

{:.center}
![]({{ site.baseurl }}/img/Stripe.png)

## Results

{:.center}
![]({{ site.baseurl }}/img/WikiIndex.png)

Through the help of several gems, I built a Wikipedia replica with basic CRUD capabilities. It contains a robust user authentication method, as well as a detailed user authorization method. It incorporates Stripe to allow user upgrades. I also was able to utilize test driven development while constructing this application:

{% highlight ruby %}
require 'rails_helper'
include RandomData

RSpec.describe Wiki, type: :model do
  before do
    @user = User.new(email: "example@example.com", password: "helloworld")
    @user.skip_confirmation!
    @user.save
  end

  let(:title) { RandomData.random_sentence }
  let(:body) { RandomData.random_paragraph }
  let(:wiki) { Wiki.create!(title: title, body: body, private: false, user: @user) }

  it { is_expected.to belong_to(:user) }
  it { is_expected.to have_many(:collaborators)}

  describe "attributes" do
    it "has title and body attributes" do
      expect(wiki).to have_attributes(title: wiki.title, body: wiki.body, private: wiki.private, user: wiki.user)
    end
  end
end
{% endhighlight %}

## Conclusion

I really enjoyed learning how to construct web applications using Ruby on Rails. The framework is easy to learn and is incredibly fast to produce. Rails provides a wonderful platform for learning back-end web programming.
