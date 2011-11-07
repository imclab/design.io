# The Watchfile

## Complete Example

``` coffeescript
Shift = require 'shift'
_path = require 'path'
fs    = require 'fs'

# ignorePaths "./tmp"

# watcher "assets", ->
#   watch ".styl"

watch ".styl", ".less", ".sass", ".css"
  create: (path) ->
    @update(path)
    
  update: (path) ->
    self = @
    
    fs.readFile path, 'utf-8', (error, result) ->
      engine = Shift.engine(_path.extname(path))
      
      if engine
        engine.render result, (error, result) ->
          return self.error(error) if error
          self.broadcast path: path, body: result, id: self.toId(path)
      else
        self.broadcast path: path, body: result, id: self.toId(path)
        
  delete: (path) ->
    @broadcast id: @id(path)
  
  render:
    update: (data) ->
      stylesheets = @stylesheets ||= {}
      stylesheets[data.id].remove() if stylesheets[data.id]?
      node = $("<style id='#{data.id}' type='text/css'>#{data.body}</style>")
      stylesheets[data.id] = node
      $("body").append(node)
      
    destroy: (data) ->
      $("##{data.id}").remove()

# if arity is 0
watch /\.(png|jpg)$/, ->
  server: (path) -> 
    @broadcast path: path, data: transform(path)

  client: (data) ->
    $("##{data.id}").attr("src", data.url)

# If arity is 1
watch /\.(png|jpg)$/, (path) ->
  @broadcast path: path, data: transform(path)
  
# If you don't want to notify the browser
watch /\.(png|jpg)$/, (path) ->
  transform(path)

watch /\.(png|jpg)$/, (path) -> path: path, data: transform(path)

```