---
layout: post 
title: Soft Delete In Rails
keywords: soft,delete,ruby,rails
tags: rails 
description: Remove an item from client, but not from the database.
---
<h3>Soft delete in rails</h3>
<p>软删除：物理上不删除数据，只是把数据隐藏起来。</p>
<h3>Paranoia</h3>
<p>Paranoia：实现软删除的gem</p>
<p><b>Paranoia使用:</b></p>
<p>添加gem:</p>

<pre>
 gem 'paranoia', :github => 'radar/paranoia', :branch => 'rails4'
</pre>

<p>Add column: deleted_at</p>

<pre>
 rails g migration add_deleted_at_to_models deleted_at:datetime:index
</pre>

<p>得到相应Migraion:</p>

<pre>
 class AddDeletedAtToClients &lt; ActiveRecord::Migration
   def change
     add_column :clients, :deleted_at, :datetime
     add_index :clients, :deleted_at
   end
 end
</pre>

<p>在相应model添加：</p>

<pre>
 class Client &lt; ActiveRecord::Base
   acts_as_paranoid
   ...
 end
</pre>

<p>软删除实例对象：</p>

<pre>
 @model.destroy 
</pre>

<p>查找所有records:</p> 

<pre>
 Model.with_deleted 
</pre>

<p>查找删除的records:</p>

<pre>
 Model.only_deleted 
</pre>

<p>查看是否软删除：</p>

<pre>
 @model.destroyed? 
</pre>

<h3>参考链接</h3>

<pre>
    <a href="https://github.com/radar/paranoia">https://github.com/radar/paranoia</a>
</pre>
