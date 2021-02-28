---
layout: post
title: "CRUD Delete"
image: /assets/images/posts/Ruby on Rails/CRUD/CRUD Delete/2017-07-18 00.33.07.png
categories: "Ruby_on_Rails"
tags:
  - Ruby on Rails
  - CRUD
---

## CRUD Delete

```ruby
#delete
get '/posts/delete/:post_id' => 'posts#delete'

```
- 어떠한 글을 지워야하는지 알 수 없음
		
		- 지워야하는 글의 아이디를 받아 `delete method`로.
		
		---
		
		![](/assets/images/posts/Ruby on Rails/CRUD/CRUD Delete/2017-07-18 00.33.07.png)
		지우고 싶은 글의 아이디를 받아서 넘김.
		
		- - -

```ruby
def delete
	@post = Post.find(params[:post_id])
	@post.destroy

	redirect_to '/'
end
```
해당 데이터베이스를 찾아서 destroy를 실행하고 다시 메인으로 돌아옴.

![](/assets/images/posts/Ruby on Rails/CRUD/CRUD Delete/2017-07-18 00.39.07.png)

삭제할것인지 경고문을 띄움.
