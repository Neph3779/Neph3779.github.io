---
layout: post
title: "CRUD Create"
image:
categories: "Ruby_on_Rails"
tags:
  - Ruby on Rails
  - CRUD
---

## CRUD Create

#### gem ‘rails_db’

+ db를생성하고, 서비스 내의 데이터를 웹으로 시각화하여 볼 수 있는 젬

  ---

```ruby
class CreatePosts < ActiveRecord::Migration
	def change
		create_table :posts do |t|
			t.string :title
			t.text :content
			t.timestamps null: false
		end
	end
end
```
제목과 컨텐츠를 생성하여 데이터베이스에 저장하려고 하는 것임.
이후 `rake db:migrate`를 하여 앱에 올려주어야함.

- - - -
컨트롤러를 생성. 메소드 이름과 같은 이름의 뷰파일을 만들어주고, route설정이 필요.

![](/assets/images/posts/Ruby on Rails/CRUD/CRUD Create/2017-07-17 23.55.59.png)
```html
<form class=“” action=“/posts/create” method=“get”> 
…
</form>
```
`action`을 통해 어떤 메소드로 보내줄 것인지 중요.

- - - -

```ruby
def create
	@post = Post.new 
	@post.title = params[:input_title]
	@post.content = params[:input_content]
	@post.save
	
	redirect_to ‘/posts/new’
end
```
`input`태그의 `name`과 같은 파라미터값을 넘겨주어야함.

- - - -
수정할 때에도 어떤 게시글을 수정하고 싶은지에 대한 아이디가 필요함.

```ruby
def edit
	@post = Post.find(params[:post_id])
end

def update
	@post = Post.find(params[:post_id])
	@post.title = params[:input_title]
	@post.content = params[:input_content]
	@post.save

	redirect_to “/posts/show/#{@post.id}”
end
```


