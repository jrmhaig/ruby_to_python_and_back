# Templated HTML pages

Templates for HTML pages are created to keep them separate from the application logic.

* [Rails](#rails)
* [Django](#django)
* [Differences between Ruby, Rails and Python](#differences-between-ruby-rails-and-python)

## Rails

To add Ruby to a template use;

* `<% %>` if the output should not be included in the HTML
* `<%= %>` if the output of the command is to be included in the HTML

```ruby
# In the controller
@page_subtitle = 'The subtitle of the page'
@some_data = [
  { key: 'one', value: 'Dog' },
  { key: 'two', value: 'Cat' }
]
render :template_to_render
```

```erb
<!-- In the view -->
<h1>An example with Rails with ERB</h1>

<h2><%= @page_subtitle %><h2>

<ul>
  <% @some_data.each do |row| %>
    <li>The key is <%= row[:key] %> and the value is <%= row[:value] %></li>
  <% end %>
</ul>
```

## Django

To embed code to a template;

* `{% %}` for a structural element
* `{{ }}` for a value to be included in the HTML

```python
# In the view
return render(
    request,
    'template_to_display.html',
    {
        'page_subtitle': 'The subtitle of the page',
        'some_data': [
            { 'key': 'one', 'value': 'Dog' },
            { 'key': 'two', 'value': 'Cat' }
        ]
    }
)
```

```html
<!-- In the view -->
<h1>An example with Django</h1>

<h2>{{ page_subtitle }}<h2>

<ul>
  {% for row in some_data %}
    <li>The key is {{ row.key }} and the value is {{ row.value }}</li>
  <% endfor %>
</ul>
```

## Differences between Ruby, Rails and Python

| Rails | Django |
|---|---|
| Templates are called 'views' | Templates are called 'templates'
| Templates and are usually stored in the `app/views` directory | Templates are usually stored in the `templates` directory of the individual application, such as `project_name/apps/app_name/templates`
| Templates may be written as HTML using ERB (Embedded Ruby) or a templating language such as Haml | Templates are written as HTML with snippets of the 'Django template language'
| Ruby can be used directly in views | Django provides a separate template language
| Templates have the file extension `.html.erb` or `.html.haml` | Templates have the file extension `.html`