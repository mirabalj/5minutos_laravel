###Facades del paquete `laravel/collective`.

```
    {!! Form::open(['url' => '/articles') !!}
		<div class="form-group">
			{!! Form::label('title', 'Title:') !!}
			{!! Form::text('title', null, ['class' => 'form-control' , 'otro' => 'prueba']) !!}
		</div>

		<div class="form-group">
			{!! Form::label('body', 'Body:') !!}
			{!! Form::textarea('body', null, ['class' => 'form-control']) !!}
		</div>

		<div class="form-group">
			{!! Form::label('published_at', 'Publish On:') !!}
			{!! Form::input('date', 'published_at', $article->published_at, ['class' => 'form-control']) !!}
		</div>

		<div class="form-group">
			{!! Form::label('tag_list', 'Tags:') !!}
			{!! Form::select('tag_list[]', $tags, null, ['id' => 'tag_list', 'class' => 'form-control', 'multiple']) !!}
		</div>

		<div class="form-group">
			{!! Form::submit('Add Article', ['class' => 'btn btn-primary form-control']) !!}
		</div>	
    {!! Form::close() !!}
```

```
	{!! Form::model($article = new App\Article ,['url' => 'articles']) !!}
```

```
		<div class="form-group">
			{!! Form::submit($submitButtonText, ['class' => 'btn btn-primary form-control']) !!}
		</div>	
```	

#ToDo:http://www.cazaplanetas.com/webmaster/otra-forma-de-usar-laravel-collective/