---
layout: post
title: "CRUD Update"
image: /assets/images/posts/Ruby on Rails/CRUD/CRUD Read/2017-07-18 00.11.51.png
categories: "Ruby_on_Rails"
tags:
  - Ruby on Rails
  - CRUD
---

## CRUD Update

### 하나의 포스트를 수정하는 작업

```ruby
get ‘posts/edit/:post_id’ => ‘posts#edit’
```
몇번 글에 대해서 수정할 창으로 보낼 것인지?

```ruby
def edit 
	@editPost = Post.find(params[:post_id])
end
```

수정을 찾아서 editPost에 저장
- - - -
action에서 원하는 포스트를 찾아 수정해주는 것을 해야함.
![](/assets/images/posts/Ruby on Rails/CRUD/CRUD Update/2017-07-18 00.49.30.png)

```ruby
get ‘/posts/update/:post_id’ => ‘posts#update’
```

```ruby
def update
	@updatePost = Post.find(params[:post_id])
	@updatePost.title = params[:input_title]
	@updatePost.content = params[:input_content]
	@updatePost.save

	redirect_to “/posts/show/#{@updatePost.id}”

end
```
해당 아이디의 타이틀과 컨텐츠를 수정 후 세이브하고 수정된 데이터의 id 목록으로 리다이렉트함.

![](/assets/images/posts/Ruby on Rails/CRUD/CRUD Update/2017-07-18 01.03.42.png)
수정하기 전의 값이 창에 입력되어있는 상태로 하려면 이렇게.

Ruby\ on\ Rails/CRUD