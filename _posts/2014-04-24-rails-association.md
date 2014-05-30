---
layout: post 
title: Polymorphic many-to-many associations in Rails 
keywords: polymorphic, associations 
tags: rails 
description: Setting up polymorphic many-to-many associations in Rails is more difficult, but only slightly.
---
<h3>understand :source option of has_one/has_many through</h3>
<p> Sometimes, you want to use different names for different associations. If the name you want to use for an association on the model is not the same as the association on the :through model, you can use :source to specify it.</p>
<p>&nbsp;</p>
<b>Here is an example</b>
<p>Assume we have three models: Pet, Dog and Dog::Breed</p>

<pre>
class Pet < ActiveRecord::Base
  has_many :dogs
end

class Dog < ActiveRecord::Base
  belongs_to :pet
  has_many :breeds
end

class Dog::Breed < ActiveRecord::Base
  belongs_to :dog
end
</pre>

<p>In this case, we have chosen to namespace the Dog::Breed, because we want to access Dog.find(123).breeds as a nice and convenient association.</p>
<p>&nbsp;</p>
<p>Now, if we now want to create a has_many :dog_breeds, :through => :dogs association on Pet, we suddenly have a problem. Rails won't be able to find a :dog_breeds association on Dog, so Rails can't possibly know which Dog association you want to use.</p>
<b>Enter :source</b>

<pre>
class Pet < ActiveRecord::Base
  has_many :dogs
  has_many :dog_breeds, :through => :dogs, :source => :breeds
end
</pre>

<p>With :source, we are telling Rails to look for an association called :breeds on the Dog model, and use that.
</p>

<h3>understand :source_type option of Polymorphic many-to-many associations</h3>
<p>Assume we have models: Tag, Tagging, Book and Movie</p>

<pre>
class Tag < ActiveRecord::Base
  has_many :taggings, :dependent => :destroy
  has_many :books, :through => :tagings, :source => :taggable, :source_type => "Book"
  has_many :movies, :through => :tagings, :source => :taggable, :source_type => "Movie"     
end

class Tagging < ActiveRecord::Base
  belongs_to :taggable, :polymorphic => true
  belongs_to :tag
end

class Book < ActiveRecord::Base
  has_many :taggings, :as => :taggable
  has_many :tags, :through => :taggings
end
    
class Movie < ActiveRecord::Base
  has_many :taggings, :as => :taggable
  has_many :tags, :through => :taggings
end
</pre>

<p>From the above code, we know that :source_type option is used only when :source option can not specify the target model in polymorphic</p>

<p>&nbsp;</p>
<b>Reference:</b> 

<pre>
<a href="http://www.brentmc79.com/posts/polymorphic-many-to-many-associations-in-rails">http://www.brentmc79.com/posts/polymorphic-many-to-many-associations-in-rails</a>
<a href="http://stackoverflow.com/questions/9500922/need-help-to-understand-source-type-option-of-has-one-has-many-through-of-rails">http://stackoverflow.com/questions/9500922/need-help-to-understand-source-type-option-of-has-one-has-many-through-of-rails</a>
</pre>
